# Volumes
 3.1 Volumes
 3.2 Copying Files into a Running Container
 3.3 Copying Files Through Dockerfile Instructions
 3.4 Testing that copying in the Dockerfile works
 3.5 Creating a Volume
 
### 3.2 Copying Files into a Running Container

One way to get a file into a container is with the docker container cp command. 
Here, we’ve got a container that’s already running with the name elegant_noether.

Like many Unix commands, the order of the arguments after the cp are important. 
First, we write a path to the location of the file we want to copy on your host 
machine, and after that we write the container ID, followed by a colon, followed 
by a path in the container where we want the file to be copied to.

Type:

    curl localhost:80/page.html 
    
to confirm that the page.html file doesn't exist already.

Type:

    docker container cp page.html elegant_noether:/usr/local/apache2/htdocs/.

Notice the order that the commands must be written: First, the path to the file on 
your host machine, then the container's ID followed by a colon, then the path to 
the file in the container.

Type:

    curl localhost:80/page.html 
    
to verify the page.html file now exists in the container.



### 3.3 Copying Files Through Dockerfile Instructions

Another way to copy a file into a container is to write a COPY instruction in a 
Dockerfile. The advantage of this approach over the docker container cp command 
is that each new container we create with the custom image will have the file in 
it versus having to run the command-line command each time we make a new container.

Add 
    
    COPY page.html /usr/local/apache2/htdocs/ 
    
after the RUN instruction in the Dockerfile.



### 3.4 Testing that copying in the Dockerfile works

We've just run docker image build -t web-server:1.1 . so that we can start a container 
and verify that the file was copied in during the build.

Write the command that starts a container with the image web-server:1.1 with port 80 on 
the host mapped to port 80 on the container. Let's also use the --detach flag to make 
the container run in the background.

    docker container run --detach --publish 80:80 web-server:1.1
    
Type:

    curl localhost:80/page.html 
    
to see the that the file is now there.
    
    
### 3.5 Creating a Volume

The first two approaches involved copying a file into a container. But as soon as the 
container is modified and stopped, all of those changes disappear. This is a problem 
if we’re using a container for local development, and one way to fix this problem is 
to use a data volume to create a connection between files on our local computer (host) 
and the filesystem in the container.


Type:

    docker run -d -p 80:80 -v /my-files:/usr/local/apache2/htdocs web-server:1.1 
    
to create a link between the folder /my-files on your host machine and the htdocs folder 
in the container. This also runs the container in the background.

    docker container run --detach --publish 80:80 --volume /my-files:/usr/local/apache2/htdocs web-server:1.1
    
Let’s see what this looks like from inside the container.

Type:

    docker container exec -it elegant_noether /bin/bash 
    
to get a shell in the container.

Type

    cd /usr/local/apache2/htdocs 
    
to change the directory to the htdocs folder.


Now that we’re in the right folder, type ls -la. This will show us that the container 
thinks those files on our local machine are inside of it, even though we know the 
truth — that we’ve created a volume that’s linking those local files to the container!


