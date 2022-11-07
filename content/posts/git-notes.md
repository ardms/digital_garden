---
title: "Git Notes"
date: 2022-10-13T08:28:55+01:00
draft: false 
tags: ['notes','git']
---

# Github spesifics

## Pull requests

__Pull request__ is actualy a request for a review. Review allow collaborators to comment on the changes proposed in pull requests.
General infor can be found at [GitHub](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews)

Instructions:
* First you need to commit all new changes to a new branch
* Then in the github website you navigate to this brach and folow [instructions](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)

## ssh key athentication 

From [Link](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

* First generate an ssh key localy `ssh-keygen -t ed25519 -C email`
* Navigate to /settings/keys in github and clic on _New SSH key_
* __cat__ the .pub key from .ssh folder and copy it in the browser
* Make sure ssh-agent is runing in the background `$ eval "(ssh-agent -s)"`
* Add the ssh key cteated above to the agent `ssh-add ~/.ssh/id_ed25519`

# Git basics 

## Git config

* `git config --global pull.ff only`
* `git config --local user.name <your name>`

## Git commands 

* `git clone -b <branchname> <remote-repo-url>`
* 

# Git Pull and all the hell breaks loose

* `git pull` is not a single operation. Consists of `git fetch` and `git merge origin/$CURRENT_BRANCH`
* Git will only perfrom the _merge_ when there are no _uncommited_ changes. otherwise you will probably get something like:
__error: Your local changes to the following files would be overwritten by merge: ... __
* Overright local changes with remote origin: 
```
git fetch
git reset --hard HEAD
git merge origin/$CURRENT_BRANCH
```
* If you don't want to overwritte your local then you can __stash__ by:
```
git fetch
git stash
git merge origin/$CURRENT_BRANCH
git stash pop
```

# Git working with submodules

# Git pull requests
