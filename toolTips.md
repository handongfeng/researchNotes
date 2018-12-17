
# Quit git log or git diff with:   in cmd: type q to exit this screen


# use vs code as git diff tool

https://stackoverflow.com/questions/30024353/how-to-use-visual-studio-code-as-default-editor-for-git


1. in the vs command line input: git config --global core.editor "code --wait"

2. Add the following to enable support for using VS Code as diff tool: 

```
[diff]
    tool = default-difftool
[difftool "default-difftool"]
    cmd = code --wait --diff $LOCAL $REMOTE
```

3. git difftool location\filename to compare your file


4. compare the working directory with local repository. 

git diff HEAD [filename] 

example: below command compare  the latest commit with the local one for test.py
```
git difftool HEAD .\src\test.py  
```

for a file "main.c" between now and two commits back, here are three equivalent commands:
```
$ git diff HEAD^^ HEAD main.c
$ git diff HEAD^^..HEAD -- main.c
$ git diff HEAD~2 HEAD -- main.c
```

# use git efficiently
(https://github.com/handongfeng/researchNotes/blob/master/usegit.jpg)

# git tutorial
https://www.atlassian.com/git/tutorials/

# common git command
https://confluence.atlassian.com/bitbucketserver/basic-git-commands-776639767.html

## get remote branch to a totally new local
https://confluence.atlassian.com/bitbucket/checkout-a-branch-into-a-local-repository-313466957.html

1. git clone https://xxxx.git
2. git branch -a to list all your branches, yould see 

You should see something similar to the following. Notice that it lists both the branches that are local and the remote branches on Bitbucket. Using the list as reference, choose the branch you want to checkout.  In this example, the feature branch is the branch.

```python
* master   <feature_branch>
  remotes/origin/<feature_branch>
  remotes/origin/master
```
3. git fetch origin
   and git branch -r to see available branches
3. Checkout the branch you want to use.
```python
git checkout <feature_branch>
```
4.Confirm you are now working on that branch:
```python
git branch
```

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



# Django 2.x db setting with MS SQL SERVER

```
DATABASES = {
    'default': {
        'ENGINE': 'sql_server.pyodbc',
        'HOST': '10.8.100.154', ### the db server address
        'NAME': 'master',
        'Trusted_Connection': 'yes',
        'OPTIONS': {
            'host_is_server': True,
            'driver': 'SQL Server',
        },
    }
}
```



python logging example using yaml format


version:
  1
formatters:
  simple:
    format: '[%(asctime)s] (%(filename)20s:%(lineno)4s) [%(levelname)8s] - %(message)s '
    datefmt: '%Y-%m-%d %H:%M:%S'
  complex:
    format: '[%(asctime)s] (%(filename)20s:%(lineno)4s) [%(levelname)8s] - %(message)s '
handlers:
  console:
    class: logging.StreamHandler
    formatter: simple
    level: DEBUG
    stream: ext://sys.stdout
  debug:
    class: logging.handlers.RotatingFileHandler
    formatter: complex
    level: DEBUG
    filename: ./files/example.10.debug.log
    mode: a
    maxBytes: 25000
    backupCount: 3
  info:
    class: logging.handlers.RotatingFileHandler
    formatter: complex
    level: INFO
    filename: ./files/example.20.info.log
    mode: a
    maxBytes: 25000
    backupCount: 3
  warning:
    class: logging.handlers.RotatingFileHandler
    formatter: complex
    level: WARNING
    filename: ./files/example.30.warning.log
    mode: a
    maxBytes: 5000
    backupCount: 6
  error:
    class: logging.handlers.RotatingFileHandler
    formatter: complex
    level: ERROR
    filename: ./files/example.40.error.log
    mode: a
    maxBytes: 5000
    backupCount: 6
  critical:
    class: logging.handlers.RotatingFileHandler
    formatter: complex
    level: CRITICAL
    filename: ./files/example.50.critical.log
    mode: a
    maxBytes: 5000
    backupCount: 6
  custom:
    class: logging.handlers.RotatingFileHandler
    formatter: complex
    level: CRITICAL
    filename: ./files/example.60.custom.log
    mode: a
    maxBytes: 5000
    backupCount: 6
  email:
    class: logging.handlers.SMTPHandler
    formatter: complex
    level: CRITICAL
    mailhost: 
      - smtp.comcast.net
      - 25 
    fromaddr: southfork.scherer@comcast.net
    toaddrs: jscherer26@gmail.com
    subject: Example Program Critical Log
    credentials: 
      - southfork.scherer
      - Cc11012003
    secure: True
  memory:
    class: logging.handlers.MemoryHandler
    formatter: complex
    level: DEBUG
    flushLevel: ERROR
    target: error
    capacity: 25
  history:
    class: logging.handlers.HistoryHandler
    formatter: complex
    level: DEBUG
    flushLevel: CRITICAL
    target: custom
    capacity: 15
loggers:
  err:
    level: ERROR
    handlers: [error]
root:
  level: NOTSET
  handlers: [console, debug, info, warning, error, critical, history, email ]
