ubuntu server commands
===
# 1. Download

* download from [here](https://www.ubuntu.com/download/server)
* select in the installation menu also the docker installation

# 2. User/Permission handling

* add docker to the user (to run docker without sudo)

````bash
sudo groupadd docker # creates the docker group
sudo usermod -a -G docker $USER
sudo reboot
````

# 3. Mounting devices

* identify disks via `fdisk -l`

```bash
sudo mount /dev/nvme1n1p1 /mnt/
```
