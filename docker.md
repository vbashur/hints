# Containers and docker

## container 

container is an application with all dependencies included

container runtime - application that hosts the containers

There are two types of containers:
- system container - used as a foundation to build your own container images
- application container - used to start just one application - it has default application to start

_container vs image_  
container is a running instance of an image  
image contains application code, lang runtime, libs etc




Create an image from Dockerfile
```
 docker build -t wrmck .
```


Run an image as container

```
docker run -it wrmck bash
```

Stop running containers
```
docker stop `docker ps -q`
```
