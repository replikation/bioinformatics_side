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

# Cheatsheet

* here is one, [click me](https://education.github.com/git-cheat-sheet-education.pdf)

# 2. Branching

* on a local master do
* creates the branch "testside"

````bash
# create a branch
git checkout -b testside
# Edit, add and commit your files.
git push -u origin testside

# commiting and push normally but use this:
git push origin testside # not master
````

* merging branch with master

````bash
# change to the master branch
git checkout master
# merge the testside branch INTO the master
git merge testside
# push the new master to github
git push origin master
# cheange back to testside via
git checkout testside
````

# 3. Working with ssh keys

a) create key via `ssh-keygen -t rsa -b 4096 -C "your_email@domain.com"` if you dont have one
b) copy the `*.pub` key to github.com (account - settings - ssh und gpg keys)
c) do `nano ~/.ssh/config` and add
```bash
host github.com
 HostName github.com
 IdentityFile ~/.ssh/id_rsa
 User git
```
d) ``chmod 660 .ssh/config``

* you need to clone with ssh not http on github.com (small button at the clone window)

```bash
# looks like this
git clone git@github.com:nanozoo/bx_guppy_gpu.git
```

e) change http to git@

`git remote set-url origin git@github.com:USERNAME/REPONAME.git`
