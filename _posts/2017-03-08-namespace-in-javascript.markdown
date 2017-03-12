---
layout: post
title:  "Namespace in JavaScript"
date:   2017-3-8 16:05:39 +0800
categories: helyao update
---

JavaScript的命名空间是使用匿名函数的立即执行函数来实现的

## 变量声明提升 - Variable Declaration Hoisting

> JavaScript引擎在预编译时，将函数体内的所有声明都提前到函数体的顶部，而赋值操作留在原处

	var scope = 'global';
	function f() {
		console.log(scope);
		var scope = 'local';
		console.log(scope);
	}

> 预编译后等同于

	var scope = 'global';
	function f() {
		var scope;
		console.log(scope);
		scope = 'local';
		console.log(scope);
	}

> 运行结果

	undefined
	local

## 函数声明提升 - Function Declaration Hoisting

	// 可调用
