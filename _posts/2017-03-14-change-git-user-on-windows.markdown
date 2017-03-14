---
layout: post
title:  "Change git username on Windows"
date:   2017-3-14 22:29:21 +0800
categories: helyao update
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

<img src="https://helyao.github.io/assets/20170304/find-credential-manager.jpg" alt="search credential manager" class="inline"/>

> 选择‘凭据管理器’，删除‘普通凭据’的git凭据

<img src="https://helyao.github.io/assets/20170304/credential-manager.jpg" alt="credential manager" class="inline"/>

> 此时重新git push，会看到自动弹出GitHub的Login界面，输入新的账户和密码即可

<img src="https://helyao.github.io/assets/20170304/github-login.jpg" alt="github login" class="inline"/>
