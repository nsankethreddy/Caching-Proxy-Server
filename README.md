Caching-Proxy-Server
=======================

We hosted a server (Proxy 0) which connects to 4 proxy servers (Proxy 1,2,3,4). The server, Proxy 0 takes requests from the client and forwards these to the list of proxy servers. These 4 proxy servers are load balanced with the round robin algorithm.   

The 4 proxy servers connect to www.espncricinfo.com and cache the response received locally. On subsequent requests, the relevant data is loaded from cache without making another request to espncricinfo.com.    

If there are changes to the website content at espncricinfo.com, the corresponding cache is updated.

Network Structure
==================

![Network Structure](media/Network-Layout.png)

Execution Details
==================
We have implimented in the caching server in 2 different approaches.

- Vanilla Nginx 
- Docker Approach

## Vanilla Nginx

In this method we bring up the caching proxy server in our own system.  

- Install Nginx in the server/Computer
- Copy the nginx.conf to the nginx folder on the system (Default Nginx location : /etc/nginx )
- Create a new caching directory called "cache" in the nginx folder (Default location : /etc/nginx/cache )
- Start the nginx server
- Connect to the hosted/loadbalanced/caching proxy server by navigating to ```localhost:8000```


## Docker Approch  

### Network Structure for the Docker approach
![Network Structure for Docker](media/Docker.png)

**Method 1 Description:** This method implements multiple containers running locally. It uses one server which the end user connects to and multiple proxy servers which load balance the incoming traffic. Each of these servers run in separate containers. If needed more proxy servers can be added.

**Method 2 Description:** This method uses a single container hosting one server (which the user contacts) and 4 proxy servers. This image can be pulled from dockerhub.

Method 1:
---------

In the current project directory run ```docker-compose up```

Then navigate to ```localhost:8000```

Method 2:
---------

Execute the following commands:
```
  docker pull skete/ccbdproxy
  docker run -p 8000:8000 skete/ccbdproxy
````
Then navigate to ```localhost:8000```