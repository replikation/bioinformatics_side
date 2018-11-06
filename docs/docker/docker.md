docker
===

* docker cant be installed on Win10 Home edition
* i created a VM now with linux mint...
* activate in the BIOS "VT-d" to install ubuntu VMs via VMWare


# Getting started
## Installing docker:

* install guide for docker [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository)
* update the apt to find the docker repositories:

**add docker rep to your ubuntu**

```bash
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

* get docker key and verify it:

```bash
sudo apt-key fingerprint 0EBFCD88

# results:
pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

* add the repository

```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```


**Installing docker**

```bash
sudo apt-get update

sudo apt-get install docker-ce

sudo docker run hello-world

```
Add "docker" to your group so you don't have to type sudo every time

```bash
sudo usermod -a -G docker $USER
sudo reboot
```
## Important commands

```bash
docker build -t <image_name> . # build a image from Docker file in .

docker images # shows all images
docker rmi <name> # removes a image

docker ps # shows current attached docker containers
docker rm <name> # removes docker container
```

# Creating docker images

* **via automated builds**
* this means i create "Dockerfiles" in ubuntu and check if they can be created, than i only push the "Dockerfile" to github


+ create a new automated rep on dockerhub
+ name it like the image (e.g. debian_basic)
+ point under options to the correct directory
+ dockerhub needs to be linked to your github

**More information here:**

* create auto build for dockers
https://docs.docker.com/docker-hub/builds/#create-an-automated-build

# Run dockers examples

* example

````bash
docker run --rm -it -v /path/to/example_data:/example_data andrewjpage/tiptoft tiptoft /example_data/ERS654932_plasmids.fastq.gz
````

+ `-rm` removes docker after it exits
