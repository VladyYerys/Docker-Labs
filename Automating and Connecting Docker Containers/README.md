# Automating and Connecting Docker Containers

### ABOUT THIS LAB
Creating a container image by hand is possible, but it requires manual processes. There has to be a more automatic way to build images. Manual processes do not scale and are not easily version controlled. Docker provides a solution to this problem - the Dockerfile. In this lab, you will create a Dockerfile to build an image to host a static website.

### LEARNING OBJECTIVES
- Build a First Version
- Load the Website into the Container
- Run a Container from the Image
- Read Me


# Docker Container Networking
### ABOUT THIS LAB
Each container should serve a single purpose, such as running one application like a web server. Containers can be powerful by themselves, but when connected together, they are far more useful. For example, a web server container can be connected to a database container to provide application storage. Docker provides multiple options for networking containers. In this lab, you'll explore a few of the common types of networks that Docker supports, and learn how containers within those networks interact.

### LEARNING OBJECTIVES
- Explore the Default Network
- Explore Bridge Networks
- Explore the Host Network

# Dockerize a Flask Application

### ABOUT THIS LAB
Migrating static content into containers was a great way to learn the basics of Docker, but real-world uses are usually dynamic, including full applications. This lab will show you the process to dockerize a Flask application. Flask is a lightweight Python WSGI micro web framework, however, you won't need to know any Python to complete this lab.

### LEARNING OBJECTIVES
- Create the Build Files
- Build and Setup Environment
- Run, Evaluate, and Upgrade
- Upgrade to Gunicorn
- Build a Production Image


# Building Smaller Images with Mutli-Stage Builds

### ABOUT THIS LAB
Containers are made of layers. Compile and install operations performed in the image, add to the layers, increasing the size of the container. Instead of keeping all of those layers into the final image, you can split those steps off, and only use the finished product. Docker provides this capability through multi-stage builds. In this lab, you will build an image the usual way, and inspect the image to see how it is put together. You'll then convert the Dockerfile to use Multi-Stage builds, and see how the new image compares.

### LEARNING OBJECTIVES
- Do Prep Work in the Image
- Add a Build Stage
- Create a Smaller Image
