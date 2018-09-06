
# use git efficiently
(https://github.com/handongfeng/researchNotes/blob/master/usegit.jpg)

# git tutorial
https://www.atlassian.com/git/tutorials/

# common git command
https://confluence.atlassian.com/bitbucketserver/basic-git-commands-776639767.html

## Force to update local from remote
1. git fetch --all
2. git reset --hard review/initial-version

if you alreay in the current branch, you will update current branch
1. git fetch --all
2. git reset --hard


## update changes from local to remote
Such as you modified somethings in local file you want to push and uodate to remote.
1. git commit -am 'upate message'
2. git push

## change message 
1. git commit --amend -m "New commit message"
2. git push --force origin <BRANCH-NAME>

# set up ssh key on windows
https://www.cgranade.com/blog/2016/06/06/ssh-keys-in-vscode.html

Here are several steps:
1. Using PuTTYgen to generate a public key and provate key, saved them in C:\Users\username\.ssh
2. copy your public key in your application ssh key
3. open Pageant, if alreay open it's minimized. Right click->add key. select your private key. Then you can use ssh connect to gitlab

# Ubuntu window 10 home folder
C:\Users\<userName>\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs\home\

# create new branch from current banch and push your update
1. git checkout interested_branch_name
2. git checkout -b newDev
3. git push origin newDev
4. git push --set-upstream origin newDev
5. git commit -am 'upate message'
6. git push


# create anaconda python 3.6 env
1. conda create -n myEnvName python=3.6 anaconda
2. on linux, source activate myEnvName
3. check python version using python --version
4. or  source deactivate

# 
1. first install the package: conda install -c auto more-itertools 
2. example usage 
```
import itertools  
num = [1, 2, 3]
color = ['red', 'while', 'black']
value = [255, 256]
for a, b, c in itertools.izip(num, color, value):
    print(a, b, c)
```

