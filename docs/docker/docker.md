Docker (hub.docker.com)
===

* docker can't be installed on Win10 Home edition
* Created a VM with ubuntu (ubuntu flavor also)
* activate in the **BIOS "VT-d"** and install a "ubuntu VM" via **VMWare**

# Getting started
## Installing docker:

* install guide for docker [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository)
* update the apt to find the docker repositories:

**1. install dependencies for docker**

```bash
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

**2. get docker key and verify it:**

```bash
sudo apt-key fingerprint 0EBFCD88

# results:
pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

**3. add the docker repository**

```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```


**4. Install docker**

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
docker run --rm <imagename> <command> # runs image as a container and removes it afterwards  
docker pull <repositoryname/dockername> # basically git clone of a docker image
docker build -t <image_name> . # build a image from Docker file in .

docker images # shows all images
docker rmi <name> # removes a image

docker ps -a # shows all current containers (active and exited)
docker rm <name> # removes docker container (IMPORTANT if you want to clean up)
```

# Creating docker images

* **via automated builds** on hub.docker.com
* this means I create `Dockerfiles` in ubuntu and check if they can be created, than I only push the `Dockerfiles` to github

+ create a new automated rep on dockerhub
+ name it like the image (e.g. debian_basic)
+ point under options to the correct directory in github
+ dockerhub needs to be linked to your github

## ssh into docker containers

* this makes creating Dockerfiles much more easier since you can check if something is wrong or not
* to get into a docker container you have to attach a images (then called container)

````bash
docker run -name docker_running -d replikation/debian_basic sleep 8h
# now the container will be active for 8 hours
# afterwards it will exits and detach the container

# check all docker containers with
docker ps -a

# ssh into it the one called "docker_running"
docker exec -it docker_running /bin/bash
````

**More information here:**

* create auto build for dockers - [LINK](https://docs.docker.com/docker-hub/builds/#create-an-automated-build)

# Run dockers examples

* example

````bash
docker run --rm -it -v /path/to/example_data:/example_data andrewjpage/tiptoft tiptoft /example_data/ERS654932_plasmids.fastq.gz
````

+ `-rm` removes docker after it exits
