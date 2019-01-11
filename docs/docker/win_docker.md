# WSL/Docker/Win10Pro

* follow these steps after installing docker in windows via .exe
* you will link WSL to the docker deamon so its more like native "dockering"

## step by step
* needs win10 pro
* install docker for windows
* check in docker-windows settings the field `expose the daemon without TLS`

* now change the mount point of c d etc. in WSL to / so docker can use this
* in wsl enter command and add this:
````bash
sudo nano /etc/wsl.conf
# Enable extra metadata options by default
[automount]
enabled = true
root = /
options = "metadata,umask=22,fmask=11"
mountFsTab = false
````

* install now docker in wsl:
````bash
# Install Docker's package dependencies.
sudo apt-get update -y
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

# Download and add Docker's official public PGP key.
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify the fingerprint.
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# Install the latest version of Docker CE.
sudo apt-get update -y
sudo apt install docker.io

# Install Docker Compose into your user's home directory.
sudo usermod -aG docker $USER
sudo apt-get install -y python python3-pip
pip3 install --user docker-compose
# connects to the win10 docker daemon
echo "export DOCKER_HOST=tcp://localhost:2375" >> ~/.bashrc && source ~/.bashrc
````


## GPU Docker
````bash
docker run --rm -it -v /e/gpu_tests:/gputests replikation/guppy_gpu  -i /gputests/FAST5 -s /gputests/FASTQ -c <config file>
    --device 0


````
