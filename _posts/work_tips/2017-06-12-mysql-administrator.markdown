---
layout: post
title:  "Mysql常用管理员相关指令"
categories: other, mysql
---

### 查看当前所有连接的详细资料

	$ mysqladmin -u <username> -p<password> -hlocalhost processlist
	or
	$ mysql -u <username> -p<password> -e 'show full processlist'
	or
	mysql> show full processlist

	
### 只查看当前的连接数量，不显示详情

	$ mysqladmin -u <username> -p<password> -hlocalhost status
	