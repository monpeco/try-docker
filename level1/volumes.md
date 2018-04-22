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


### 3.4 Testing that copying in the Dockerfile works


### 3.5 Creating a Volume


