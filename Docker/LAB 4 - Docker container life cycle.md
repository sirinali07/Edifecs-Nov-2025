## Docker container life cycle (Self Excercise)

### Task 1: Pull a Docker Image 
Pull the official httpd (Apache HTTP Server) Docker image
```
docker pull httpd
```
Verify that the image is downloaded
```
docker image ls
```
### Task 2: Create and Manage Containers
Create a container from the downloaded image
```
docker container create httpd
```
List all containers
```
docker container ls -a
```
Start the created container
```
docker container start < container id/name >
```
```
docker container ls
```
Stop the created container
```
docker container stop < container id/Name >
```
```
docker container ls -a
```
```
docker container start < container id/Name >
```
### Task 3 : Pause and Unpause Containers
Pause the container
```
docker container pause < container id/Name >
```
Verify the paused state
```
docker container ls -a
```
Unpause the container
```
docker container unpause < container id/Name >
```
Verify the unpaused state
```
docker container ls -a
```
### Task 4: Access the Container and Modify Its Contents
Access the container's bash shell
```
docker exec -it < container id/name > /bin/bash
```
Navigate to the **htdocs** directory ( `Note:` if using **httpd** image, use htdocs; if using **nginx**, navigate to /usr/share/nginx/html):
```
cd htdocs
```
Update the package list and install wget
```
apt update
```
```
apt install wget -y
```
Replace the default index.html file
```
rm index.html
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/docker-essentials/index.html
```
Exit the container
```
exit
```
### Task 5: Monitor Docker Container Resource Usage
Check the logs of the container
```
docker logs < container id/name >
```
Monitor real-time resource usage for running Docker containers

* docker stats         # Shows usage statistics for only running containers

* docker stats -a      # Shows usage for all containers
```
docker stats < container id/name > 
```
### Task 6: Clean Up Resources

```
docker container ls
```
Stop a running container
```
docker stop < replace container id/name >
```
Remove a stopped container
```
docker container rm < replace container id/name > 
```
Remove a running container forcefully
```
docker container rm < replace container id/name > -f
```
Remove unused images
```
docker image ls
```
```
docker image rm < replace image id/name > < replace image id/name >
```
```
docker image ls
```

