---
layout: post
title:  "Ubuntu下Angular环境的创建"
categories: other
---

> 在Ubuntu 14.04 & 16.04运行良好

### Update npm

	$ npm install -g npm
	$ npm -v

### Install nodejs

	$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
	$ sudo apt-get install -y nodejs
	$ node -v

### Install Angular-cli

	$ npm config set registry="https://registry.npm.taobao.org/"
	$ npm config list
	$ sudo install -g @angular/cli
	$ ng -v
	
