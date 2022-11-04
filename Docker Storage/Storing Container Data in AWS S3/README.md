# Storing Container Data in AWS S3

## Introduction
Using Docker volumes is the preferred method of storing container data locally. Volume support is built directly into Docker, making it an easy tool to use for storage, as well as more portable. However, storing container data in Docker volumes still requires you to back up the data in those volumes on your own.

There is another option - storing your container data in the cloud. It's not a solution for every problem, but after this lab, you'll have another tool at your disposal.

This lab will show you how to mount an S3 bucket onto your local system as a directory. You will then mount that directory into your Docker container. We will use an httpd container to serve the contents of that bucket as a webpage, but you can use it to share any common data between containers.

This will demonstrate how flexible Docker can be. You can make changes to your bucket and all of your containers using the S3 bucket will near-instantly have access to the content.

### Scenario
While working on upgrading your website to a Docker container, you should explore other options for storing container data. One alternative to storing static content in a Docker volume, is to store the data in an AWS S3 bucket.

If you are already using AWS, this would allow you to not worry about backing up container data, thanks to the data being stored in the S3 bucket.

You will be using s3fs, a FUSE-based file system, to mount the S3 bucket to your server. You'll prepare the server by installing all necessary tools. You'll then copy the website data, cloned from GitHub into cloud_user's home directory, to the S3 bucket. You'll use a bind mount to get your S3 bucket directory into your container; and finally, you'll view the content from a web browser.

Note: While this method works for static content, or content that changes occasionally, this should not be used for files that are written frequently, such as a database. That will drastically increase latency and drive up your cloud storage costs. We enable local caching and mount the data as read-only in the container to help mitigate these issues. You can, however, use this method for data output if the container processes a job that emits uniquely-named output.
![Screenshot_51](https://user-images.githubusercontent.com/106797604/199860801-756aa61b-bb23-4ae1-a47d-3decc528038c.png)
![Screenshot_52](https://user-images.githubusercontent.com/106797604/199860804-65a18577-e7b1-4925-8ce7-cbdf856914ef.png)


### 1.Configuration and Installation
1. Install the AWS CLI.
2. Configure the AWS CLI for your user.
   - Use the Access Key ID and Secret Access Key provided in your lab credentials.
   - Use us-east-1 as the default region.
   - Output is optional, but json is a good choice.
3. Copy the credentials to the root user.
4. Install s3fs-fuse. This package is in EPEL, which is already installed on the server.

### 2.Prepare the Bucket
1. Create a mount point on the server.
2. Mount the S3 bucket.
   - Use the bucket name provided to you with the lab credentials. It is helpful to set this as an environment variable.
   - To prevent a lot of extra calls to S3 that will increase your AWS bill, enable local file system caching by setting the use_cache option when mounting.
3. Copy the website files into the bucket.
4. Verify the files are in the folder.
5. Verify the files are in the S3 bucket.

### 3.Use the S3 Bucket Files in a Docker Container
1. Run an httpd container to serve the website. Remember to mount the bucket and publish the web server port.
2. View the webpage in a browser. Use the server's public IP provided with the lab.
3. Create a new page in the bucket, called newPage.html, by making a copy of an existing page.
4. View the new webpage in a browser. This will be at: http://<ServerPublicIP>/newPage.html.
5. Verify that the new webpage is in the S3 bucket.

  
  ### Configuration and Installation
1. Install the awscli, while checking if there are any versions currently installed, and not stopping any user processes:
```pip install --upgrade --user awscli```
2. Configure the CLI:
```aws configure```
Enter the following:
```
AWS Access Key ID: <ACCESS_KEY_ID>
AWS Secret Access Key: <SECRET_ACCESS_KEY>
Default region name: us-east-1
Default output format: json
```
4. Copy the CLI configuration to the root user:
```sudo cp -r ~/.aws /root```
5. Install the s3fs package:
```sudo yum install s3fs-fuse -y```

  
### Prepare the Bucket
1. Create a mount point for the s3 bucket:
```sudo mkdir /mnt/widget-factory```
2. Export the bucket name:
```export BUCKET=<S3_BUCKET_NAME>```
3. Mount the S3 bucket:
```sudo s3fs $BUCKET /mnt/widget-factory -o allow_other -o default_acl=public-read -o use_cache=/tmp/s3fs```
4. Verify that the bucket was mounted successfully:
```ll /mnt/widget-factory```
5. Copy the website files to the s3 bucket:
```cp -r ~/widget-factory-inc/web/* /mnt/widget-factory```
6. Verify the files are in the folder:
```ll /mnt/widget-factory```
7. Verify the files are in the s3 bucket:
```aws s3 ls s3://$BUCKET```

### Use the S3 Bucket Files in a Docker Container
1. Run an httpd container using the S3 bucket:
```docker run -d --name web1 -p 80:80 --mount type=bind,source=/mnt/widget-factory,target=/usr/local/apache2/htdocs,readonly httpd:2.4```
2.In a web browser, verify connectivity to the container:
```<SERVER_PUBLIC_IP_ADDRESS>```
3. Check the bucket cache:
4. Change to the ```/mnt/widget-factory/``` directory:
```cd /mnt/widget-factory```
5. Create a new page within the bucket:
```cp index.html newPage.html```
6. In a web browser, verify that the new page is accessible:
```<SERVER_PUBLIC_IP_ADDRESS>/newPage.html```
7. Verify that the page was added to the bucket:
```aws s3 ls $BUCKET```

  ### Conclusion
Congratulations — you've completed this hands-on lab!
