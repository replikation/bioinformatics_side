# GPU

* accessing gpus is done via the cuda, because its not a cpu there are various driver and dependencies that have to match exactly
* see all nvidia things in your system
`ls -la /dev | grep nvidia`

## Nvidia cuda
* Nvidia cuda allows the user access to the gpus
* First you need to know for what Nvidia drivers your program was designed (e.g. nvidia 384 driver)
* now you need to make sure your nvidia cuda supports this versions
* check (here)[https://docs.nvidia.com/deploy/cuda-compatibility/index.html]
* some examples:

|CUDA Toolkit |	Linux x86_64 Driver Version
|-|-|
|CUDA 10.0 (10.0.130)|	>= 410.48
|CUDA 9.2 (9.2.88)|	>= 396.26
|CUDA 9.1 (9.1.85)|	>= 390.46
|CUDA 9.0 (9.0.76)|	>= 384.81
|CUDA 8.0 (8.0.61 GA2)|	>= 375.26
|CUDA 8.0 (8.0.44)|	>= 367.48
|CUDA 7.5 (7.5.16)|	>= 352.31
|CUDA 7.0 (7.0.28)|	>= 346.46

* e.g. if your program needs **384** since its the latest on xenial you need **cuda 9.0**
* download the deb file from [nvidia](https://developer.nvidia.com/cuda-downloads)
* use Legacy Download for older cuda versions
* use the .deb download on this side end do this (follow site, this is just an example)

```bash
sudo dpkg -i cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-<press tab>/7fa2af80.pub #this is a file on your computer
sudo apt-get update
sudo apt-get install cuda
```

## Setting up a GPU-machine
* example `gpu-guppy basecaller`
* available only via xenial (ubuntu 16) and it uses latest xenial-nvidia driver 384
* therefore cuda 9.0 is needed
* cuda is usually installing are drivers

```bash
sudo apt-get update
# download and get the cuda 9 deb from nvidia
wget https://developer.nvidia.com/compute/cuda/9.0/Prod/local_installers/cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64-deb
# this gets ubuntu 16.04 64bit linux deb file
# after downloading go to the deb file and do:
sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb
sudo apt-key add /var/cuda-repo-9-0-local/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda

# now guppy basecaller
sudo apt-get update
sudo apt-get install wget lsb-release
export PLATFORM=$(lsb_release -cs)
wget -O- https://mirror.oxfordnanoportal.com/apt/ont-repo.pub | sudo apt-key add -
echo "deb http://mirror.oxfordnanoportal.com/apt ${PLATFORM}-stable non-free" | sudo tee /etc/apt/sources.list.d/nanoporetech.sources.list
sudo apt-get update
sudo apt-get install ont-guppy
```
