# Handcrafting a Container Image
![Screenshot_39](https://user-images.githubusercontent.com/106797604/199668255-1518d101-9561-4a12-bc32-2e5a6b059c99.png)
![Screenshot_38](https://user-images.githubusercontent.com/106797604/199668259-8152262c-37d2-4be1-bb86-18b4b039f4ee.png)

# Introduction
If you run your website from a pre-built base image, it will require a manual process to set up the container each time it runs. For repeatability and scalability, the container, and your website code should be made into an image.

In this lab, you will start with a base webserver image, modify settings in the container for your website, and then create images from the container. You'll demonstrate the importance of small changes to your container, and how they affect your image. Lastly, you will use your new images to create containers to see your hard work in action.

## Scenario
Creating a container image by hand allows you to quickly make and test images using Linux commands you already know. It will allow you to automate part of the process of creating a purpose-built container from an existing image. This technique is also useful for troubleshooting problems in existing containers. You will be able to save the state of an existing container as an image, similar to a Virtual Machine snapshot, then run containers from that image to debug your problem. Debugging is not covered in this lab, but it is an important use of the techniques you are learning.

### 1.Get and Run the Base Image
Retrieve the httpd 2.4 image from Docker hub.
Start a container from the httpd image.

### 2.Install Tools and Code in the Container
Log in to the container.
Update the base image and install git.
Get website code from GitHub.
Remove the default index page and copy the website files to httpd's web serving directory.
Log out of the container.
![Screenshot_40](https://user-images.githubusercontent.com/106797604/199671016-3c225641-cd7e-4f15-89b3-45b70b92424a.png)


### 3.Create an Image from the Container
Find the template container's ID.
Create an image named widgetfactory with version v1 from the container.
 1. View the image information.
Note: You can use the container's name in place of its ID for docker commit. However, we will practice using the container ID in this lab.

### 4.Clean up the Template for a Second Version
1. Log in to the container.
2. Remove temporary files and installed utilities.
3. Log out of the container.
4. Find the template container's ID.
5. Create a new image named widgetfactory with version v2 from the container
6. View the image information.
7. Delete the v1 image since it is obsolete.
![Screenshot_41](https://user-images.githubusercontent.com/106797604/199675362-c6b2617b-fc7e-4c53-8a41-c6ce8a3258eb.png)


### 5.Run Multiple Containers from the Image
Start three containers from the widgetfactory:v2 image with different published web ports. The exposed ports should be in the 8000 to 8999 range.
View the running containers in Docker.
View the website from each container in a browser, using the three exposed web ports.

### Get and Run the Base Image
1. Retrieve the httpd image:
```
docker pull httpd:2.4
```
2. Run the image:
```
docker run --name webtemplate -d httpd:2.4
```
3.Check the status of the webtemplate container:
```
docker ps
```
### Install Tools and Code in the Container
Log in to the container:
docker exec -it webtemplate bash
Run apt update and install git
apt update && apt install git -y
Clone the website code from GitHub:
git clone  https://github.com/linuxacademy/content-widget-factory-inc.git /tmp/widget-factory-inc
Verify that the code was cloned successfully:
ls -l /tmp/widget-factory-inc/
List the files in the htdocs/ directory:
ls -l htdocs/
Remove the index.html file:
rm htdocs/index.html
Copy the webcode from /tmp/ to the htdocs/ folder:
cp -r /tmp/widget-factory-inc/web/* htdocs/
Verify that they were copied over successfully:
ls -l htdocs/
Exit the container:
exit

### Create an Image from the Container
1. Copy the Container ID:
```
docker ps
```
2. Create an image from the container:
```
docker commit <CONTAINER_ID> example/widgetfactory:v1
```
3. Verify that the image was created successfully:
```
docker images
```
4. Take note of the image size.

### Clean up the Template for a Second Version
1. Log in to the container:
```
docker exec -it webtemplate bash
```
2. Remove the cloned code from the /tmp/ directory:
```
rm -rf /tmp/widget-factory-inc/
```
3. Use apt to uninstall git and clean the cache:
 ```
 apt remove git -y && apt autoremove -y && apt clean 
```
4. Exit the container:
```
exit
```
5. Check the status of the container:
```
docker ps
```
6. Create an image from the updated container:
```
docker commit <CONTAINER_ID> example/widgetfactory:v2
```
7. Verify that both images are now running:
```
docker images  
```
8. Delete the v1 image:
```
docker rmi example/widgetfactory:v1
```
### Run Multiple Containers from the Image
1. Run multiple containers using the new image:
```
docker run -d --name web1 -p 8081:80 example/widgetfactory:v2
docker run -d --name web2 -p 8082:80 example/widgetfactory:v2
docker run -d --name web3 -p 8083:80 example/widgetfactory:v2
```
2. Check the status of the containers:
```
docker ps
```
3. Stop the base webtemplate image:
```
docker stop webtemplate
```
4. Verify that only the created containers are running:
```
docker ps
```
5. Using a web browser, verify that the containers are running successfully:
```
<SERVER_PUBLIC_IP_ADDRESS>:8081
<SERVER_PUBLIC_IP_ADDRESS>:8082
<SERVER_PUBLIC_IP_ADDRESS>:8083
```
### Conclusion
Congratulations — you've completed this hands-on lab!
