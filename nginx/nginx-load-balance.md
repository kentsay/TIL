# nginx as HTTP load balancer

### Load balancing methods in nginx
The following load balancing mechanisms (or methods) are supported in nginx:

* round-robin — requests to the application servers are distributed in a round-robin fashion,
* least-connected — next request is assigned to the server with the least number of active connections,
* ip-hash — a hash-function is used to determine what server should be selected for the next request (based on the client’s IP address).

### Configuration
When the load balancing method is not specifically configured, it defaults to round-robin. You can change to other methods by adding the config like `ip_hash` or `least_conn`. All requests are proxied to the server group myapp1, and nginx applies HTTP load balancing to distribute the requests.

To configure load balancing for HTTPS instead of HTTP, just use “https” as the protocol.


Example:
```
http {
    upstream myapp1 {
        //ip_hash;
        //least_conn;
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://myapp1;
        }
    }
}
```

### Weighted load balancing
It is also possible to influence nginx load balancing algorithms even further by using server weights. When the weight parameter is specified for a server, the weight is accounted as part of the load balancing decision.

```
    upstream myapp1 {
        server srv1.example.com weight=3;
        server srv2.example.com;
        server srv3.example.com;
    }
```
With this configuration, every 5 new requests will be distributed across the application instances as the following: 3 requests will be directed to srv1, one request will go to srv2, and another one — to srv3.
