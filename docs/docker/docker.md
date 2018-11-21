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
# get into the docker bash
docker run --name docker_running -d replikation/debian_basic /bin/bash
# this can be done to just activate a container:
docker run --name docker_running -d replikation/debian_basic sleep 8h
# afterwards it will exits and detach the container
# check all docker containers with
docker ps -a
````

**More information here:**

* create auto build for dockers - [LINK](https://docs.docker.com/docker-hub/builds/#create-an-automated-build)

# Run dockers examples

* example

````bash
docker run --rm -it -v /path/to/example_data:/example_data andrewjpage/tiptoft tiptoft /example_data/ERS654932_plasmids.fastq.gz
````

* example porechop

````bash
docker run --rm -it -v /home/christian/Desktop/Docker_experimental_area:/Docker_experimental_area porechop porechop -i /Docker_experimental_area/all2.fastq -b /Docker_experimental_area/demultiplexed
````

+ `--rm` removes docker after it exits, so you dont "fill up" on containers

## Docker pipelines
* they need to run with `-i` and **without** `-t`
```bash
docker run --rm -i replikation/nanopolish echo $((3+4888)) | \
docker run --rm -i replikation/nanopolish tr -d 1
 ```

## Docker Wildcards
* WILDCARDS in docker via run are only interpreted via shell
* so run a command like this with `sh -c`:
```
docker run --rm -i replikation/nanopolish sh -c 'ls /folder/*.fasta'
```

# Clean up for dockers

**update all docker images**

* change the REPOSITORY to your repository name were all the images are stored

````bash
docker images |grep -v REPOSITORY|awk '{print $1}'|xargs -L1 docker pull
````

**stop all containers**

````bash
docker stop $(docker ps -a -q)
````

**remove all container**

````bash
docker rm $(docker ps -a -q)
````

**remove all untagged images**
````bash
docker rmi $(docker images -f "dangling=true" -q)
````
