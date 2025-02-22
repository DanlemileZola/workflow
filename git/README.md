# Introduction

*In addition to `git`, I installed `gh` which is a CLI interface for github. I will not present how to configure its allow us to skip some step once done.* 
*GitHub as the database will be refered as GH.*
*The following instructions are considering we want to work and therefore synchronise from the local machine.* 

# Basics

## Creation / Clone

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

In order to clone a remote repository on the local machine : 
```bash
# Download on local.
git clone https://github.com/username/repo_name.git
```
By default, folder on local is named 'repo_name' but that can be changed to anything since git use the .git to get its info. 
## Common usage

### Push

Basics manipulation include to pick which files we want to incorpore in the push. Therefore : 
```bash
git add . 
git commit -m "Message"
git push -u origin main 
# git push will be enough after set the first time.
```
### Pull 

2 options here : the fast way and the slighty less fast way. 

```bash
# The Path OF The Wise
git fetch origin # Get updates from the remote repo w/o applying
git merge origin/branch-name # Apply the updates
```
or 
```bash
# 
git pull origin branch-name # fetch & merge in one command. 
```

# Question mark 

## How to check update from remote before merging ? :
```bash
# That allow you to check if local branch is behind remote
git status 
```

```bash
# Show latest commit in each branch
 git branch -v 
```

```bash
# Compares local branch (HEAD) with the fetched remote branch (main in that case)
git diff HEAD origin/main

# or to see line-by-line changes in files : 
git diff origin/main
```

```bash
# See detailed commit history
git log HEAD origin/main
# There is option to log commmand to make it more appealing. In my case, I created a 'lg' alias (replace log).
```

## How to change a remote repository name ? 

```bash
# While being in the matching local repository. 
gh repo edit --name NEW_NAME 
```

And then, to update the URL linked to the local repository : 

```bash
# Oherwise pull and puss will raised error
git remote set-url origin https://github.com/yourusername/new-repo-name.git
```
