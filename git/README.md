## Introduction

*Currently using debian, the command will be set accordingly. 
In addition to `git`, I installed `gh` which is a CLI interface for github. I will not present how to configure its allow us to skip some step once done. 
GitHub as the database will be refered as GH.
The following instructions are considering we want to work and therefore synchronise from the local machine. 
An other software that i installed is `git-run`(or `gr`) in order to run commands accross multiple repos. That's probably not a good practice but I reserve that for my obsidian's notes only which are on GH more as showcase than for proper versioning*
## Creation

In order to create a new repositories on GH, first operation is to create a local folder and then initialize it. 
Then we have to create that repo on GH.
Since we used `gh` local and online repos are already linked. 
```bash
# Set local workspace
mkdir new_repo && cd new_repo
git init 

# Create repo on GH
gh repo create new_repo_GH --public --source=. --remote=origin

```

Note that it is not necessary for the local folder to be named according to the repo on GH since we set the source folder. 
## Regular usage

### Push

Basics manipulation include to pick which files we want to incorpore in the push Therefore : 
```bash
git add . 
git commit -m "Message"
git push -u origin main 
```
### Pull 


##

##
