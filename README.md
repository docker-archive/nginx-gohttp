# nginx-gohttp
Compose app skeleton with two services: frontend and backend. 
    - frontend - nginx server that will forward requests to the backend (its port 80 is mapped by docker-compose to 8080 on the localhost). To query it, we use the localhost:8080 endpoint.
    - backend - Go http server which servers an ascii drawing and greeting message.

Project structure
```$ tree hello-docker
hello-docker
├── backend
│   ├── Dockerfile
│   └── main.go
├── docker-compose.yml
└── frontend
    ├── Dockerfile
    └── nginx.conf
```

Deploy it with docker-compose:
```
$ docker-compose up -d
Creating network "nginx-gohttp_default" with the default driver
Building frontend
Step 1/2 : FROM nginx:alpine
 ---> 48c8a7c47625
Step 2/2 : COPY nginx.conf /etc/nginx/conf.d/default.conf
 ---> Using cache
 ---> 9d5f008155e5

Successfully built 9d5f008155e5
Successfully tagged nginx-gohttp_frontend:latest
WARNING: Image for service frontend was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating nginx-gohttp_backend_1 ... done
Creating nginx-gohttp_frontend_1 ... done
```
Check containers are running
```
$ docker-compose ps
         Name                    Command           State          Ports        
-------------------------------------------------------------------------------
nginx-gohttp_backend_1    /usr/local/bin/backend   Up                          
nginx-gohttp_frontend_1   nginx -g daemon off;     Up      0.0.0.0:8080->80/tcp

```

The Compose app was deployed successfully if we can query the localhost on port 8080 and get the following messsage
```
$ curl localhost:8080

          ##         .
    ## ## ##        ==
 ## ## ## ## ##    ===
/"""""""""""""""""\___/ ===
{                       /  ===-
\______ O           __/
 \    \         __/
  \____\_______/

	
Hello from Docker!
```

Stop and remove containers
```
$ docker-compose down
Stopping nginx-gohttp_frontend_1 ... done
Stopping nginx-gohttp_backend_1  ... done
Removing nginx-gohttp_frontend_1 ... done
Removing nginx-gohttp_backend_1  ... done
Removing network nginx-gohttp_default
```
