# Atom
* steps here are described using win10 and a WSL with ubuntu

## Important Packages

* minimap
* linter
    * and linter-shellcheck
        * need also shellcheck in linux or powershell
    * and linter-python
        * needs pylama installed in python 3
* remote-atom
    + you need to start the atom server in atom
    + `Packages -> Remote Atom -> Start Server`
    * you need to install rmate on the host
    * access host via `ssh -R 52698:localhost:52698 name@IP`
    * start a remote script like `rmate skript.sh`
    * you can rename rmate to ratom also for `ratom skript.sh`

## Installing shellcheck (corrects shell scripts)

* you need the linter package and the linter-shellcheck package  in atom
* then you install shellcheck via scoop into windows directly via powershell
* check the scoop site for scoop installation

```bash
# In windows power shell type this:
Set-ExecutionPolicy RemoteSigned -scope CurrentUser

iex (new-object net.webclient).downloadstring('https://get.scoop.sh')`

scoop install shellcheck
```

* in atom go to the shellcheck package and enter the following path to it

```bash
C:\Users\<USERNAME>\scoop\apps\shellcheck\0.5.0\shellcheck.exe
```

## Installing pylama (corrects python scripts)

* you need to install the linter and linter-python package in atom
* also you need to install pylama in python3

```bash
# installing the pip3 from python3.X (not pip from python 2.7)
sudo apt-get install python3-pip ## use the apt-get for now
pip3 install pylama pylama-pylint ## dont use sudo here
```
* add this path to the settings of linter-python:

```
C:\Users\<USERNAME>\Documents\Atom_linter_shortcuts\python.bat
```
* in python.bat write the following line `wsl python3 -m pylama`
