## LAB - Port Mapping from Docker Host to container

Run the following command to install Apache HTTP Server, and mapping the port 80 on the host to port 80 in the container.
```
docker run -d -p 80:80 httpd
```
The `-p` option maps port 80 of the Docker host to port 80 of the container.

List all running containers.
```
docker ps -a
```
Access the Container’s Shell
```
docker exec -it < replace container id/name > /bin/bash
```
modify the webpage and exit from the container
```
echo "Hello, This is Port Mapping testing" > /usr/local/apache2/htdocs/index.html
```

```
exit
```
Verify the webpage in the browser
copy the `public IP` of the Docker Host VM in the browser with `Host Port`
* `http://<Public IP>`:`<Host Port>`
* http://54.193.64.107:8080/
```
docker ps -a
```
Stop the container.
```
docker container stop < replace container id/name >
```
Delete the container.
```
docker container rm < replace container id/name >
```
Lists all the images.
```
docker image ls
```
Delete the image from your system.
```
docker image rm < replace image id >
```

