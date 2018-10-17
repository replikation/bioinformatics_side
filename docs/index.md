Home
===
____

# Bioinformatics Collection

This website is a collection of lectures and tutorials for me.

Created/Collected by Christian Brandt.



## Table of Content

see navigation above

## mkdocs
Using this tutorial for side creation:
https://mkdocs.readthedocs.io/en/stable/

Using this style:
https://github.com/mkdocs/mkdocs-bootswatch

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
mkdocs build

# push all changes to your master
git add & git commit & git push ....

# push the side
mkdocs gh-deploy
```

## License

Unless stated otherwise, all is licensed under the Creative Commons Attribution 4.0 International License.
To view a copy of this license, visit <http://creativecommons.org/licenses/by/4.0/> or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.
