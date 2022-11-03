# Storing Container Data In Docker Volumes

![Screenshot_42](https://user-images.githubusercontent.com/106797604/199679754-b02a48d7-7d3d-4b31-978f-0c35884ded5d.png)


You want to reduce the amount of static and infrequently changing content being built into your images. Some of the content does change occasionally, so it would be nice if the container images did not have to be rebuilt to handle these minor changes.

You'll also need to know how to back up the data being stored in this new way. Backups won't do any good if you can't restore them, so you'll need to know how to restore the volume data from a backup.

Since the website code is static, it makes a good test case to learn how to work with content in Docker volumes. The website code is available on GitHub. It has been cloned to the server, in the cloud_user's home directory, as widget-factory-inc.

### Introduction
Storing data within a container image is one option for automating a container with data, but it requires a copy of the data to be in each container you run.

For static files, this can be a waste of resources. Each file might not amount to more than a few megabytes, but once the containers are scaled up to handle a production load, that few megabytes can turn into gigabytes of waste.

Instead, you can store one copy of the static files in a Docker volume for easy sharing between containers.

In this lab, you will learn how Docker volumes interact with containers. You will do this by creating new volumes and attaching them to containers. You'll then clean up space left by anonymous volumes created automatically by the containers. Finally, you'll learn about backup strategies for your volumes.

### 1.Discover Anonymous Docker Volumes
1. View the currently available Docker images.
2. List any existing Docker volumes.
3. Run two postgres:12.1 containers in detached mode.
4. Inspect the postgres containers to learn about the attached volumes.
5. Run another detached postgres container using the --rm flag.
6. List the Docker volumes, then shut down the postgres containers to see what effect that has on the volumes.

### 2.Create a Docker Volume
1. Create a Docker volume for the website code.
2. Copy the website code into the volume from the host. You will need to use root permissions.

### 3.Use the Website Volume with Containers
1. Run an httpd container and mount the data volume. Expose container port 80 to view the website.
2. View the webpage in a browser. Use the public IP address of your instance.
3. Run another httpd container using the --rm flag. What do you expect will happen with the data volume when this container is stopped?
4. List the current Docker volumes.
5. Stop the temporary httpd container.
6. List the Docker volumes again. Did the volume behave like you expected?

### 4.Clean Up Unused Volumes
1. Remove any volumes not currently in use.
2. View the new list of volumes.

### 5.Back Up and Restore the Docker Volume
1. Inspect the website Docker volume to find its location on disk.
2. Back up the Docker volume from the host by using tar to create a compressed archive.
3. Back up the Docker volume from a container by mounting the volume and a backup data location to another container.
4. Restore a backup.
Note: Because the Docker data directories are protected, change to the root user for this task using sudo su - and the provided password.

### Discover Anonymous Docker Volumes
1. Check the docker images:
```docker images```
2. Run a container using the postgres:12.1 repository:
```docker run -d --name db1 postgres:12.1```
3. Run a second container using the same image:
```docker run -d --name db2 postgres:12.1```
4. Check the status of the containers:
```docker ps```
5.List the anonymous volumes:
```docker volume ls```
6. Inspect the db1 container:
```docker inspect db1 -f '{{ json .Mounts }}' | python -m json.tool```
7. Inspect the db2 container:
```docker inspect db2 -f '{{ json .Mounts}}' | python -m json.tool```
8. Create a third container using the --rm flag:
```docker run -d --rm --name dbTmp postgres:12.1```
9. Check the status of the container:
```docker ps -a```
10. List the anonymous volumes:
```docker volume ls```
11. Stop the db2 and dbTmp containers:
```docker stop db2 dbTmp```
12. List the anonymous volumes:
```docker volume ls```
13. Check the status of the containers:
```docker ps -a```
### Create a Docker Volume
1. Create a Docker volume:
```docker volume create website```
2. Verify that the volume was created successfully:
```docker volume ls```
3. Copy the widget-factory-inc data to the website container:
```sudo cp -r /home/cloud_user/widget-factory-inc/web/* /var/lib/docker/volumes/website/_data/```
4. List the copied data:
```sudo ls -l /var/lib/docker/volumes/website/_data/```

### Use the Website Volume with Containers
1. Run a docker container with the website volume:
```docker run -d --name web1 -p 80:80 -v website:/usr/local/apache2/htdocs:ro httpd:2.4```
2. Check the status of the container:
```docker ps```
3. In a web browser, verify connectivity to the container:
```<PUBLIC_IP_ADDRESS>```
4. Run a second container with the --rm flag:
```docker run -d --name webTmp --rm -v website:/usr/local/apache2/htdocs:ro httpd:2.4```
5. Check the status of the containers:
```docker ps```
6. Stop the webTmp container:
```docker stop webTmp```
7. Check the status of the containers:
```docker ps -a```
8. Verify that the website can still be accessed through a web browser:
```<PUBLIC_IP_ADDRESS>``` 

### Clean Up Unused Volumes
1. Clean up the unused volumes:
```docker volume prune```
2. Check the currently running containers:
```docker ps -a```
3. Remove the db2 container:
```docker rm db2```
4. Clean up the unused volumes again:
```docker volume prune```
5. List the current volumes:
```docker volume ls```

### Back Up and Restore the Docker Volume
1. Switch to the root user:
```sudo -i```
2. Find where the website volume data is stored:
```docker volume inspect website```
3. Copy the Mountpoint.
4. Back up the volume:
```tar czf /tmp/website_$(date +%Y-%m-%d-%H%M).tgz -C /var/lib/docker/volumes/website/_data .```
5. Verify that the data was backed up properly:
```ls -l /tmp/website_*.tgz```
6.List the contents of the tgz file:
```tar tf <BACKUP_FILE_NAME>.tgz```
7.Exit out of root:
```exit```
8. Run a new container using the website volume, and create a backup:
```docker run -it --rm -v website:/website -v /tmp:/backup bash tar czf /backup/website_$(date +%Y-%m-%d-%H-%M).tgz -C /website .```
9. Verify that the data was backed up properly:
```ls -l /tmp/website_*.tgz```
10. Switch to the root user:
```sudo -i```
11. Change to the /docker/volumes/ directory:
```cd /var/lib/docker/volumes/```
12. List the volumes:
```ls -l```
13. Change to the website/_data directory:
```cd website/_data/```
14. Remove the contents of the directory:
```rm * -rf```
15. Verify that the backups are still available:
```ls -l /tmp/website_*.tgz```
16. Restore the data to the current directory:
```tar xf /tmp/<BACKUP_FILE_NAME>.tgz .```
17. Verify that the data was restored successfully:
```ls -l```
### Conclusion
Congratulations — you've completed this hands-on lab!
