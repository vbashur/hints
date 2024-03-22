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

## starting a container
* on launching system container you gotta provide an application name to have it run a default command as well, e.g.  
```shell
docker run -it busybox /bin/sh
```
* on running an application image this is not required, e.g.  
```shell
docker run -d nginx  # -d stands for running in  background
```

search container
```
docker search
```




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
