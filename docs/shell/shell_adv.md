Advanced shell
===
___
# 1. Remote/cloud computing
## 1.1 ssh & keys

* tutorials for key generation and stuff [here](https://www.digitalocean.com/community/tutorials/how-to-copy-files-with-rsync-over-ssh)
* `ssh`, `-i ~/.ssh/key`(key location) and username@IP, use the `-K` flag to allow fastqc graphical interface

```bash
# examples
ssh -i ~/.ssh/replikation.pem ubuntu@ec2-11-218-160-32.us-east-2.compute.amazonaws.com
ssh -i ~/.ssh/azure_rsa student@13.45.57.169
```
## 1.2 file transfer

* normally you use the `scp` command
* however use `rsync` for huge amount of files

```bash
# examples
rsync -avp -e "ssh -i ~/.ssh/cloudgoogle" * replik@35.231.111.99:~/fast5_files
rsync -avpr -e "ssh -i ~/.ssh/cloudgoogle" <USER>@<IP>:~/auto_assembly/ .
# example for synology with different rsync path
# Username@IP is stored in $IP
rsync --rsync-path=/bin/rsync -vr -e "ssh -i ~/.ssh/id_rsa" --remove-source-files --include "*.fast5" --include "*/" --exclude "*" /cygdrive/c/data/reads/ $IP:/volume1/sequencing_data/
```

* example for an ongoing rsync to transfer fast5 files
  * limits the speed (so sequencer can use still most of the SSD write speed)

````bash
while true;
do
   rsync -vr --ignore-existing --bwlimit=3M --include "*.fast5" --include "*/" --exclude "*" /mnt/e/data/reads/20181205_1008_02WW /mnt/g/02.Schweden_WW
  sleep 5 ;
done
````  

## 1.3 Transfer through a proxy

* "Proxy jump" Transfer from A(host) over B(proxy) to C(server) or vice versa
* it functions like a normal ``scp`` but with the added `-oProxyJump=$PROXY` option
* so you state what the proxy in between is
* example:

````bash
# A is the host so "."
# B(proxy) is e.g.:
PROXY="username@IP"
# C(server) is e.g.:
Server="username@IP"
#download fast5 files from server to host via proxy:
scp -r -oProxyJump=$PROXY $SERVER:/var/home/FAST5_files .
# it will now ask for both passwords (proxy and server)
````

* more examples [here](https://superuser.com/questions/276533/scp-files-via-intermediate-host)

# 2. Screen

**virtual shell session in the cloud**

* you want to do this if you work remotely
* than it wont matter if the terminal window is closed sinces its a virtual process running on the host
* Start a virtual shell in a cloud computer:

```bash
screen -R <name> #name of the virtual machine
# works with tab completion after you started a virtual screen
screen -R <tab completion>
```

* A new empty command screen appears (this is the virtual screen)
* start the command in it and close the window
* to see all active screens type `screen -list`
* to get back to your screen type `screen -R <name>` or the number of `screen -list` (works better) so `screen -R 37509`


# 3. Advanced Scripting for deploying Software
* use [fabric](http://www.fabfile.org/) for software deploying scripts
* take a look at Hadriens [github for examples](https://github.com/SGBC/course/tree/master/fabfiles)
* makes sense to just create a simple sudo apt install script for all the important libraries


# 4. mounting devices

* get the name of the disk to mount via `df`

```bash
# mount <name of disk> to <target>
sudo mount /dev/nvme1n1p1 /mnt/
```

# 5. Big file splitting

* pack files via `tar`
* split them into "chunks"
* transfer chunks via internet (e.g. `scp` or `rsync`)
* put them together via `cat` and extract via `tar`
* example:

````bash
# tar a folder (-v for terminal status)
tar -cf archive.tar sequencing_read_folder/
  # alternatively with compression
  tar -czf archive.tar.gz sequencing_read_folder/

# split it into 10 GB chunks, alternatively -b 500M (500MB)
split -b 10G archive.tar "archive.tar.part"

# joining them together
cat archive.tar.part* > archive.tar

# extract it
tar -xf archive.tar sequencing_read_folder/
````

* a combined tar zip and splitting
````bash
# pack split
tar -cvzf targetdir/ | split --bytes=10GB - archivname.tar.gz.
# combine unpack
cat archivname.tar.gz.* > archivname.tar.gz; tar -xvzf archivname.tar.gz -C /mnt/d/dir
````


## 5.1 other

* not tested, its like tar directly to remote machine

````bash
tar -zc ./path | ssh remoteMachine "cat > ~/file.tar.gz"
````
