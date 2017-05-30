---
layout: post
title:  "Codeignore中调用cross-site脚本提示无Access-Control-Allow-Origin"
categories: php
---

Allowing cross-site scripting may cause security issues, try to adjust your codeigniter options;

1.) Go to application/config/config.php file, 
2.) find $config['base_url'] = ""; and 
3.) place your project folder's path as value. $config['base_url']="http://localhost/yourProjectFolder/";

