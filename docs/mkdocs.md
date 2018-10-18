## mkdocs
Used `mkdocs` tutorial for site creation:
https://mkdocs.readthedocs.io/en/stable/

Using this style:
https://github.com/mkdocs/mkdocs-bootswatch
(do a `pip3` installation)

* don't change anything on the git repository, it should automatically do every thing after the push !!
* it uses the branch pushed via mkdocs for the website

### First deployment
```bash
# create rep on github
# git clone it and do on the repository:
mkdocs new <repositoryname>
cd <repositoryname>
# change a few things in the mkdocs.yml
# create markdowns

# now build the side with:
mkdocs build

# push all changes to your master
git add & git commit & git push ....

# push the side
mkdocs gh-deploy
```
### change things
```bash
# change files
# now build the side with:
mkdocs build --clean  # clean is important if a style change was done

# push all changes to your master
git add & git commit & git push ....

# push the side
mkdocs gh-deploy
```
