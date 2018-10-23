Git via github.com
===
____

# 1. Github.com
* create a repository at [github](https://github.com/)
* copy the link for cloning a repository
* locally type `git clone <git_URL>`
* get updates via `git pull` inside the git repository

## 1.1 Local Repository

** step by step**

* use `git init` to create a git directory in any directory you want
* this is the master directory, so every sub directory is included
* at [github](https://github.com/), follow the description to link it to your local folder
* `gitstatus` always tells you what to do
* you add things you want to commit using `git add <file>`
* `git commit -m "message"` to commit your changes
* `git push origin master` pushes commited file to internet git repository
* `git pull` to get a update from github
* `nano .gitignore` write filenames in .gitignore that you don't want to be added or pushed

## 1.2 Installing Gits
* Download and compile new programs or git clones into `~/install/`
* then put the path to `~/.bashrc`:
    ```b
    # do nano ~/.bashrc and add
    export PATH="/home/replik/install/canu/Linux-amd64/bin:$PATH"
    ```
*   Search the programs in github
*   the `make` or `configure` steps are written there or in the `README.md`. Do a `make clean` if you get errors during `make` before doing a new `make`

## 1.3 Git sides
* this is basically a website hosted via github
* all files are maintained via the master
* can be easily created with mkdocs
* [see this side](../tools/mkdocs.md)
