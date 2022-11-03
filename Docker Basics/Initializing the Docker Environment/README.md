# Initializing the Docker Environment
- Install Docker
- Enable the Docker Daemon
- Configure User Permissions
- Run a Test Image


### Additional Resources
Scenario
Your company wants to migrate its main website to containers, to be more responsive to recent growth, and bursts in traffic. You have been tasked with leading the migration effort.

Your company's servers, and your local workstation, use CentOS. Where do you start?

By installing Docker on your local machine, you can test how containers work, and develop a plan for moving the website into containers.

Lab Goals
This lab has you practice installing Docker in three easy steps:

Install the docker daemon and CLI from the Docker repo.
Enable the Docker daemon.
Configure your account to use Docker without root permissions.
Once configuration is complete, run the hello-world container to ensure everything is set up properly.

## Installing Docker
### 1. Create shell script nginx+docker

```
#!/bin/bash
sudo yum update -y
sudo yum install epel-release
sudo amazon-linux-extras install nginx1 -y 
sudo systemctl enable nginx
sudo systemctl start nginx
sudo yum update -y
#Install DOCKER
sudo amazon-linux-extras install docker -y
sudo service docker start
sudo usermod -a -G docker ec2-user
#Make docker auto-start
sudo chkconfig docker on
#Because you always need it....
sudo yum install -y git
#Reboot to verify it all loads fine on its own.
sudo reboot
#Enable the Docker Daemon
sudo systemctl enable --now docker
#Configure User Permissions
#Add the lab user to the docker group
sudo usermod -aG docker ec2-user




```
<img src="https://user-images.githubusercontent.com/106797604/197916448-53d64a45-cd81-40f6-9675-ede23ad789ee.png" align="right">


![Screenshot_31](https://user-images.githubusercontent.com/106797604/197916658-ffd9950b-7b19-46a8-a2d9-c61bb61ba22f.png)

![Screenshot_32](https://user-images.githubusercontent.com/106797604/197918253-820bb900-cc09-415c-9df4-b8b38fd63d52.png)


# Run a Test Image
Using docker, run the hello-world image to verify that the environment is set up properly:
```
docker run hello-world
```

![Screenshot_33](https://user-images.githubusercontent.com/106797604/197918556-3842a5d2-1877-4cd7-b041-a0d5cf5a6cc3.png)



