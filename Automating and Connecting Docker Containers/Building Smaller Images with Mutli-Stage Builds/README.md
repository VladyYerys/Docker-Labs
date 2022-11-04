### Building Smaller Images with Mutli-Stage Builds

### Introduction
Containers are made of layers. Compile and install operations performed in the image, add to the layers, increasing the size of the container.

Instead of keeping all of those layers into the final image, you can split those steps off, and only use the finished product.

Docker provides this capability through multi-stage builds. In this lab, you will build an image the usual way, and inspect the image to see how it is put together. You'll then convert the Dockerfile to use Multi-Stage builds, and see how the new image compares.

### Scenario
You have been asked to evaluate the Docker containers your company is using to try to make them more efficient. Reducing the image size will allow you to use less resources, or run more containers using the same hardware. This can save the company money, and make you look like a rock star.

![Screenshot_65](https://user-images.githubusercontent.com/106797604/200059823-787e0999-ac3d-4b8f-b1bc-35c71d637b7b.png)


### 1.Do Prep Work in the Image
1. Inspect the current Dockerfile for the NotesApp image.
2. Build the image as notesapp and tag it as version default.
3. Inspect the default image version, to see how many layers it has, and how large the image is.

### 2.Add a Build Stage
1. Move the install steps in the Dockerfile to a separate stage.
2. Copy the finished installs into the final image.

### 3.Create a Smaller Image
1. Build the image as notesapp and tag it as version multistage.
2. Inspect the new multistage image version to see how many layers it has and how large the image is.


### Do Prep Work in the Image
1. Change to the notes directory:
```cd notes```
2. Check the Dockerfile:
```cat Dockerfile```
3. Build an image using the file:
```docker build -t notesapp:default .```
4. Set a variable to view the layers of the image:
```export showLayers='{{ range .RootFS.Layers }}{{ println . }}{{end}}'```
5. Set a variable to show the size of the image:
```export showSize='{{ .Size }}'```
6. Show the image layers:
```docker inspect -f "$showLayers" notesapp:default```
7. Count the number of layers:
```docker inspect -f "$showLayers" notesapp:default | wc -l```
8. Show the size of the image:
```docker inspect -f "$showSize" notesapp:default | numfmt --to=iec```

### Add a Build Stage
1. Open the Dockerfile:
```vim Dockerfile```
2. Add a build stage by adding the following:
```
FROM python:3 AS base
ENV PYBASE /pybase
ENV PYTHONUSERBASE $PYBASE
ENV PATH $PYBASE/bin:$PATH

FROM base AS builder
RUN pip install pipenv
WORKDIR /tmp
COPY Pipfile .
RUN pipenv lock
RUN PIP_USER=1 PIP_IGNORE_INSTALLED=1 pipenv install -d --system --ignore-pipfile

FROM base
COPY --from=builder /pybase /pybase
COPY . /app/notes
WORKDIR /app/notes
EXPOSE 80
CMD [ "flask", "run", "--port=80", "--host=0.0.0.0" ]
```
3. Save the file:
```ESC:wq```

### Create a Smaller Image
1. Build the image:
```docker build -t notesapp:multistage .```
2. Show the multistage image layers:
```docker inspect -f "$showLayers" notesapp:multistage```
3. Count the layers:
```docker inspect -f "$showLayers" notesapp:multistage | wc -l```
4. Show the size of the image:
```docker inspect -f "$showSize" notesapp:multistage | numfmt --to=iec```

### Conclusion
Congratulations — you've completed this hands-on lab!
