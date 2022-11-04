# Docker Container Networking

### Introduction
Each container should serve a single purpose, such as running one application like a web server. Containers can be powerful by themselves, but when connected together, they are far more useful.

For example, a web server container can be connected to a database container to provide application storage. Docker provides multiple options for networking containers.

In this lab, you'll explore a few of the common types of networks that Docker supports, and learn how containers within those networks interact.

### Scenario
Thanks to an increase in demand for your services, your organization wants to begin migrating a few applications to containers; to be in a better position to scale with the increase in demand. You will need to ensure that once in containers, the multi-tier applications can still communicate with each other.
![Screenshot_60](https://user-images.githubusercontent.com/106797604/200053564-d809efca-9c28-411f-b199-672ef4fdd93d.png)
![Screenshot_61](https://user-images.githubusercontent.com/106797604/200053567-be0e5694-363d-4018-9087-1a6b5312faef.png)
![Screenshot_62](https://user-images.githubusercontent.com/106797604/200053570-ed7ca84f-0723-496d-ba43-2cfa6593ab6a.png)
![Screenshot_63](https://user-images.githubusercontent.com/106797604/200053572-2f3946dd-6653-4dad-801f-7139a9094d58.png)
### 1.Explore the Default Network
1. List the default networks.
2. Run an httpd container named web1 without specifying a network and see which network it uses.
3. Attempt to connect to web1 by name and by IP from another container run without a network specified.

### 2.Explore Bridge Networks
1. Create a new bridge network named test_application.
2. Run an httpd container named web2 in the test_application network.
3. Attempt to connect to web2 by name and by IP from another container within the test_application network.
4. Attempt to connect to web1 from a container in the test_application network.

### 3.Explore the Host Network
1. Run an httpd container named web3 on the host network.
2. Attempt to connect to web3 directly from the server.
3. Attempt to connect to web3 from another container on the host network.
4. Attempt to connect to web1 and web2 from a container on the host network.

### Explore the Default Network
1. List the default networks:
```docker network ls```
2. Run an httpd container named web1, without specifying a network, and see which network it uses:
```
docker run -d --name web1 httpd:2.4
docker inspect web1```
3. Take note of the ```IPAddress```.
4. Run a container using the busybox image, and see if you can connect to the web1 server:
```docker run --rm -it busybox```
5. Check the container's networking, and verify it is in the same IP range as web1:
```ip addr```
6. Ping the web1 container using the IP address:
```ping <WEB1_IP_ADDRESS>```
7. Attempt to ping the web1 container by name:
```ping web1```
8. Attempt to access web1 using wget:
```wget <WEB1_IP_ADDRESS>```
9.Exit the container:
```exit```

### Explore Bridge Networks
1. Create a new bridge network named test_application:
```docker network create test_application```
2. Run an httpd container named web2, in the test_application network:
```docker run -d --name web2 --network test_application httpd:2.4```
3. Check the status of the container:
```docker ps -a```
4. Verify that web2 was added to the test_application network:
```docker inspect web2```
5. Run a container using the busybox image, and see if you can connect to the web2 server, within the test_application network:
```docker run --rm -it --network test_application busybox```
6.Check the container's networking, and verify it is in the same IP range as web2:
```ip addr```
7. Ping the web2 container using the IP address:
```ping <WEB2_IP_ADDRESS>```
8. Attempt to ping the web2 container by name:
```ping web2```
9. Using wget, attempt to access web2 with the hostname:
```wget web2```
10. Attempt to ping web1:
```ping <WEB1_IP_ADDRESS>```
11. Attempt to access web1 using wget:
```wget <WEB1_IP_ADDRESS>```
12. Exit the container:
```exit```

### Explore the Host Network
1. Run an httpd container named web3 on the host network:
```docker run -d --name web3 --network host httpd:2.4```
2. Check the status of the container:
```docker ps -a```
3. Attempt to connect to web3 directly from the server:
```wget localhost```
4. Stop web3:
```docker stop web3```
5. Attempt to connect to web3 directly from the server again:
```wget localhost```
6. Start web3:
```docker start web3```
7. Run a container using the busybox image, and see if you can connect to the web3 server:
```docker run --rm -it --network host busybox
ping web3```
8. Using wget, attempt to access localhost within the busybox image:
```wget localhost```
9. Attempt to ping web2:
```ping <WEB2_IP_ADDRESS>```
10. Attempt to ping web1:
```ping <WEB1_IP_ADDRESS>```

### Conclusion
Congratulations — you've completed this hands-on lab!


