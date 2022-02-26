# HAProxy

### I. What's HAProxy?
   HAProxy, short for High Availability Proxy, is an open source load balancing software for TCP/HTTP. It can run on Linux, Solaris and FreeBSD. Its main purpose is to improve the performance and reliability of the system by directing the load to other servers. HAProxy is also used at many famous companies such as GitHub, Imgur, Instagram,...
   1. Terms in HAProxy
    
   Access Control List (ACL): In load balancing, the ACL is used to check a condition and perform an action (such as selecting a server or blocking a request) based on the results of that check. Using ACLs allows creating an environment that can dynamically forward requests based on various factors that the user can customize easily.
      Ex:
        
           acl url_blog        src         /something
   Backend: The backend is a set of servers that HAProxy can forward requests to. The backend is configured in the backendHAProxy configuration file entry. In its simplest form, a backend can be installed by Set up an algorithm for load balancing (round-robin, least-connection, ...). A list of servers and their ports that can be used to receive requests from HAProxy. A backend can contain one or more servers, basically, the more servers in a backend, the higher the system's load capacity and performance will be. System reliability is also increased by this method because when a server is disconnected from the proxy, the servers can take the load on it.
HAProxy also allows a dedicated backup server, to be used when the servers are offline. This backup server will usually contain offline system messages or some other message so that users can know that the system is temporarily unavailable.
      Ex:
      
          backend web-backend
             balance leastconn
             mode http
             server backend-1 web-backend-1.example.com check
             server backend-2 web-backend-2.example.com check
             server backend-3 backup-backend.example.com check backup
    
          backend forum
              balance leastconn
              server forum-1 forum-1.example.com check
              server forum-2 forum-2.example.com check
              server forum-3 backup-forum.example.com check backup
  Frontend: The frontend is used to define how requests are routed to the backend. The frontend is defined in the frontendHAProxy configuration entry. Configurations for the frontend include:
      - A set of IP addresses and ports (e.g. 10.0.0.1:8080, *:443,...)
      - User-defined ACLs
      - Backend is used to receive requests
     Ex:
            
            frontend web
               bind 0.0.0.0
               default_backend web-backend

            frontend forum
               bind 0.0.0.0:8080
               default_backend forum
### II. Types of load balancers?
   2.1 No load balancing:
   This is the simplest form for a web application, often used for dev environments, testing when the number of users is small and there is no or no need to ensure the reliability of the application. With this model, users will connect directly to the web server at yourdomain.comand no load balancing is used. If the webserver fails, the user will no longer be able to connect to the web application.
      [![z3213029024879-c96d0c75e20fa08be1d170014679d9ea.jpg](https://i.postimg.cc/BbTcvcpk/z3213029024879-c96d0c75e20fa08be1d170014679d9ea.jpg)](https://postimg.cc/tngVS6LN)
   
   2.2 Load balancing at layer 4 (transport layer):
   The simplest way to be able to load balance across multiple servers is to use load balancing on layer 4. In this direction, requests will be routed based on IP address and port range (for example, a request to that address http:://www.example.com/somethingwill is redirected to the backend used to navigate for the domain example.comwith port 80)
      [![z3213035505042-f607f48e2719258d6b165344642e4ff4.jpg](https://i.postimg.cc/Bb0HRP3g/z3213035505042-f607f48e2719258d6b165344642e4ff4.jpg)](https://postimg.cc/sQTMhXvQ)
   
   2.3 Load balancing at layer 7 (application layer):
   Layer 7 load balancing is the most complex and also the most customizable. Using load balancing at layer 7, we can redirect requests based on the content of that request. With this type of load balancing, multiple backends can be used for a single domain and port.
   
  [![z3213038351228-59f5ff4b1876d8a3cb5ce569df69189d.jpg](https://i.postimg.cc/Qdy5hysV/z3213038351228-59f5ff4b1876d8a3cb5ce569df69189d.jpg)](https://postimg.cc/3k2WZ9kH)
