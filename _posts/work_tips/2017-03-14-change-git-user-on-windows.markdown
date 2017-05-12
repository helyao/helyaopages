---
layout: post
title:  "Windows环境下修改git账户"
categories: other
---

### 修改Git默认登录账户

> Windows 7 + Git + GIthub

> 一直使用github都没有问题，直到新注册了一个github账号，问题就出来了

> 每次执行, 

	C:\Users\helyao> git push -u origin master
	remote: Permission to helyao/proname.git denied to OLDNAME.
	
> 我试图在git中设置，git config -global user.name helyao，可是依旧显示我的旧账号，我开始查看所有能查看的文本文件，都找到不我的旧账号的任何相关记录，结果我发现是windows太坑了。

> Windows的设计总想把所有事情都自己解决，减少用户的误操作，真的做到傻瓜级，于是Windows系统真的普及了，但是对于开发者来说，也为灵活的修改提供了不便。


### 修改过程

> 打开‘管理Windows凭据’，直接搜索'Credential Manager'

![search credential manager]("assets/20170314/find-credential-manager.jpg")

> 选择‘凭据管理器’，删除‘普通凭据’的git凭据

![credential manager]("assets/20170314/credential-manager.jpg")

> 此时重新git push，会看到自动弹出GitHub的Login界面，输入新的账户和密码即可

![github login]("assets/20170314/github-login.jpg")
