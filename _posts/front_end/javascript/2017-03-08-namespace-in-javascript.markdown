---
layout: post
title:  "JavaScript中的命名空间"
categories: javascript
---

JavaScript的命名空间是使用匿名函数的立即执行函数来实现的

### 变量声明提升 - Variable Declaration Hoisting

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

### 函数声明提升 - Function Declaration Hoisting

	// 可调用
	f('helyao');
	function f(name) {
		console.log(name);
	}
	// 不可调用
	f('helyao');
	var f = function(name) {
		console.log(name);
	}

### 立即执行函数

	// 函数声明
	f1();
	function f1() {
		...
	}
	function f2() {
		...
	}();
	// 函数表达式
	var f3 = function() {
		...
	}();

> 以下函数会报错

	// 函数表达式 - 由于变量声明提升导致
	f4();
	var f4 = function() {
		...
	};
	// 匿名函数 - 无函数名或变量名
	function() {
		...
	}();

### 匿名函数的立即执行函数 - 即匿名包裹器or命名空间

> (function(){...})();

> 其他符号(!  +  -  ,  等)也可是，只是()更安全

	!(function(name) {
		console.log(name);
	})('helyao');

> 这么做的好处：在无私有作用于概念的JavaScript中模拟一个私有作用域，将匿名函数封装为一个“容器”，这个“容器”内部的变量可以访问外部变量，但是“容量”外部的变量不能访问内部变量。
