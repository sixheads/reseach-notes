# Dive into Docker course - Nick Janetakis

* to check you have docker installed run:
  docker info

* then check you have docker composer running:
  docker-compose --version

## Discover Docker

* starts out with a simple docker hello world option
  docker run hello-world

* this just creates a simple container that prints some info to the console
* we then tried downloading and running the Alpine container which is a small linux os
  docker run -it alpine sh

- A docker image is everything you need to run an application - has files & no state
- you can download and run an image
- A docker container is an instance of a docker image
- you can have multiple containers running off a single image
- a container looses all its content when you stop running it. - they are immutable

### Docker Images

* we download images from Docker Hub - hub.docker.com
* docker store will replace the hub
* can manage your own docker images there too
* docker has official images from big names
* versions are labelled as tags

* to create a image we use a dockerfile
* dockerfile has the build steps for your application
* docker images are made up of layers

## Docker In the Real World

* dockerfiles are like a recipe or blueprint for the image you create
* first you have to start off with the FROM instruction to pull in an official image
* eg. FROM python:2.7-alpine // pulls in python then sets the verision

```
FROM python:2.7-2.7-alpine                  // import base image, in this instance python then after : the version

RUN mkdir /app                              // next we make the main app directory
WORKDIR /app                                // then set that as the working directory

COPY requirements.txt requirements.txt      // now copy the requirements for the container into the WORKDIR
RUN  pip install -r requirements.txt        // run those requirements - for Node this would be a package.json

COPY . .                                    // then copy all files from the top directory into the WORKDIR /app

LABEL maintainer="John Fry <john@sixheads.com>" \  // lets you add meta data to the dockerfile
  version="1.0"                                    // using the \ lets you move to the next line

CMD flask run --host-0.0.0.0 --port=5000    // with the CMD instruction we run the script and add a host and port
```

### Building an image

* to see all the docker commands in the terminal type
  docker --help
* or a better why is to pick a category first
  docker image --help

* to build your image within the directory:
  docker image build -t web1 .
  * the -t creates a tag for referencing the image in this case its web1
* to inspect the image run:
  docker image inspect web1

- creates a json dump of info about the image

* to give the image a version number rather than just latest run the command:
  docker image build -t web1:1.0 .

* to list all docker images on your machine run:
  docker image ls

  * this gives you lots of detail on all your images

* to remove an image you can either use the tag or that image ID:
  docker image rm web1:1.0

### Adding an image to Docker Hub

* first you need to setup the login and authenticate your docker account. run:
  docker login
* then fill in your details
* this gets saved to a config file

* then to setup the image repo type:
  docker image tag web1 sixheads/web1:latest
* must use your username then supply the name of the repo and version

* then to push it up to docker hub type:
  docker image push sixheads/web1:latest

* to remove the image locally type (this time using the first 4 characters of the image id):
  docker image rm -f 2f0f

* you can then pull it back down from Docker Hub
  docker pull sixheads/web1:latest

### Running Docker Containers

* to see all running containers use:
  docker container ls

  or -a to see all
  docker container ls -a

* to run a container:
  docker container run -it -p 5000:5000 -e FLASK_APP=app.py web1

  * the -it flag gives you access to terminal comands like CNTR C and also run things on the container itself

  - the -p 5000:5000 lets you set two ports the original docker container and where you want to view it
  - the -e flag lets you set an environment in this case we have a flask app which points to the main entry file
  - finally we add the name of the image to run, in this case web1

* to add a name and automatically stop it :
  docker container run -it --rm --name web1 -p 5000:5000 -e FLASK_APP=app.py web1

* to -d flag to run it in detached mode so we can use the same terminal window for other command
  docker container run -it --rm --name web1 -p 5000:5000 -e FLASK_APP=app.py -d web1

* to see a log for a running container use (can use the name or the first for character of id):
  docker container logs web1

* to run the log in realtime:
  docker container logs -f web1

* to get info about a running container
  docker container stats

* add the --research on-failure flag to make a container automatically rerun if it crashes
  docker container run -it --name web1 -p 5000:5000 -e FLASK_APP=app.py -d --restart on-failure web1

* to stop a container run:
  docker container stop web1

* to find all the possible flags run
  docker container run --help

### Live Reloading with Volumes

* in order to see changes we need to be running a container in debug mode - add this: -e FLASK_DEBUG=1
  docker container run -it -p 5000:5000 -e FLASK_APP=app.py --rm --name web1 -e FLASK_DEBUG=1 web1

* that's fine and it sees the change in the terminal but to live reload it in the browser we need to add a volume flag showing where to load it from and where to load it into - add this -v "$PWD:/app" :
  docker container run -it -p 5000:5000 -e FLASK_APP=app.py --rm --name web1 -e FLASK_DEBUG=1 -v "$PWD:/app" web1

### Debugging

* to execute a terminal window within the running container user:
  docker container exec -it web1 bash

  * this seems to fail with Alpine as it has no bash setup
  * User this instead:
    docker container exec -it web1 sh

* now we can see whats in the container:
  ls -la

* user CNTL d to get out of this terminal window

- to get the version of Flask running use:
  docker container exec -it web1 flask --version
