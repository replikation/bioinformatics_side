# Github.com

* to create a git repository follow the  introductions at [github](https://github.com/)
* to get a new git repository (not yours) for the first time use:
`git clone <git_URL>`

## Create Repository

** step by step**

* create git repository at [github](https://github.com/)
* follow the description to link it to your local folder
* use `git init` to create a git directory in any directory you want
* this is the master directory, so every sub directory is included
* `gitstatus` always tells you what to do
* you add things you want to commit using `git add <file>`
* `git commit -m "message"` to commit your changes
* `git push origin master` pushes commited file to internet git repository
* `git pull` to get a update from github
* `nano .gitignore` write filenames in .gitignore that you don't want to be added or pushed

## Installing Gits
* Download and compile new programs or git clones into `~/install/`
* then put the path to `~/.bashrc`:
    ```b
    # do nano ~/.bashrc and add
    export PATH="/home/replik/install/canu/Linux-amd64/bin:$PATH"
    ```
*   Search the programs in github
*   the `make` or `configure` steps are written there or in the `README.md`. Do a `make clean` if you get errors during `make` before doing a new `make`
