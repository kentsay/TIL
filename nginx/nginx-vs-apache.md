# Nginx vs Aapche

### Connection Handling Architecture
One big difference between Apache and Nginx is the actual way that they handle connections and traffic. This provides perhaps the most significant difference in the way that they respond to different traffic conditions.

#### Apache
Apache provides a variety of multi-processing modules (Apache calls these MPMs) that dictate how client requests are handled. Basically, this allows administrators to swap out its connection handling architecture easily. These are:

* mpm_prefork: This processing module spawns processes with a single thread each to handle request. Each child can handle a single connection at a time. As long as the number of requests is fewer than the number of processes, this MPM is very fast. However, performance degrades quickly after the requests surpass the number of processes, so this is not a good choice in many scenarios. Each process has a significant impact on RAM consumption, so this MPM is difficult to scale effectively. This may still be a good choice though if used in conjunction with other components that are not built with threads in mind. For instance, PHP is not thread-safe, so this MPM is recommended as the only safe way of working with mod_php, the Apache module for processing these files.

* mpm_worker: This module spawns processes that can each manage multiple threads. Each of these threads can handle a single connection. Threads are much more efficient than processes, which means that this MPM scales better than the prefork MPM. Since there are more threads than processes, this also means that new connections can immediately take a free thread instead of having to wait for a free process.

* mpm_event: This module is similar to the worker module in most situations, but is optimized to handle keep-alive connections. When using the worker MPM, a connection will hold a thread regardless of whether a request is actively being made for as long as the connection is kept alive. The event MPM handles keep alive connections by setting aside dedicated threads for handling keep alive connections and passing active requests off to other threads. This keeps the module from getting bogged down by keep-alive requests, allowing for faster execution. This was marked stable with the release of Apache 2.4.
* 
As you can see, Apache provides a flexible architecture for choosing different connection and request handling algorithms. The choices provided are mainly a function of the server's evolution and the increasing need for concurrency as the internet landscape has changed.

#### Nginx
Nginx came onto the scene after Apache, with more awareness of the concurrency problems that would face sites at scale. Leveraging this knowledge, Nginx was designed from the ground up to use an asynchronous, non-blocking, event-driven connection handling algorithm.

Nginx spawns worker processes, each of which can handle thousands of connections. The worker processes accomplish this by implementing a fast looping mechanism that continuously checks for and processes events. Decoupling actual work from connections allows each worker to concern itself with a connection only when a new event has been triggered.

Each of the connections handled by the worker are placed within the event loop where they exist with other connections. Within the loop, events are processed asynchronously, allowing work to be handled in a non-blocking manner. When the connection closes, it is removed from the loop.

This style of connection processing allows Nginx to scale incredibly far with limited resources. Since the server is single-threaded and processes are not spawned to handle each new connection, the memory and CPU usage tends to stay relatively consistent, even at times of heavy load.

### Reference
* [Apache vs Nginx: Practical Considerations](https://www.digitalocean.com/community/tutorials/apache-vs-nginx-practical-considerations)
