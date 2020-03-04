---
title: 配置bashrc简易教程
date: 2017-07-19 17:48:20
tags: [Linux, bashrc]
categories: 常用工具
---

#### 了解权限

`.bashrc`文件, 路径`/home/user/.bashrc`注意前面有个`“.”`，表示隐藏文件,
`/etc/profile`，`/etc/bashrc` 是系统全局环境变量设定,只有root管理员才有权限。
`~/.profile`，`~/.bashrc` 用户home目录下的私有环境变量设定，为用户自己的，其他用户没有权限。

<!-- more -->

#### 了解执行过程

当登入系统时候获得一个shell进程时，其读取环境设定文档有三步:
1. 首先读入的是全局环境变量设定档`/etc/profile`, 然后根据其内容读取额外的设定的文档，如`/etc/profile.d和/etc/inputrc`
2. 然后根据不同使用者帐号，去其家目录读取`~/.bash_profile`, 如果这读取不了就读取`~/.bash_login`, 这个也读取不了才会读取`~/.profile`， 这三个文档设定基本上是一样的，读取有优先关系；
3. 然后在根据用户帐号读取`~/.bashrc` 至于`~/.profile` 与`~/.bashrc`的不区别都具有个性化定制功能
`~/.profile`可以设定本用户专有的路径，环境变量，等，它只能登入的时候执行一次

`~/.bashrc`也是某用户专有设定文档，可以设定路径，命令别名，每次shell script的执行都会使用它一次,这个文件里面放你的环境变量，可以将常用的软件路径放到这里，这样就可以不用每次输入这个软件的全路径，另外还可以写很多快捷键（简写），比如alias work=‘cd /path/workdir’ 就直接进入了你的工作目录.

以下是常用的内容，仅供参考:
```bash
if [ -f /etc/bashrc ]; then
. /etc/bashrc;
fi
# User specific aliases and functions
alias l='ls -lhrt --color'
alias le='less -NS'
alias la='ls --color -a'
alias work='cd /yourpath'
alias grep='grep --color=auto'
alias top='top -u yourname'
alias sbr='source ~/.bashrc'
# Promote
PS1='-------------------------------------\n\u@\h\A:\w\n\$ '
# Color Promote
PS1='\[\033[01;32m\]       -------------------------------------------------------\[\033[00m\]\n${debian_chroot:+($debian_chroot)}\	[\033[01;32m\]\u@\h  	\A\[\033[00m\]:\[\033[01;34m\] \w\[\033[00m\]\n\$ '
```
为了每次登陆服务器时候都能正常加载这个`.bashrc`文件，你还需要另外一个文件告诉系统你要使用自己的`.bashrc`文件，所以需要一个文件叫：`.bash_profile`内容为:
```bash
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi

# User specific environment and startup programs
PATH=$PATH:$HOME/bin
export PATH
```
同样需要`source .bash_profile`，此时，配置完成，可以退出再次登陆看看.

