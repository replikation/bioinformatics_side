ubuntu server commands
===
# 1. Download

* download from [here](https://www.ubuntu.com/download/server)

# 2. User/Permission handling
## 2.1 User/Group informations

* general commands to get some inforamtions about users and their rights
````bash
# show all users
getent passwd
# show all groups (e.g. docker)
getent group
# see who is logged in
who
````

## 2.2 Add User/Group

* ask user also to change password
````bash
sudo adduser [username]
# tell him to change pw
sudo chage -d 0 [username]
````

* add docker to the user (to run docker without sudo)
````bash
sudo groupadd docker # creates if group is not available
sudo usermod -a -G docker $USER
````

# 3. Mounting devices

* identify all disks via `fdisk -l`
```bash
sudo mount /dev/nvme1n1p1 /mnt/500GB
```
