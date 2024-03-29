# GIT NOTES

## Installation (LINUX)

through apt:  
```bash
$ sudo apt update  
$ sudo apt upgrade  

$ sudo add-apt-repository ppa:git-core/ppa
$ sudo apt update
$ sudo apt install git
```

then verify with `git --version`

---
## Config & Credentials

```bash
# this will open in the configured editor (core.editor)
$ git config --global --edit

$ git config --global user.name "blabla"
$ git config --global user.email "blabla@lablab.com"
$ git config --global color.ui auto
$ git config --global core.editor "code --wait"

# default branch should be main, not master:
git config --global init.defaultBranch main

# to check them later on:
git config --get (...)
```

### ssh setup:  
(also on [GitHub docs: Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh))

```bash
# check if you already have an ssh key
$ ls ~/.ssh/id_ed25519.pub

# if not, generate one, and copy the contents
$ ssh-keygen -t ed25519 -C [email address]
$ cat ~/.ssh/id_ed25519.pub
```

then on github navigate to "Settings > SSH and [...] > new ssh key"  
paste the SSH key in there (including the email)

verify that ssh works correctly:

```bash
$ ssh -T git@github.com
```

verify the fingerprint that comes up in the prompt:  
[GitHub docs: SSH Key Fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)  

if it is correct, proceed


---
## Basic Git Workflow

Three states of Git:
- Working directory _(modified)_
- Staging area _(staged)_
- Repository _(committed)_

`git status` to check the situation  
`git add .` will add all files to staging area (otherwise just type file name)  
`git commit` on its own will open the text editor for the message.  

if it's a short commit with just a subject you could do `git commit -m "commit message"`  
or also directly `git commit -am "message"` to not go through staging

`git log` to check commits  
add `--oneline` to see just the subjects of the commits  
`-<number>` will limit the display to that many commits  

`git push` will update the server
`git push origin main` would be the long version, in case of branches


---
## New Repository

Two ways to go about it:
1. Create a GitHub repo and clone it on your computer.
2. Create a Git repo locally, then push it to a new empty GitHub repo.

create a new repo on github (enable README.md)  
navigate to Code (green button) > Clone > SSH, copy the key  
(git@github.com:USER-NAME/REPOSITORY-NAME.git)  

then go to terminal, cd into wherever you want your repo folder,  
then git clone [paste key]. git status to check if all good (also git remote -v)  

alternatively, create a new repo locally (mkdir, git init, touch README.md)  
then create new repo  
then import repo to github  


---
Pretty logs

git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
^ is what i got from the internet

improvements: 
    never mind the graph
    make the date YYYY-MM-DD (%cs instead of %cr)
    skip the hash? maybe put date there
    <%an> in the end is cool (author name)

so it looks like this:
    git log --pretty=format:"%C(magenta)%cs |%Creset⁠%C(bold yellow)%d%Creset %s %C(red)<%al>%Creset" --abbrev-commit

from the manual;
    You can set the color to any of the following values: normal, black, red, green, yellow, blue, magenta, cyan, or white. 
    If you want an attribute like bold in the previous example, 
    you can choose from bold, dim, ul (underline), blink, and reverse (swap foreground and background).


---
Git Diff

git diff --help gives a lot of insight
but mainly you can diff in different states
if you staged all files but want to do a quick check, do git diff --cached
git diff on its own is for unstaged changes, i guess?

$ git diff HEAD~1 HEAD
this will give you what you changed on your last commit to HEAD


---
Rollback and modification

Rollback commands: RESTORE, RESET, REVERT, REBASE

$ git restore index.html discards uncommitted changes to a file
you can also restore a specific historic revision with --source [id | ^ | branch~#]
$ git restore --staged *.css : unstages a file (removes from staging area/index)

$ git commit --amend will let you edit the last commit
if you staged some changes, --amend will add those to the previous commit
adding --no-edit will not make you go through the commit message

!!! DO NOT AMEND A PUBLIC COMMIT:
--amend creates an entirely new commit
so if you push and then amend and push again, you'll have a conflict in your hands.
amending makes things cleaner WHILE THEY ARE STILL LOCAL,
otherwise you need merge conflict resolutions that end up being even lengthier.

if you want to look at previous commits, use checkout:
e.g. git checkout tags/v0.1.0
this creates a detached head

git rebase can be used to squash multiple commits into one,
whereas git reset can be used to split a single commit into many.

DON'T MESS WITH git reset --hard; it is highly destructive


---
## Branches

`git branch` lists branches (-a for all)  
`git branch <branchname>` will create new branch locally  

`git checkout` for changing branches  
`git checkout -b <branchname>` creates a new branch and checkout into it  
`git push -u origin <branchname>` to create the new branch on remote as well  

to return, `git checkout main`

`git merge <branchname>` takes the changes from `<branchname>`  
and puts them inside your **CURRENT BRANCH**

after merging, `git branch -d <branchname>` to delete  
then `git push origin --delete <branchname>` to delete from github

**migrating from master to main:**

> - create main branch locally, taking the history from master
> git branch -m master main
> 
> - push the new local main branch to the remote repo (GitHub) 
> git push -u origin main
> 
> - switch the current HEAD to the main branch
> git symbolic-ref refs/remotes/origin/HEAD refs/remotes/origin/main
> 
> - change the default branch on GitHub to main
> - https://docs.github.com/en/github/administering-a-repository/setting-the-default-branch
> 
> - delete the master branch on the remote
> git push origin --delete master



---
Git tags & Semantic versioning

a commit can be tagged as such:  
git tag -a <tag-name> -m "Write description here"

you can also do git log --oneline (optionally also -5 or -10 or however far you want to go)
and find the hash of an older commit
then do the following:
git tag -a <tag-name> 

git push does not push tags. for that: 
git push --tags
or if you want to do push both tags AND commits:
git push --follow-tags

https://git-scm.com/book/en/v2/Git-Basics-Tagging
https://travishorn.com/semantic-versioning-with-git-tags-1ef2d4aeede6

git tags appear on "releases" part of github
that's why it works well for semver (0.1.0, 0.2.0, 1.0.0 etc etc)

you can access these tags by typing tags/<tagname>
e.g. git checkout tags/v0.1.0


---
Git conflicts

Conflicts happen when there is at least one new commit in local repo,
and one new commit in remote, effectively creating two different versions

Then you do a git pull and receive a conflict message.
Git creates flags in the conflicted file:
<<<<<< HEAD ======== MAIN >>>>>>>>
just search for one of those

Then delete the incorrect version AND the conflict markup
VSCode has buttons: "Accept current change" etc etc, easier with one click

Then finally git add <conflicting-file>, git commit, git push
(or git merge --abort to quit)


---
Misc Git Notes

commits should be concise and give CONTEXT.
The diff can show you what changed, but only the commit can tell you why.

atomic commits in every meaningful code change.
if you're manipulating many files at once, consider leaving some out during staging.
don't just automatically git add . everytime!

the more you care about the log, the more it actually gets useful
and vice versa, vicious cycle of not caring and not using it

you can have both subject and body on a commit message,
separate the subject with a blank line (50 chars tops)
so that git log --oneline gives you just the subjects

use IMPERATIVE TONE for subject and capitalize ("Fix bug" and not "fixed bug.")

Git ignore:
    create a file (.gitignore) on the root
    add all the files/directories/etc you want GIT to ignore
    simplest form: just the file (e.g. NOHUP.out)
    an entire directory could be selected by using forward slash (my_directory/)
    *: match any number of characters including 0 (*.ttf will ignore all fonts)
    ?: match any single character
    **: match any number of directories

there is a way to untrack a file that had been tracked before:
    git rm --cached FILENAME


