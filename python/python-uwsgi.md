# Python uWSGI applications

```Python
def application(env, start_response):
  start_response('200 OK', [('Content-Type','text/html')])
  return [b"Hello World"]
```
save it as `test.py`


Now start uWSGI to run an HTTP server passing requests to your uWSGI applcaiotn:
`uwsgi --http :9090 --wsgi-file test.py`
