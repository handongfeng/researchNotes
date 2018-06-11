
# common git command
https://confluence.atlassian.com/bitbucketserver/basic-git-commands-776639767.html

Force to update local from remote
git fetch --all
git reset --hard review/initial-version

if you alreay in the current branch, you will update current branch
git fetch --all
git reset --hard


# set up ssh key on windows
https://www.cgranade.com/blog/2016/06/06/ssh-keys-in-vscode.html

Here are several steps:
1. Using PuTTYgen to generate a public key and provate key, saved them in C:\Users\username\.ssh
2. copy your public key in your application ssh key
3. open Pageant, if alreay open it's minimized. Right click->add key. select your private key. Then you can use ssh connect to gitlab

# Ubuntu window 10 home folder
C:\Users\<userName>\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs\home\
