## Screen - virtual shell session in the cloud

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

## Installing software
* Download and compile new programs or git clones into `~/install/`
* then put the path to `~/.bashrc`:
    ```b
    # do nano ~/.bashrc and add
    export PATH="/home/replik/install/canu/Linux-amd64/bin:$PATH"
    ```
*   Search the programs in github
*   the `make` or `configure` steps are written there or in the `README.md`. Do a `make clean` if you get errors during `make` before doing a new `make`

## Advanced Scripting for deploying Software 
* use [fabric](http://www.fabfile.org/) for software deploying scripts
* take a look at Hadriens [github for examples](https://github.com/SGBC/course/tree/master/fabfiles)
* makes sense to just create a simple sudo apt install script for all the important libraries

## pip for Python installation

* due to the version chaos that is pip pip3 python and python3, here an explanation
* stick with python 3
* so if a site tells you to do `pip install` do `pip3 install`

```bash
# installing the pip3 from python3.X (not pip from python 2.7)
sudo apt-get install python3-pip ## use the apt-get for now
# example of a pip install, DONT use sudo here
pip3 install pylama pylama-pylint
```
