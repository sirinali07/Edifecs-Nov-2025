# Lab : Creating an EC2 Instance in AWS and Installing Docker
To begin, log in to AWS Console.

### Task-1: Installing Docker on Ubuntu 20.04 operating system
* Manually Launch a `t2.micro` instance with OS version as `Ubuntu 22.04 LTS` in North Virginia (us-east-1) Region.
* Use tag "`Name : Docker`"
* Create a new Keypair with the Name `Docker-Keypair-YourName`
* In security groups, include ports `22 (SSH)` and `80 (HTTP)` 
* Configure Storage: 10 GiB
* Launch the Instance.
* Once Launched, Connect to the Instance using `MobaXterm` or `Putty` or `EC2 Instance Connect` with username "`ubuntu`".

Once the EC2 is ready, follow the below Commands to perform lab:
```
sudo hostnamectl set-hostname docker
bash
```
```
sudo apt update -y
```
```
sudo apt install curl -y
```
```
sudo curl -SSL https://get.docker.com/ | sh
```
To know more about docker  - [Install Docker Engine](https://docs.docker.com/engine/install/ubuntu/)
```
sudo service docker status   
```
OR
```
sudo systemctl status docker
```
Add your ubuntu user to the docker group to run **Docker** commands without sudo
```
sudo usermod -aG docker ubuntu
```
```
sudo reboot
```
```
docker --version
```
