# Demo Scenario:
In this demo, we shall build a containerized web application and a standard redis server container image to build a typical enterprise application as part of web application modernization.

The application we’re going to be using is a simple 3-tier application. It has a webpage that talks to an application, that’s written in python. This application then then talk to a backend redis server to count the number of hits to the application. We are mostly ignoring any complexities in the presentation layer (webpage), and using the web-browser simply to access the output of the business logic tier.

# Pre-requisites
If you do not have docker, you can install it from here: https://docs.docker.com/engine/installation/
If you do not have a Docker ID account, you can set it up as follows: https://docs.docker.com/docker-id/ . 


# Step-1: Build the Application into a Docker Image
We have an application, but it needs to be packaged into a Docker image before we can deploy it. We do that using a file called “Dockerfile” as input to the Docker build process. A Dockerfile is a list of instructions for the Docker “builder” component that tells it how to construct the filesystem that will make up a container. Once that container has all of the files needed, it will then save the file system into an “image” that we can then deploy later. The concept is very similar to how you might build, and share, virtual machine images.

$ docker build -t <namespace>/webapp .

To verify if the image has been created, type 
$ docker images

# Step-2: Push the docker image to docker hub or IBM Cloud
The image that we just built is only available to our local Docker engine. In order for us to use it elsewhere, like in our IBM Cloud hosted Kubernetes cluster, we need to upload it (“push” it) to the registry:

Login into docker hub:

$ docker login
$ docker push namespace/webapp

# Step-3: Run the container locally
Use the docker container run command to run a container with the image created

$ docker run -d -p 5000:5000 namespace/webapp

Use the docker container ls command to get the ID of the running container you just created.
$ docker container ls

Then use that id to run bash inside that container using the docker container exec command. Since we are using bash and want to interact with this container from our terminal, use the -it flag to run using interactive mode while allocating a psuedo-terminal.
$ docker container exec -it containerId /bin/bash
  
# Step-4: Run the web application locally 
http://localhost:5000
The application will show errors, as the still the redis container is not up. Once the database is up, the errors should go off.

# Step-5: Use docker compose to use database service with web
$ docker-compose up

We might see some errors, while running this. Make sure you have logged in using the docker login command

# Step-6: Launch application should be running successfully
http://localhost:5000

You can actually login into the redis container and check the value yourself using the "redic-cli"



