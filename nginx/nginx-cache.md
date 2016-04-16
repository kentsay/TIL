# nginx caching

### Use cases

The following use-cases are some popular ones for using Nginx as a cache server:

* Nginx can sit "in front of" web servers, which may be other Nginx installations or web applications. This is a typical use case for a Cache Server - it acts as a gateway to other web/application servers, similar to a load balancer.
  * Nginx caching can be used in conjunction with a load balancer.
  * Actually, Nginx can act as both a load balancer and a cache server!
* Nginx can also cache the results of requests proxied to FastCGI and uWSGI processes, in addition to other HTTP servers/listeners! A good use case is to cache the results from CMSes, where most users don't require the dynamic aspects of the site - they just want to see the content.

![cache use case](http://image.slidesharecdn.com/nginxcaching-141120212306-conversion-gate02/95/nginx-highperformance-caching-9-638.jpg?cb=1416560537)
image source: [here](http://www.slideshare.net/Nginx/nginx-highperformance-caching)

From the above use cases, a few components can be observed: the original server, the cache server, and a client. If we want to setup a caching server on nginx, we need to setup the configurations on both original server and cache server.

### Component settings

#### Original server

Example:
```
# Expire rules for static content

# cache.appcache, your document html and data
location ~* \.(?:manifest|appcache|html?|xml|json)$ {
  expires -1;
  # access_log logs/static.log; # I don't usually include a static log
}

# Feed
location ~* \.(?:rss|atom)$ {
  expires 1h;
  add_header Cache-Control "public";
}

# Media: images, icons, video, audio, HTC
location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|svgz|mp4|ogg|ogv|webm|htc)$ {
  expires 1M;
  access_log off;
  add_header Cache-Control "public";
}

# CSS and Javascript
location ~* \.(?:css|js)$ {
  expires 1y;
  access_log off;
  add_header Cache-Control "public";
}
```

#### Cache server

Example:
```
# Note that these are defined outside of the server block,
# altho they don't necessarily need to be
proxy_cache_path /tmp/nginx levels=1:2 keys_zone=my_zone:10m inactive=60m;
proxy_cache_key "$scheme$request_method$host$request_uri";

server {
    # Note that it's listening on port 80
    listen 80 default_server;
    root /var/www/;
    index index.html index.htm;

    server_name example.com www.example.com;

    charset utf-8;

    location / {
        proxy_cache my_zone;
        add_header X-Proxy-Cache $upstream_cache_status;

        include proxy_params;
        proxy_pass http://172.17.0.18:9000;
    }
}
```

#### Proxy Caching

Nginx's cache can also cache the results of FastCGI, uWSGI proxied requests and even the results of load balanced requests (requests sent "upstream"). This means we can cache the results of requests to our dynamic applications.

Example:
```
fastcgi_cache_path /tmp/cache levels=1:2 keys_zone=fideloper:100m inactive=60m;
fastcgi_cache_key "$scheme$request_method$host$request_uri";

server {

    # Boilerplay omitted

    set $no_cache 0;

    # Example: Don't cache admin area
    # Note: Conditionals are typically frowned upon :/
    if ($request_uri ~* "/(admin/)")
    {
        set $no_cache 1;
    }

    location ~ ^/(index)\.php(/|$) {
            fastcgi_cache fideloper;
            fastcgi_cache_valid 200 60m; # Only cache 200 responses, cache for 60 minutes
            fastcgi_cache_methods GET HEAD; # Only GET and HEAD methods apply
            add_header X-Fastcgi-Cache $upstream_cache_status;
            fastcgi_cache_bypass $no_cache;  # Don't pull from cache based on $no_cache
            fastcgi_no_cache $no_cache; # Don't save to cache based on $no_cache

            # Regular PHP-FPM stuff:
            include fastcgi.conf; # fastcgi_params for nginx < 1.6.1
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass unix:/var/run/php5-fpm.sock;
            fastcgi_index index.php;
            fastcgi_param LARA_ENV production;
    }
}

```

### Reference
* [https://serversforhackers.com/nginx-caching/](https://serversforhackers.com/nginx-caching/)
