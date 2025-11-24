## Volume Mount in Docker
### Checking Volumes in Docker Area
```
sudo ls -l  /var/lib/docker/volumes/
```

### Task 1: Creating a new docker volume and inspecting containers
```
docker volume create ct-volume1
```
```
docker volume ls
```
```
docker volume inspect ct-volume1
```

```
sudo ls -l  /var/lib/docker/volumes/
```
### Task 2: Launching a Nginx container mapped to a specific docker volume and verification
```
docker run -d -p 80:80 --name=nginx-container --mount src=ct-volume1,dst=/usr/share/nginx/html nginx
```
```
docker ps
```
```
docker container inspect nginx-container
```
```
sudo ls /var/lib/docker/volumes/ct-volume1/_data/ 
```
```
sudo touch  /var/lib/docker/volumes/ct-volume1/_data/{f1,f2,f3}
```

```
sudo vi /var/lib/docker/volumes/ct-volume1/_data/index.html
```
```
sudo ls -l  /var/lib/docker/volumes/
```
### Task 3: Deleting container and attaching the volume to another container

```
docker container stop nginx-container
```
```
docker rm container nginx-container
```
```
docker ps -a
```
```
docker run -it --name busybox-container1 --mount source=ct-volume1,target=/data busybox sh
```
```
ls
```
```
cd /data
```
```
ls
```
```
exit
```
```
docker stop busybox-container1
```
```
docker rm busybox-container1
```
```
docker ps -a
```
```
docker volume ls
```
```
docker volume rm ct-volume1
```
```
docker volume ls
```
