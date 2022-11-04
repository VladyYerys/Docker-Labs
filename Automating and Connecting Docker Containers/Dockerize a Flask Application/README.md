# Dockerize a Flask Application

### Introduction
Migrating static content into containers was a great way to learn the basics of Docker, but real-world uses are usually dynamic, including full applications.

This lab will show you the process to dockerize a Flask application. Flask is a lightweight Python WSGI micro web framework, however, you won't need to know any Python to complete this lab.

### Scenario
You have successfully migrated your company's website into a container. Thanks to your efforts, you have proven the value of containers to your boss and have now been tasked with migrating another application into a container. This time, however, it's a full application and not just a static website. Using the knowledge you've gained from working with static content, migrate the Notes application into a container.

This lab will be using the Notes application available in the notes folder in your user's home directory.

![Screenshot_64](https://user-images.githubusercontent.com/106797604/200055944-f582a135-bb72-4940-8bcb-63d874425be6.png)


### 1.Create the Build Files
1. Inspect the Flask application files to learn what files we need to include and exclude from the image. There is a ```Pipfile.lock```, a ```.gitignore```, and a ```migrations``` directory. We don't want those in the image.
2. Create a ```.dockerignore``` file to exclude build and metadata information from the image.
3. Create the ```Dockerfile``` to build the image.
Start with Python 3 as the base image.
- Setup Python environment variables.
- Install dependencies in the container from the Pipfile.
- Set the working directory.
- Specify the port flask will listen on.
- Start the flask server.

### 2.Build and Setup Environment
1. Build an image named notesapp with version 0.1.
2. Inspect the Docker environment to see the new image, and what else is running.
3. Use a container of the notesapp image to set up the database.
- Note: The database can be set up via the app itself using the following command.

```
flask db init && flask db migrate && flask db upgrade
```

### 3.Run, Evaluate, and Upgrade
1. Run the NotesApp container in the current terminal to watch its output.
2. View the application in a browser. Sign up for an account, create a note, then edit it.
3. View the messages in the terminal and check for warnings.
4. Stop using Flask in debug mode.
-  Remove the line FLASK_ENV=development from the file ~/notes/.env.
5. Since we've changed a file included in the image, build the image again as version 0.2.
6. Run the new version of the NotesApp image in the terminal. Debug mode should no longer be on.
7. Look for any new warnings.
- Note: We're not in debug anymore, but now Flask tells us "Do not use it in a production deployment". That's not a good way to leave our container.
8. Stop the container.

### 4.Upgrade to Gunicorn
We will upgrade our Flask application to use Gunicorn, which is a production-ready WSGI HTTP server. This will involve adding a bit of code to our existing app and upgrading our build.

1. Add Gunicorn to the dependencies using pipenv.
Pipenv is being used to manage the dependencies for our application. Let's use the image we have, which includes pipenv, to upgrade the Pipfile so we don't have to install a development environment on the server.

2. Upgrade the application code for Gunicorn.
Flask can use dotenv automatically, but Gunicorn must be told to use it explicitly. Add the following lines to __init__.py below the import statements:
from dotenv import load_dotenv, find_dotenv
load_dotenv(find_dotenv())

3. Change the Dockerfile to run Gunicorn instead of Flask.
- Replace the WORKDIR and CMD in the Dockerfile with the below code:
```
WORKDIR /app
CMD [ "gunicorn", "-b 0.0.0.0:80", "notes:create_app()"]
```

### 5.Build a Production Image
1. Build the NotesApp image again. Remember to increment the version.
2. Run a container from the production image, this time using detached mode to run in the background.
3. Inspect the Docker environment to ensure everything is running.
4. View the application in a browser. Use the server's public IP.

### Create the Build Files
1. Change to the notes directory:
```cd notes```
2. List the files in the directory:
```ls -la```
3. Inspect the config.py file:
```cat config.py```
4. Crate the .dockerignore file:
```vim .dockerignore```
5.In the file, paste the following:
```
.dockerignore
Dockerfile
.gitignore
Pipfile.lock
migrations/
```
6. Save the file:
```ESC:wq```
7. Create the Dockerfile:
```vim Dockerfile```
8. In the file, paste the following:
```
FROM python:3
ENV PYBASE /pybase
ENV PYTHONUSERBASE $PYBASE
ENV PATH $PYBASE/bin:$PATH
RUN pip install pipenv

WORKDIR /tmp
COPY Pipfile .
RUN pipenv lock
RUN PIP_USER=1 PIP_IGNORE_INSTALLED=1 pipenv install -d --system --ignore-pipfile

COPY . /app/notes
WORKDIR /app/notes
EXPOSE 80
CMD ["flask", "run", "--port=80", "--host=0.0.0.0"]
```

### Build and Setup Environment
1. Build the notesapp image:
```docker build -t notesapp:0.1 .```
2. Check the status of the image:
```docker images```
3. List the containers:
```docker ps -a```
4. List the docker networks:
```docker network ls```
5. Run a container using the notesapp image and mount the migrations directory:
```
docker run --rm -it --network notes -v /home/cloud_user/notes/migrations:/app/notes/migrations notesapp:0.1 bash
```
6. Once connected to the container, enable SQLAlchemy:
```flask db init```
7. Check the migrations folder:
```ls -l migrations```
8. Create the files needed to configure the database:
```flask db migrate```
9. Apply the files:
```flask db upgrade```

### Run, Evaluate, and Upgrade
1. Run a container using the notesapp:0.1 image:
docker run --rm -it --network notes -p 80:80 notesapp:0.1
2. Using a web browser, navigate to the public IP address for the server.
3. Sign up for a new account using an email address and password.
4. Once you are signed up, log in to your account.
5. Create your first note.
6. Verify that you can edit the note.
7. Back in the terminal, disable ```Debug mode``` by editing the ```.env``` file:
```vim .env```
8. Remove the ```export FLASK_ENV='development' ``` line.
9.Save the file:
```ESC:wq```
10. Build the image again:
```docker build -t notesapp:0.2 . ```
11. Run a container using the updated image:
``` docker run --rm -it --network notes -p 80:80 notesapp:0.2 ```
12. In a web browser, navigate to the public IP address for the server, and log in to your account.
13. Verify that you can add a second note.
14. In the terminal, stop the container:
```CTRL+C```

### Upgrade to Gunicorn
1. Check the Pipfile:
```cat Pipfile```
2. Run a container and modify the pip file:
```docker run --rm -it -v /home/cloud_user/notes/Pipfile:/tmp/Pipfile notesapp:0.2 bash```
3. Once connected, change to the /tmp directory:
```cd /tmp```
4. Add gunicorn to the list of dependencies:
```pipenv install gunicorn```
5. Exit the container:
```exit```
6. Verify that gunicorn was added under [packages]:
```cat Pipfile```
7. Modify the init.py script:
```vim __init__.py```
8. Beneath the ```import``` section, add the following:
```
from dotenv import load_dotenv, find_dotenv
load_dotenv(find_dotenv())
```
9. Save the file:
```ESC:wq```
10. Open the ```Dockerfile```:
```vim Dockerfile```
11. To the bottom of the file, make the following changes:
```
COPY . /app/notes
WORKDIR /app
EXPOSE 80
CMD ["gunicorn", "-b 0.0.0.0:80", "notes:create_app()"]
```
12. Save the file:
```ESC:wq```
 
### Build a Production Image
1. Build the updated notesapp image:
```docker build -t notesapp:0.3 .```
2. Run a detached container using the updated image:
```docker run -d --name notesapp --network notes -p 80:80 notesapp:0.3```
3. Check the status of the container:
```docker ps -a```
4. In a web browser, navigate to the public IP address for the server, and log in to your account.
5. Verify that you can create a new note.

### Conclusion
Congratulations — you've completed this hands-on lab!

