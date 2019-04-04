mkdocs
===
___

# 1. Overview

* Use the `mkdocs` tutorial for site creation [click me](https://mkdocs.readthedocs.io/en/stable/)
* Used the site style template frome here: [click me](http://mkdocs.github.io/mkdocs-bootswatch/)

**Installation:**

```bash
# installing mkdocs
sudo apt install mkdocs

# installing styles
pip3 install mkdocs-bootswatch
```

* don't change anything on the git repository, it should automatically do every thing after the first `mkdocs gh-deploy` !!
* it uses the branch pushed via `mkdocs gh-deploy` for the website

# 2. Practical steps
## 2.1 First creation
```bash
# create repository via github.vom
# git clone it to the local machine and do on the repository folder:
mkdocs new <repositoryname>
cd <repositoryname>
# change a few things in the mkdocs.yml
# create markdown-files under docs/

# now build the site with:
mkdocs build --clean

# push all changes to your master
git add & git commit & git push # and so on

# push the site
mkdocs gh-deploy
```
## 2.2 Working with it, daily

```bash
# change files
# now build the site with:
mkdocs build --clean  # clean is important if a style change was done

# push all changes to your master
git add & git commit & git push ....

# push the site
mkdocs gh-deploy
```

# 3. Themes and Navigation

* you need to `pip3 install` themes and add their name to the `theme:` line
* `mkdocs build --clean` will remove old stuff and gather stuff for the new theme

**Example mkdocs.yml:**
```bash
site_name: Bioinformatics # your site name
theme: yeti # enter the theme name here
nav: # nav bar at the top of site

- Shell: #name of drop down menu
        - Basics: # second drop down menu
              - Commands: shell/shell.md # content under Shell/Basics/
              - Cloud Computing: shell/shell_adv.md # content under Shell/Basics/
        - File Manipulation (fasta): shell/fasta.md # content under Shell/
        - Git: shell/git.md
- Python:
        - Basics: python.md
- R:
        - Basics: R.md
```
