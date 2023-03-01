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

# Implement a healthcheck in the V3 Docker compose file

This configuration adds a healthcheck to the your_service service. The test parameter specifies the command to run to check the health of the container. In this example, the healthcheck uses curl to request the /healthcheck endpoint on localhost:0000. If the command exits with a non-zero status code, the container is considered unhealthy.

```version: '3'
services:
  your_service:
    image: your_image
    ports:
      - "0000:0000"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:0000/healthcheck"]
      interval: 1m30s
      timeout: 10s
      retries: 3
 ```

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
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]
```

#  best practices of Dockerfiles 

1. Use official base images from Docker Hub whenever possible. These images are maintained by the Docker community and are more secure and reliable.

2. Minimize the number of layers in your Dockerfile. Each layer adds overhead to the build process and increases the size of the final image.

3. Use the COPY command instead of ADD whenever possible. COPY is more predictable and doesn't have the extra functionality of ADD.

4. Use multi-stage builds to reduce the size of the final image. This allows you to build your application in one image and then copy only the necessary files into a smaller image.

5. Use environment variables to pass configuration options to your container. This makes your container more flexible and easier to configure.

6. Use specific versions for all dependencies to ensure reproducibility. This helps avoid issues with incompatible versions in the future.

7. Avoid running commands as root in your Dockerfile. Instead, use the USER command to specify a non-root user to run the container.



