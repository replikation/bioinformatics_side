# GPU Docker

* for detailed cuda installation etc check the **shell/gpu section**.


* nvidia creates docker which should make it easier to communicate with a gpu and dont have driver issues
* this workflow creates the nvidia gpu docker support
* the installation below assumes that you installed docker-ce and you are now uninstalling docker via apt remove and installing a nvidia compatible docker version
* installation and gpu-test is working:

```bash
# Add the package repositories
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -

# for a command line environment
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
# for linux mint 19.1 (based on ubuntu 18.04)
distribution="ubuntu18.04"

curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update

# add this point you need to know which current docker version is supported with nvidia
  docker --version
    # e.g. Docker version 18.09.1, build 4c52b90
  apt-cache madison nvidia-docker2 nvidia-container-runtime
    # e.g. nvidia-docker2 | 2.0.3+docker18.09.0-1 ...
    # so you need docker 18.09.0-1 if you are "above" it install docker like this:
# get docker versions
  apt-cache madison docker-ce
  # e.g. docker-ce | 18.09.0~ce-0~ubuntu | (...)
  sudo apt-get install docker-ce=18.09.0~ce-0~ubuntu

# Install nvidia-docker2 and reload the Docker daemon configuration
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd

# Test nvidia-smi with the latest official CUDA image
docker run --runtime=nvidia --rm nvidia/cuda:9.0-base nvidia-smi
```
