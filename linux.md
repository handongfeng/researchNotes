screen command

# screen 命令简单用法

现在很多时候我们的开发环境都已经部署到云端了，直接通过SSH来登录到云端服务器进行开发测试以及运行各种命令，一旦网络中断，通过SSH运行的命令也会退出，这个发让人发疯的。

好在有screen命令，它可以解决这些问题。我使用screen命令已经有三年多的时间了，感觉还不错。

## 新建一个Screen Session

```
$ screen -S screen_session_name
```

## 将当前Screen Session放到后台

```
$ CTRL + A + D
```

## 唤起一个Screen Session

```
$ screen -r screen_session_name
```

## 分享一个Screen Session

```
$ screen -x screen_session_name
```

通常你想和别人分享你在终端里的操作时可以用此命令。

## 终止一个Screen Session

```
$ exit
$ CTRL + D
```

## 查看一个screen里的输出

当你进入一个screen时你只能看到一屏内容，如果想看之前的内容可以如下：

```
$ Ctrl + a ESC
```

以上意思是进入Copy mode，拷贝模式，然后你就可以像操作VIM一样查看screen session里的内容了。

可以 Page Up 也可以 Page Down。

## screen进阶

对我来说，以上就足够了，有特定需求时再说。

## screenrc

[.screenrc](.screenrc)

如果没有就在~下新建该文件

然后添加一行配置

```
escape ^Bt
```

表示其他(例如ctrl+B)替换掉默认的ctrl+A, 

因为ctrl+A往往是快速回到命令的头部的快捷键，非常常用和方便

## End

screen命令很好用，但是最让人头痛的是`CTRL+A`命令和BASH里的快捷键重复了，我不觉得替换一下快捷键是个很好的解决方案，所以这个问题一直存在我这里。

1. kill a screen session
https://askubuntu.com/questions/356006/kill-a-screen-session

First you need to re attach to the screen session
screen -r 23520 as you have done. Then press ctrl + a and then a k and press y when it asks if you really want to kill the session

2. https://gist.github.com/jctosta/af918e1618682638aa82


# Screen Quick Reference

## Basic

| Description 				| Command 				|
|---------------------------------------|---------------------------------------|
| Start a new session with session name | `screen -S <session_name>`		|
| List running sessions / screens	| `screen -ls`				|
| Attach to a running session		| `screen -x`				|
| Attach to a running session with name	| `screen -r <session_name>`		|
| Detach a running session		| `screen -d <session_name>`		|

## Escape Key

All screen commands are prefixed by an escape key, by default Ctrl-a (that's Control-a, sometimes written ^a). To send a literal Ctrl-a to the programs in screen, use Ctrl-a a. This is useful when when working with screen within screen. For example Ctrl-a a n will move screen to a new window on the screen within screen. 

## Getting Out

| Description				| Command						|
|---------------------------------------|-------------------------------------------------------|
| detach 				| `Ctrl-a d`						|
| detach and logout (quick exit) 	| `Ctrl-a D D`						|
| exit screen 				| `Ctrl-a :` quit or exit all of the programs in screen.|
| force-exit screen 			| `Ctrl-a C-\` (not recommended) 			|

## Help

| Description	| Command			|
|---------------|-------------------------------|
| See help	| `Ctrl-a ?` (Lists keybindings)|

## Window Management

| Description				| Command								|
|---------------------------------------|-----------------------------------------------------------------------|
| Create new window 			| `Ctrl-a c`								|
| Change to last-visited active window 	| `Ctrl-a Ctrl-a` (commonly used to flip-flop between two windows)	|
| Change to window by number 		| `Ctrl-a <number>` (only for windows 0 to 9)				|
| Change to window by number or name 	| `Ctrl-a ' <number or title>`						|
| Change to next window in list 	| `Ctrl-a n` or `Ctrl-a <space>`					|
| Change to previous window in list 	| `Ctrl-a p` or `Ctrl-a <backspace>`					|
| See window list 			| `Ctrl-a "` (allows you to select a window to change to)		|
| Show window bar 			| `Ctrl-a w` (if you don't have window bar)				|
| Kill current window 			| `Ctrl-a k` (not recommended)						|
| Kill all windows 			| `Ctrl-a \` (not recommended)						|
| Rename current window 		| `Ctrl-a A`								|

## Split screen

| Description				| Command								|
|---------------------------------------|-----------------------------------------------------------------------|
| Split display horizontally 		| `Ctrl-a S`								|
| Split display vertically 		| `Ctrl-a \|` or `Ctrl-a V` (for the vanilla vertical screen patch)	|
| Jump to next display region 		| `Ctrl-a tab`								|
| Remove current region 		| `Ctrl-a X`								|
| Remove all regions but the current one| `Ctrl-a Q`								|

## Misc

| Description						| Command								|
|-------------------------------------------------------|-----------------------------------------------------------------------|
| Redraw window 					| `Ctrl-a C-l`								|
| Enter copy mode 					| `Ctrl-a [` or `Ctrl-a <esc>` (also used for viewing scrollback buffer)|
| Paste 						| `Ctrl-a ]`								|
| Monitor window for activity 				| `Ctrl-a M`								|
| Monitor window for silence 				| `Ctrl-a _`								|
| Enter digraph (for producing non-ASCII characters) 	| `Ctrl-a Ctrl-v`							|
| Lock (password protect) display 			| `Ctrl-a x`								|
| Enter screen command 					| `Ctrl-a :`								|
| Enable logging in the screen session 			| `Ctrl-a H`	
