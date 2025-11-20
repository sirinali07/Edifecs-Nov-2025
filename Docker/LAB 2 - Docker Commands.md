### Task 1: Creating your first Docker container
Pull the image from **DockerHub** to the Local repo and start the container
```
docker container run hello-world  
```
```
docker ps -a
```
### Task 2: Basic Commands to run the Container in Interactive mode
Download the image from Docker Hub to the Local repo
```
docker image pull ubuntu
```
List all the images in the local repo
```
docker image ls
```
List all running processes related to containers
```
docker ps 
```
List all processes related to containers including exited or terminated
```
docker ps -a
```
Run the container in interactive mode
```
docker container run -it --name ct1 ubuntu
```
create files inside the container
```
touch f1 f2 f3
```
```
ls
```
Press Crtl+P+Q to switch the terminal to Docker Host.
```
docker ps
```
Now create another container in interactive mode
```
docker run -it --name ct2 ubuntu
```
```
exit
```
```
docker ps -a
```
Note: You may have observed that when you press CTRL+P+Q, the Docker container continues to run in the background. However, if you type exit inside the container, it terminates the primary process, causing the container to stop.

### Task 3: Basic Commands to run the Container in Detach mode 

Create the container in detach mode
```
docker container run -d --name ct3 nginx
```
```
docker ps -a
```
```
docker container exec -it ct3 /bin/bash
```
```
apt update && apt install -y wget curl
```
```
apt install procps
```
```
ps aux
```
```
exit
```
```
docker ps -a
```

