# Understand how uWSGI and Nginx works to Serve Python Apps

### Clarifying terms
* WSGI: Abbreviation of "Web Server Gateway Interface(WSGI)". A Python spec that defines a standard interface for communication between an application or framework and an application/web server.

* uWSGI: An application server container that aims to provide a full stack for developing and deploying web applications and services. The main component is an application server that can handle apps of different languages. It communicates with the application using the methods defined by the WSGI spec, and with other web servers over a variety of other protocols. This is the piece that translates requests from a conventional web server into a format that the application can process.

* uwsgi: A fast, binary protocol implemented by the uWSGI server to communicate with a more full-featured web server. It is the preferred way to speak to web servers that are proxying requests to uWSGI.

### How does it work
The web server (uWSGI) must have the ability to send requests to the application by triggering a defined "callable". The callable is simply an entry point into the application where the web server can call a function with some parameters. The expected parameters are a dictionary of environmental variables and a callable provided by the web server (uWSGI) component.

In response, the application returns an iterable that will be used to generate the body of the client response. It will also call the web server component callable that it received as a parameter.

In our example, Nginx will handle client requests and proxy them to the uWSGI server, which runs our python application.

client -> nginx (proxy to forward request)-> uWSGI(application server) -> python code

### Steps
* write python application
* write uWSGI configuration and deploy your service as daemon
* config Nginx to forward request by using uwsgi protocol to call uWSGI


### Reference
* [uWSGI](https://github.com/unbit/uwsgi)
* [Python WSGI quick start](http://uwsgi-docs.readthedocs.io/en/latest/WSGIquickstart.html)
* [How To Set Up uWSGI and Nginx to Serve Python Apps on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-uwsgi-and-nginx-to-serve-python-apps-on-ubuntu-14-04)
