# Initializing the Docker Environment
- Install Docker
- Enable the Docker Daemon
- Configure User Permissions
- Run a Test Image


#1. Create shell script nginx+docker

```
#!/bin/bash
sudo yum update -y
sudo yum install epel-release
sudo amazon-linux-extras install nginx1 -y 
sudo systemctl enable nginx
sudo systemctl start nginx
sudo yum update -y
#install DOCKER
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
#Make docker auto-start
sudo chkconfig docker on
#Because you always need it....
sudo yum install -y git
#Reboot to verify it all loads fine on its own.
sudo reboot
```
<img src="https://user-images.githubusercontent.com/106797604/197916448-53d64a45-cd81-40f6-9675-ede23ad789ee.png" align="right">


![Screenshot_31](https://user-images.githubusercontent.com/106797604/197916658-ffd9950b-7b19-46a8-a2d9-c61bb61ba22f.png)






