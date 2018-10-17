## mkdocs
Using this tutorial for side creation/installation:
https://mkdocs.readthedocs.io/en/stable/

Using this style:
https://github.com/mkdocs/mkdocs-bootswatch
(do a pip3 installation)

* don't change anything on the git repository for the side creation !!

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
#change stuff
# now build the side with:
mkdocs build --clean  # clean is important if a style change was done

# push all changes to your master
git add & git commit & git push ....

# push the side
mkdocs gh-deploy
```
