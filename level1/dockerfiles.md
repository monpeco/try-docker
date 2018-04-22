# Dockerfiles
 2.1 Dockerfiles
 2.2 Your First Dockerfile
 2.3 Building an Image From a Dockerfile
 2.4 Running a Container From a Custom Image
 
### 2.2 Your First Dockerfile
In the previous level we created containers from pre-built base images. If we want 
to customize a base image before creating a container, we should use a Dockerfile.

The filename is important — it's always Dockerfile with a capital D.

We've created an empty Dockerfile that we can fill in.

The first line of a Dockerfile is usually the FROM keyword followed a base image name. 
Every other instruction in the Dockerfile following the FROM instruction will create a 
new image that blends together everything in the base plus the modifications we're 
making in the rest of the Dockerfile.

Type:

    FROM httpd:2.4 

as the first line of the Dockerfile. This will base our custom image off of 
version 2.4 of the httpd image.

Next, we can use the Dockerfile to expose a port inside of its associated container. 
Note that we'll still need to map ports between the host and container when we run 
docker container run, but this line will let the image know which ports inside of 
the container should become available.

Type: 

    EXPOSE 80 

on line 2

Remember how in the last level we had to docker container exec to install fortunes? 
Well, with a Dockerfile we can run any commands as the image is being built with the 
RUN command.

Type:
    
    RUN apt-get update 

on line 3 and 

    RUN apt-get install -y fortunes 
    
on line 4.

We can even combine multiple run statements on a single line, which is sometimes 
preferred to separate lines.

Remove the code on lines 3 and 4 and replace them with 

    RUN apt-get update && apt-get install -y fortunes


Since we can distribute Dockerfiles to other developers, it's a good idea to put 
our contact information in them with the LABEL instruction. LABEL accepts key-value 
pairs in the form of key="value". Let's label the maintainer of this image.

Type 

    LABEL maintainer="moby-dock@example.com"
    
(Did you know Moby Dock is the name of the Docker whale?)



### 2.3 Building an Image From a Dockerfile

Great, the Dockerfile is ready to go. We can use that Dockerfile to build a new 
image with the docker image build command. The --tag command is a useful option 
to pass to docker build that lets us specify the name of our new image followed 
by a version number.

Run the docker image build command with the --tag option and name the image web-server 
followed by a : and the number 1.0. (The number that comes after the colon sets the 
version number of the image.)

End the command with a single . so it knows to look for the Dockerfile in the same 
folder that the command is run in.

    docker image build --tag web-server:1.0 .
    
Now type docker image ls to get a list of all images on your computer. In this list, 
we'll see our newly created web-server image.



### 2.4 Running a Container From a Custom Image

Now we can create a container with the new web-server image.

Let's create a container with the new image we just made that meets the following 
criteria:

Uses the web-server version 1.0 image

Maps port 80 on the host to 80 on the container.

    docker container run --publish 80:80 web-server:1.0
    

