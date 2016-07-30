# Python uWSGI applications
This is a note about how to write and deploy an Python WSGI application and put it behind a full webserver.

### WSGI application and deploy

Let's start with a very simple example `test.py`:

```Python
def application(env, start_response):
  start_response('200 OK', [('Content-Type','text/html')])
  return ["<h1 style='color:blue'>Hello There!</h1>"]
```

Now you can start uWSGI to run an HTTP server and passing requests to your uWSGI application:

`uwsgi --http :8080 --wsgi-file test.py`

There are many parameters you can use to tune your application. For example: adding concurrency and monitors.

`uwsgi --http :8080 --wsgi-file test.py --master --processes 4 --threads 2`

In order not to type in such a long command, we can use a `test.ini` file to setup all your WSGI parameters:

```
[uwsgi]
module = wsgi:application
socket = 127.0.0.1:3031
master = true
processes = 4
threads = 2
chdir = /home/kentsay/playground/bikeapi/
wsgi-file = wsgi.py
chmod-socket = 664
vacuum = true
die-on-term = true
```
Since we are designing this config for use with Nginx, we're also going to change from using a network port and use a Unix socket instead.

To start this application, you just need to run:
`uwsgi test.ini`

You can use netstat to check if the socket is listening now:

`netstat -lt | grep 3031`

### Putting behind a full webserver(Nginx)
Even though uWSGI HTTP router is solid and high-performance, you may want to put your application behind a fully-capable webserver.

uWSGI natively speaks HTTP, FastCGI, SCGI and its specific protocol named “uwsgi” (yes, wrong naming choice). The best performing protocol is obviously uwsgi, already supported by nginx and Cherokee (while various Apache modules are available).

We're now at the point where we can work on configuring Nginx as a reverse proxy. Nginx has the ability to proxy using the uwsgi protocol for communicating with uWSGI. This is a faster protocol than HTTP and will perform better. Now we create a new file within `sites-available` directory under Nginx config. For example:

```
server {
  listen 8080;

  location / {
    include      uwsgi_params;
    uwsgi_pass   127.0.0.1:3031;
  }
}
```
Enable the server configuration we just made by linking it to the sites-enabled directory:

`sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled
`

Check the configuration file for syntax errors:

`sudo service nginx configtest`

If it reports back that no problems were detected, restart the server to implement your changes:

`sudo service nginx restart`

That's all. 
