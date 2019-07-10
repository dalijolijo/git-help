# git-help

## Define `gitlog` alias
```sh
alias gitlog='git log --graph --oneline --decorate --all'
```

## Create new remote GitHub project from bash
```sh
curl -u dalijolijo https://api.github.com/user/repos -d '{ "name": "bitcore-node-joli" }'
```

#### Option 1: Create a new repository on the command line (with signed commit)
```sh
echo "# bitcore-node-joli" >> README.md
git init
git add README.md
git commit -S -m "first commit"
git remote add origin https://github.com/dalijolijo/bitcore-node-joli.git
git push -u origin master
```

#### Option 2: Push an existing repository from the command line
```sh
git init
git add .
git commit -m "inital commit"
git remote add origin https://github.com/dalijolijo/bitcore-node-joli.git
git push -u origin master
```

## Clone GitHub repository
```sh
git clone https://github.com/dalijolijo/bisq-assets.git
cd bisq-assets
```

### Clone a Git Repository Using Private Key on Ubuntu
See http://blog.morzproject.com/post/clone-git-using-private-key/

Create a config file in .ssh folder
```sh
cd /home/USER/.ssh
touch config
```
Add this value to the config file and save.
```sh
IdentityFile /home/USER/id_rsa.key
```

## Show remote repository name
```sh
git remote -v
```

## Create new branch (local => remote)
```sh
git branch testnet
git checkout testnet
git status
git push --force --set-upstream origin testnet
```

## Change to branch `list-diamond-asset`
```sh
git checkout list-diamond-asset
```

## Show the current working branch
```sh
git branch
```

## Add remote master of my remote fork (needed?)
```sh
git remote add upstream https://github.com/bisq-network/bisq-assets.git
```

## Sync with remote repository
```sh
git pull --rebase 
```

## Make sure everything looks right
```sh
gitlog

| | | * 3513b32 (HEAD, origin/list-diamond-asset, list-diamond-asset) Apply Bisq code style
| | | * d2eb7f9 List Diamond (DMD)
```

## Squash two commits
```sh
git rebase -i HEAD^^ 
```

In the editor you change `pick` to `fixup` on the commits you want to merge with upper commit.
```sh
FROM:
pick d2eb7f9 List Diamond (DMD)
pick 3513b32 Apply Bisq code style
...

TO:
pick d2eb7f9 List Diamond (DMD)
fixup 3513b32 Apply Bisq code style
...
```

## Rename last commit text
```sh
git commit --amend
```

## Reset last X commits (Be careful!)
```sh
git checkout master
git branch
git reset --hard 6b8bd76
git push --force
```

## Reset last X commits => result in new branch 3.1
```sh
git checkout master
git branch
git checkout 1e19cf0
git checkout -b 3.1
git push --set-upstream origin 3.1
```

## Reset branch of forked project with remote branch
example: master branch
```sh
git remote add upstream https://github.com/whoever/whatever.git
git fetch upstream master
git checkout master
git reset --hard upstream/master
git clean -f -d
git push --force
```

## Remove commits - Cherry pick (Be careful!)
Example git log
```sh
Number	Hash	Commit Message	Author
1	2c6a45b	(HEAD) Adding public method to access protected method	Tom
2	ae45fab	Updates to database interface	Contractor 1
3	77b9b82	Improving database interface	Contractor 2
4	3c9093c	Merged develop branch into master	Tom
5	b3d92c5	Adding new Event CMS Module	Paul
6	7feddbb	Adding CMS class and files	Tom
7	a809379	Adding project to Git	Tom
```
Say we want to remove commits 2 & 4 from the repo.

* `git checkout b3d92c5` Checkout the last usable commit.
* `git checkout -b repair` Create a new branch to work on.
* `git cherry-pick 77b9b82` Run through commit 3.
* `git cherry-pick 2c6a45b` Run through commit 1.
* `git checkout master` Checkout master.
* `git reset --hard b3d92c5` Reset master to last usable commit.
* `git merge repair` Merge our new branch onto master.
* `git push --force origin master` Push master to the remote repo.

## Push to remote branch (Be careful!)
```sh
git push --force

Counting objects: 1, done.
Writing objects: 100% (1/1), 203 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To https://github.com/dalijolijo/bisq-assets.git
 + 3513b32...bcc60b5 list-diamond-asset -> list-diamond-asset (forced update)
```

# How do I update a GitHub forked repository
Add the remote, call it "upstream":
```sh
git remote add upstream https://github.com/whoever/whatever.git
```

Fetch all the branches of that remote into remote-tracking branches, such as upstream/master:
```sh
git fetch upstream master
```

Make sure that you're on your master branch:
```sh
git checkout master
```

Rewrite your master branch so that any commits of yours that aren't already in upstream/master are replayed on top of that other branch:
```sh
git rebase upstream/master
```

If you've rebased your branch onto upstream/master you may need to force the push in order to push it to your own forked repository on GitHub. You'd do that with:
```sh
git push -f origin master
```

# SSHFS mount of shared host directory 
Login on private host (stored SSH key) to use git push via SSH. 
```sh
sudo apt-get install sshfs
sudo sshfs -o allow_other,IdentityFile=~/.ssh/id_rsa root@<IP_SHARED_HOST>:/opt/<Project> /mnt/
cd /mnt/
git remote set-url origin git@github.com:<Username>/<Project>.git
git push
```
Hit: Add SSH pub key to git repo

# Git with meld (Windows)
See http://meldmerge.org/ and https://gitforwindows.org

Install GitBash and set:
```sh 
git config --global merge.tool meld
git config --global mergetool.meld.path "C:\Program Files (x86)\Meld\Meld.exe" <- path to meld here
```

Usage: 
```sh
git diff
```

# Example: electrum
```sh
git clone https://github.com/dalijolijo/electrum-bitcore.git
cd electrum-bitcore
git remote add upstream https://github.com/dalijolijo/electrum.git
git fetch upstream
git checkout 3.2.3
git rebase upstream/3.2.3
```

Fix conflict in electrum/version.py
```sh
git add .
git rebase --continue
```

Fix all conflicts:
* MANIFEST.in: needs merge
* electrum: needs merge
* electrum-bitcore: needs merge
* electrum.desktop: needs merge
* electrum/blockchain.py: needs merge
* electrum/checkpoints.json: needs merge
* electrum/constants.py: needs merge
* electrum/gui/qt/amountedit.py: needs merge
* electrum/gui/qt/exception_window.py: needs merge
* electrum/gui/qt/main_window.py: needs merge
* electrum/network.py: needs merge
* electrum/servers.json: needs merge
* electrum/util.py: needs merge
* run_electrum: needs merge
* setup.py: needs merge

```sh
git add .
git rebase --continue
```
