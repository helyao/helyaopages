---
layout: post
title:  "Namespace in JavaScript"
date:   2017-3-8 16:05:39 +0800
categories: helyao update
---

# Namespace in JavaScript

> JavaScript的命名空间是使用匿名函数的立即执行函数来实现的

## 变量声明提升（Variable Declaration Hoisting）

> JavaScript引擎在预编译时，将函数体内的所有声明都提前到函数体的顶部，而赋值操作留在原处

    var scope = 'global';
	function f(){
	    console.log(scope);
	    var scope = 'local';
	    console.log(scope);
	}

> 预编译后等同于

	var scope = 'global';
	function f(){
	    var scope;、
	    console.log(scope);
	    scope = 'local';、
	    console.log(scope);
	}

> 运行结果

	undefined
	local

## 函数声明提升（Function Declaration Hoisting）

> 函数声明将被提升到外部脚本或外部函数作用域的顶部

	// 可调用
	f('superman');
	function f(name){
	    console.log(name);
	}
	// 不可调用
	f('superman');
	var f= function(name){
	    console.log(name);
	}

## 立即执行函数 

> 立即执行函数为定义后立即执行的函数

	// 函数声明
	fnName();
	function fnName(){
		...	
	}
	function fnName2(){
		...
	}();
	// 函数表达式
	var fnName = function(){
		...
	}();


> 以下方法会报错

	// 函数表达式 - (by 变量声明提升)
	fnName();
	var fnName = function(){
		...	
	};
	// 匿名函数 - 无函数名或变量名
	function() {
		...
	}();
	
## 匿名函数的立即执行函数 即 匿名包裹器 or 命名空间

> (function (){...})();

> 其他符号(! | + | - | = | , | etc)也可以，只是()更安全

	！(function(a) {
		console.log(a);
	})(123);

> 这么做的好处：在无私有作用域概念的JavaScript中模拟一个私有作用域，将匿名函数封装为一个“容器”，这个“容器”内部的变量可以访问外部变量，但是“容器”外部的变量不能访问内部变量

