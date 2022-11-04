# Initializing the Docker Environment

### ABOUT THIS LAB
Docker is the leading containerization platform. If you are using containers, you are likely using Docker. In order to work with Docker, you must have the Docker daemon, and CLI available. This lab teaches you how to set up your environment, so you can get started working with Docker today!

### LEARNING OBJECTIVES
- Install Docker
- Enable the Docker Daemon
- Configure User Permissions
- Run a Test Image


# Working With Prebuilt Docker Images
### ABOUT THIS LAB
After installation, the best way to familiarize yourself with Docker, is to run containers from a few prebuilt images. In this lab, you will explore Docker Hub for images that will run a website. Once you find suitable images, you will get them into your development environment, and begin experimenting. You will run, stop, and delete containers from those images. You will also learn how to use existing data in containers.

### LEARNING OBJECTIVES
- Explore Docker Hub
- Get and View `httpd`
- Run a Copy of the Website in `httpd`
- Get and View `nginx`
- Run a Copy of the Website in `nginx`


# Handcrafting a Container Image
### ABOUT THIS LAB
If you run your website from a pre-built base image, it will require a manual process to set up the container each time it runs. For repeatability and scalability, the container, and your website code should be made into an image. In this lab, you will start with a base webserver image, modify settings in the container for your website, and then create images from the container. You'll demonstrate the importance of small changes to your container, and how they affect your image. Lastly, you will use your new images to create containers to see your hard work in action.

### LEARNING OBJECTIVES
- Get and Run the Base Image
- Install Tools and Code in the Container
- Create an Image from the Container
- Clean up the Template for a Second Version
- Run Multiple Containers from the Image


