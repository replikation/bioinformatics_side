docker
===

* docker cant be installed on Win10 Home edition
* i created a VM now with linux mint...

* activate in the BIOS VT-d
*

# Getting started

## Installing docker:

* install guide for docker [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository)
* update the apt to find the docker repositories:

**add docker rep to you ubuntu**

```bash
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

* verifying the apt update with:

```bash

sudo apt-key fingerprint 0EBFCD88

# results:
pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```


**installing docker**

```bash
sudo apt-get update

sudo apt-get install docker-ce

sudo docker run hello-world

```
* add "docker" to your group so you don't have to type sudo every time

```bash
sudo usermod -a -G docker $USER
sudo reboot
```
## Important commands

```bash
docker build -t <image_name> . # build a image

docker images # shows all images
docker rmi <name> # removes a image

docker ps # shows current attached docker containers
docker rm <name> # removes docker container
```

# Docker example

* the Dockerfile input in the following steps can be all combined to "one big" Dockerfile
* like an big ready to deploy docker pipeline container

+ currently I would still need the initial `docker pull ubuntu`

## Step 1 - ubuntu

* first I want a working ubuntu image with the essentials on it, so I can use apt get and stuff (easier than creating a minimal container at the start)
* go into an empty folder (e.g. `docker_tutorial`)
* do `nano Dockerfile` and add:

````bash
FROM ubuntu # gets the ubuntu image
MAINTAINER YOU NAME<your.name@googlemail.com>
RUN apt-get update
RUN apt-get install -y wget build-essential
````
* save it
* run inside the `docker_tutorial` folder the following:

````bash
docker pull ubuntu # first get the basic ubuntu image
# now we create out of the ubuntu image the essential image with more libs and stuff
docker build -t="ubuntu/ubuntu-build-essential" .
# this creates a docker image using the Dockerfile
# you can view all your docker images and sizes via
docker images
````



## Step 2 - samtools

* we now use the essential image and create a new image which also includes samtools
* change the content of the Dockerfile to:

```bash
FROM main_ubuntu/ubuntu-build-essential # the image from step 1
MAINTAINER YOU NAME<your.name@cruk.cam.ac.uk>
RUN apt-get update
RUN apt-get install -y ncurses-dev zlib1g-dev
RUN wget https://github.com/samtools/samtools/releases/download/1.3.1/samtools-1.3.1.tar.bz2
RUN mv samtools-1.3.1.tar.bz2 /opt
WORKDIR /opt
RUN tar -jxf samtools-1.3.1.tar.bz2
WORKDIR samtools-1.3.1
RUN ./configure
RUN make
RUN make install
```
* now run:

````bash
docker build -t bioinf/samtools_docker .
````

## Step 3 - running samtools_docker
* just do the following

````bash
 docker run bioinf/samtools_docker samtools --help
````
