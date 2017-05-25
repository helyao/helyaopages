---
layout: post
title:  "Python的Requests速度非常慢"
categories: other
---

### request post测试访问速度超过10s

	$ sudo apt-get remove python-requests
	$ sudo apt-get install libffi-dev
	$ pip install --upgrade requests
	$ pip install --upgrade pyOpenSSL

