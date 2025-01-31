### 1. Initialization (_solo_)[](https://github.com/becodeorg/BXL-kamkar-03/tree/main/content/00.Other/01.Cooperative_Git#1-initialization (solo))


- **fork[^1] the repository on your Github account :**
`gh repo fork https://github.com/becodeorg/BXL-kamkar-03/`
    *Using CLI, you will be ask to clone or not the repository. Do so, and then you need to get on that newly create folder.*
`cd ~/BXL-kamkar-03`
- **initialize a file called _poem.md_ at it's root :** 
`touch poem.md`
- **give a title to your story :** 
`nano poem.md`[^2]
    *There you would be able to edit the file to add the title*
- **clone the repository :** 
    *Using CLI, it's already done before*
- **create a _development_ branch :** 
`git switch -c development`[^3]
- **go to that branch :** 
    *Using CLI, already made on previous step*
- **write 3 lines of text to begin the story :**
    *Again, we go with `nano poem.md`*
- **commit/push the changes to the branch *development* :**[^4]
*That require multiple step :*
    *- Stage the file :* `git add poem.md`
    *- Commit the changes :* `git commit -m "Initial story"`
    *- Push the change to the development branch :* 
    `git push -u origin development`

- **invite the other members of the group as contributors** : 
    *While possible using CLI, it needs extra tools so we will go through web interface*.
    When on your repo : Settings > Collaborators and Teams > Add people.
    For this exercice, you should put them with "write" privilege.

[^1]: fork is a copy of the original (in that case repository). Working on a fork won't affect the original version. On git, cloning means create a copy **on the local machine**.
[^2]: nano : when edit is finish, just use CTRL+X then `y`to confirm the change.
[^3]: While `switch`is made to change branch when they already exist, the `-c`option create a new one.
[^4]: Explanations for this walk-trough are too complicate for a footnote. See below to understand git workflow. 


![Screencast from 2024-10-10 16-42-30](Screencast%20from%202024-10-10%2016-42-30.webm)




### 2. Contributions[](https://github.com/becodeorg/BXL-kamkar-03/tree/main/content/00.Other/01.Cooperative_Git#2-contributions)



- check a repository where you have been invited

- clone said repository
`git clone https://github.com/anotherkimnguyen/cadavre_exquis`

- list the existing branches
`git branch`
     *Only show `main`branch while there is a `development`branch on the original repo.*
     *It's because, by default, when you clone a repository, only the default branch get checkout locally. Therefore, we need to fetch all remote branches. First, to be sure of others branch, we use :*
     `git branch -r`
     *That will show all remote branches. We obtain :* 
     
       origin/HEAD -> origin/main  
       origin/development  
       origin/main  
*Since we want only the `development`branch, we use :* 
     `git fetch origin development`

- go to the branch _development_:
     `git switch development` 

- create a branch called _your-name_ from there:
     `git switch -c danlemilezola
- write 3 lines of text in the `poem.md` file to begin the story :
     *We can use `nano`for that*
- commit the changes to the branch _your-name_ [^4]: 
    ```
    git add poem.md
    git commit -m "Add Jeremy's line in poem.md from his branch"
    ```
- merge your branch with _development_ [^5]: 
     *To do so, we need to be on the `development`branch.*
     `git switch development`
     *Then we can merge our own branch with it.*
     `git merge danlemilezola
- push the branch _development_ : 
     `git push origin development`
- do this for all members of the group

[^5]: Merging conflict : 

### 3. Versioning 
[](https://github.com/Aziz-KherBek/BXL-kamkar-03/tree/development/content/00.Other/01.Cooperative_Git#3-versioning)


- go back to your repository when all your colleagues are done
- merge _development_ with the branch _main_ : 
    *To do so, i had to resolve some problem. To start, i had to pull remote version to be sure i was up-to-date on my local machine :* 
    *`git pull origin main`*
    *Then i resolved the merging conflict by editing the file then commit the change and push them.*
    *All that being done on the `main` branch so far.*
    *Then i did the same on* `development`*.
    *Once all have done, i could finally merge development into main :* 
    `git merge main development`
    *And again resolve merging conflict.* 
- make a tag of _main_ called _version-1_ : 
    `git tag version-1`


### 4. Correction

[](https://github.com/Aziz-KherBek/BXL-kamkar-03/tree/development/content/00.Other/01.Cooperative_Git#4-correction)

- create a new branch _corrections_ from _main_ : 
    `git switch -c corrections`
- correct all misspellings (_if there are none add the mention perfect_)
- commit/push : 
    *After commit, i had to create the upstream branch[^6]  :* 
    `git push --set-upstream origin corrections`
- merge with _main_ : 
    *We need to `switch` to `main`to do so*
- delete all the branches except _main_ [^7] :
   ```
    git branch -d corrections development
    git push origin --delete corrections development
     ```

[^6]:upstream branch : it's the branch that the local version track on the remote repository. 
[^7]: First command is to delete branch on local machine, second to delete the ones on the remote. 


`git switch`: allow to switch between branch. Using `-c`option, you create a new branch and switch to it. 

`git restore`: 
     Severals use depending of uses cases :
     - `git restore filename`If changes have been made to filename but haven't been staged. 
     - `git restore --source=<commit-hash> filename` Restoring a file from a specific commit. 
     - `git restore --staged filename` to unstage a file


`git branch` : show available branch 


what is 'origin' ? 
`origin` is the **default name** given to the remote repository when you clone a repository from GitHub (or another Git server). It is an alias that points to the URL of your remote repository, and Git uses this to identify where to push and pull changes from.

what is '-u' ? 
In 'git push -u origin development', the -u stand for upstream. Its sets the default upstream tracking branch for your local branch. This means that Git will "remember" which remote branch your local branch should push to in future commands.
- When you run `git push` with `-u` for the first time, you're telling Git to **link** your local branch with the corresponding remote branch on GitHub (or another remote).
- After using `git push -u`, future `git push` and `git pull` commands can be simplified. Instead of specifying the remote (`origin`) and branch each time, Git knows where to push or pull changes from by default.

git checkout -t origin/development ????
   Now you will only have the `development` branch fetched and checked out locally, without fetching the other branches from contributors. Let me know if you need any more clarification!

`fetch`VS `pull` : 
     `fetch`download changes (commits, branches, tags) from the remote repository they **do not integrate them** into your working branch (on local machine). So it updates the local copy of the remote branches (like origin/main).
     `pull` does apply the changes directly on the working branch. So it's equivalent to `fetch`then `merge`. 

rename a repo : 
     Being on the right folder on your local machine, you can use `gh repo rename new-repo-name`.
     After renaming, any local clones of the old repo will still point to the old URL. Therefore, use `git remote set-url origin https://github.com/yourusername/new-repo-name.git` to update the remote URL in your 

`git commit --amend -m "Add hackaday.txt"` : 
    To change the commit message **from the last commit**. Since it's create a new ID for the commit, it has to be used **before** pushing the commit. 

`git rebase -i <commit-hash>`:
     `-i`to open interactive mode. Open a editor with a 'script'. 

`git reset <commit-hash>`: 
     Undo changes by moving the current branch and optionnally modifying the staging area (index) and working directory. Different mode : 
     `--soft`to keep all changes in index and working directory but re-commit them differently. 
     `--mixed` (default) changes in working directory still there, but they are unstaged.
     `--hard` all changes are lost (index and working directory)

~/.gitconfig : path to git configuration file.[](https://jr0cket.co.uk/2013/06/designing-your-own-commit-graph-with-git.html)
     