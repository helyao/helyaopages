---
layout: post
title:  "Namespace in JavaScript"
date:   2017-3-8 16:05:39 +0800
categories: helyao update
---

# Namespace in JavaScript

> JavaScript�������ռ���ʹ����������������ִ�к�����ʵ�ֵ�

## ��������������Variable Declaration Hoisting��

> JavaScript������Ԥ����ʱ�����������ڵ�������������ǰ��������Ķ���������ֵ��������ԭ��

    var scope = 'global';
	function f(){
	    console.log(scope);
	    var scope = 'local';
	    console.log(scope);
	}

> Ԥ������ͬ��

	var scope = 'global';
	function f(){
	    var scope;��
	    console.log(scope);
	    scope = 'local';��
	    console.log(scope);
	}

> ���н��

	undefined
	local

## ��������������Function Declaration Hoisting��

> �������������������ⲿ�ű����ⲿ����������Ķ���

	// �ɵ���
	f('superman');
	function f(name){
	    console.log(name);
	}
	// ���ɵ���
	f('superman');
	var f= function(name){
	    console.log(name);
	}

## ����ִ�к��� 

> ����ִ�к���Ϊ���������ִ�еĺ���

	// ��������
	fnName();
	function fnName(){
		...	
	}
	function fnName2(){
		...
	}();
	// �������ʽ
	var fnName = function(){
		...
	}();


> ���·����ᱨ��

	// �������ʽ - (by ������������)
	fnName();
	var fnName = function(){
		...	
	};
	// �������� - �޺������������
	function() {
		...
	}();
	
## ��������������ִ�к��� �� ���������� or �����ռ�

> (function (){...})();

> ��������(! | + | - | = | , | etc)Ҳ���ԣ�ֻ��()����ȫ

	��(function(a) {
		console.log(a);
	})(123);

> ��ô���ĺô�������˽������������JavaScript��ģ��һ��˽�������򣬽�����������װΪһ������������������������ڲ��ı������Է����ⲿ���������ǡ��������ⲿ�ı������ܷ����ڲ�����

