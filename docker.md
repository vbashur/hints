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
# run container in foreground, press ctrl+p or ctrl-q to disconnect
docker run -it busybox /bin/sh
```
* on running an application image this is not required, e.g.  
```shell
# run container in background
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
docker run -it --rm wrmck bash  # rm - remove the container after running
```

Stop running containers
```
docker stop `docker ps -q`
```

## inspecting container

get the info about running containers with resources utilization
```shell
docker stats
```

get the detailed information about container parameters: default command, keys, creation date etc
```shell
docker inspect <image>
```

## Useful commands over the running containers

runnning the shell inside the container
```shell
docker exec -it <container name> <command name, e.g. bash>
```

```
