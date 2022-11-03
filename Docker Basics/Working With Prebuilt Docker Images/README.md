# Working With Prebuilt Docker Images
![Screenshot_34](https://user-images.githubusercontent.com/106797604/197918953-b0ff7cbe-0616-4be9-83da-1f80cb2fd79e.png)

![Screenshot_35](https://user-images.githubusercontent.com/106797604/197919063-418ba6e2-8007-4fce-a2fa-2248a3a13aee.png)


### Introduction
After installation, the best way to familiarize yourself with Docker, is to run containers from a few prebuilt images.

In this lab, you will explore Docker Hub for images that will run a website. Once you find suitable images, you will get them into your development environment and begin experimenting. You will run, stop, and delete containers from those images. You will also learn how to use existing data in containers.

# Get and View httpd
In the Docker Instance, verify that docker is installed:
```
docker ps
```
Using docker, pull the httpd:2.4 image:
```
docker pull httpd:2.4
```
Run the image:
```
docker run --name httpd -p 8080:80 -d httpd:2.4
```
Check the status of the container:

```
docker ps
```
In a web browser, test connectivity to the container:
```
<PUBLIC_IP_ADDRESS>:8080
```


# Run a Copy of the Website in httpd
![Screenshot_36](https://user-images.githubusercontent.com/106797604/197922795-913274f7-158e-414d-9d97-48fd73cf67b1.png)
```
git clone https://github.com/linuxacademy/content-widget-factory-inc
cd content-widget-factory-inc
ll
cd web
ll
docker stop httpd
docker rm httpd
docker ps -a
docker run --name httpd -p 8080:80 -v $(pwd):/usr/local/apache2/htdocs:ro -d httpd:2.4
docker ps
<PUBLIC_IP_ADDRESS>:8080
```

![Screenshot_44](https://user-images.githubusercontent.com/106797604/199852777-1aba9336-0bae-4d44-ab6a-00f69438eb86.png)


![Screenshot_37](https://user-images.githubusercontent.com/106797604/197923722-cd53f710-3d46-4652-b4be-e7d93b66bf2a.png)

