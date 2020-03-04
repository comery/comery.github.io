---
title: log in cluster using a interactive way
date: 2020-03-03 23:19:33
tags: [linux, expect, ssh]
categories: 常用工具
---

也不知道公司的集群为什么就开始使用双向验证的，自从有了这东西，每天到公司除了打开电脑，输入ssh的快捷键，输入密码之后，就得掏出手机看看验证码，有时候要是眼花还得多输几遍，真是欲哭无泪，太tm浪费时间了， 上传下载文件更是如此，长此以往，记数字的能力都给提高了，记忆力都被迫营业了。

不过活人总不能被尿给憋死，上周师弟分享了一个expect语言交互式登录的方法，今天有空来试一下，试完感觉是： 真香！

他的博客原文在这：=> [link](https://nonewood.github.io/2019/10/25/Mac-Authenticator-md/)

#### 一键登录
总结一下，主要是两个方面：

- 实现用电脑端就能收到验证码，另外需要在系统层面捕获到验证码序列，这个前人已经做了，我们只需要”拿来主义“， 这里主要是讲Mac上的方式，Win在文章中也有提到相应的软件，这里不再赘述。首先下载GoldenPassport.zip 然后解压，移动到Applications里面，因为不是官方软件所以需要你去system里面授权使用；开始运行之后在状态栏就有一个小图标，点击添加，将你的个人身份链接加进去，或者从二维码识别添加。然后就搞定了。

>https://www.cnblogs.com/jasondan/p/6508249.html  
>https://github.com/stanzhai/GoldenPassport

- 现在电脑已经可以接受验证码了，上面的软件是通过HTTP的方式给其他程序调用。那我们怎么接收验证码并实现自动登录呢？可以使用expect语言编写脚本获取验证码，实现交互式登录，具体脚本如下：

```shell
#!/usr/bin/expect -f
 # xxxx is your account
  set user xxxx 
  # your IP address
  set host IP 
  # xxxx is your passsword
  set password xxxx 
  # get the verfication code from the local host; 在 expect 脚本里边使用 shell 命令； grep 捕获变量；
  set code [exec sh -c {curl -s "http://localhost:17304" |grep -Eo "\d{6}"}] 
  set timeout -1

  spawn ssh $user@$host
  expect "*assword:*"
  send "$password\r"
  expect "*erification code:*"
  send "$code\r"
  interact
  expect eof
```

将上述脚本保存到文件，修改必要的参数为自己的信息，并 ```chmod +x script``` 添加执行权限，另外你需要检查一下电脑有没有expect语言，没有到话用 ```brew install expect``` 安装一下；然后就可以一键登录了。

#### 上下传文件

和上面的登录类似的，可以实现文件的自由下载；

```shell
#!/usr/bin/expect -f
# the file need scp 
 set src_file [lindex $argv 0] 
 # the target directory 
 set dest_dir [lindex $argv 1] 
 # xxxx is your account
 set user xxxx 
 # your IP address
 set host IP  
 # xxxx is your passsword
 set password xxxx 
 set code [exec sh -c {curl -s "http://localhost:17304" |grep -Eo "\d{6}"}]
 set timeout -1

 spawn scp -r $user@$host:$src_file $dest_dir
 expect "*assword:"
 send "$password\r"
 expect "*erification code:*"
 send "$code\r"
 expect "100%"
 expect eof
 ```
 
 这里面有个小坑就是，上面的两个脚本在写的时候，注释行不能写到行末，不然会引起格式报错。