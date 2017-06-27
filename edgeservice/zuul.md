### Netflix Zuul
Zuul is an edge service that provides dynamic routing, monitoring, resiliency, security, and more.

#### What's in an Edge service
An edge service is a component which is exposed to the public internet. It acts as a gateway to all other services, which we will refer to as platform services. For example, consider an Nginx reverse proxy in front of some web resource servers. Here, Nginx acts as an edge service by routing public HTTP requests to the appropriate platform service. 
The reverse proxy pattern makes sense if every one of your resource servers is hardened to receive raw internet traffic. But in the typical service-oriented architecture, implementing a simple reverse proxy would duplicate a substantial amount of security and reliability code across platform services. For example, we do not want to reimplement user authentication in each platform service. Instead, we factor these responsibilities directly into the edge service. It validates and authenticates incoming requests; it enforces rate limits to protect the platform from undue load, and it routes requests to the appropriate upstream services. Factoring these high-level features into the edge service is a huge win for platform simplicity.

#### Zuul
A Zuul edge service is made up of a series of such filters, each performing some action on an HTTP request and / or response before passing control along to the next filter. For example, a filter might add authentication information to request headers, write an error response to an invalid request, or forward a request to an upstream service for further processing. In this section, we will walk through an example Zuul filter to show you the simplicity of the pattern.

Zull filters can be consider of three categories: pre-filters, route-filters, and post-filters. Pre-filters run before the edge service routes a request; route-filters forward requests to upstream services; and post-filters run after the proxied service returns its response. By implementing the filter by yourself, you can reject bad requests, do reverse-proxy good ones, and reduce interservice traffice.


#### Reference
* [What's edge service and Netflix zull](https://tech.knewton.com/blog/2016/05/api-infrastructure-knewton-whats-edge-service/)
* [Netflix Zull](https://github.com/Netflix/zuul)
