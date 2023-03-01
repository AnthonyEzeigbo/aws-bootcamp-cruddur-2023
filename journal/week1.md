# Week 1 — App Containerization

## Required homework 

# Run the dockerfile CMD as an external script

I ran the Dockerfile from an app.py script that has already been created, the script contains the following command below that is requsting,
To import flask, which will help setup the frontend and it dependencies.

![script](assets%20week1/external%20script.PNG)


# push and tag image to dockerhub 
Docker Hub provides a centralized location for storing and sharing Docker images. By pushing images to Docker Hub, we can easily distribute them to others and allow them to be pulled from any machine with Docker installed, i created a free tier dockerhub acccont, i loged in to dockerhub using the command ```docker login```
eg ```docker login -u anthonyeze - c qgpr_pat_IHRYB42N_Ls``` 

I tagged the Docker image with my Docker Hub username and the repository name using the command ```docker tag image_name``` 

then i push the Docker image to Docker Hub using the command ```docker push dockerhub_username/repository_name:version_number```

![Dockerhub](assets%20week1/dockerhub%20image.PNG)


# Using multi-stage building for a Dockerfile build
multi-stage build is used for creating lightweight images that a secure by separating the build process from run time enviroment

```# This code will build the specified python image
FROM python:3.10-slim-buster

# The working directory is set to the backendflask
WORKDIR /backend-flask

# it copies the requirements.txt file into the image. This txt file is currently inside the backend-flask folder
COPY requirements.txt requirements.txt

# This will install all the dependencies for our  our backend run on 
RUN pip3 install -r requirements.txt

# This will copy every folder inside the image and also build the application

COPY . .
RUN npm run build

ENV FLASK_ENV=development

# This will expose the port specified in our case its 4567
EXPOSE ${PORT}

# This will run our backend via flask module of python
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]```


