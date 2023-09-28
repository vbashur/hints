
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
