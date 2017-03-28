---
layout: post
title:  "Limit in MySQL"
date:   2017-3-28 15:18:21 +0800
categories: helyao update
---

### limit

> MySQL中通常使用limit进行分页操作

	mysql > SELECT * FROM table LIMIT [offset,] rows;
	mysql > SELECT * FROM cell LIMIT 100, 200;
	// 从cell表中的第101行开始取200行数据，即101~300

### limit会随着程序运行越来越慢

> 当table的行数不多时，limit不会产生过多性能问题，但是当table的表有很多row时，随着程序的运行，会发现select语句越来越慢。

#### limit内部运行机制

> 当MySQL遇到limit时，会将该表从第一行一直加载到分页结束，然后在内存中将分页内的信息return。也就是说例如上面的sql语句，MySQL会将1~300行数据全部加载在内存中，这个时候会发现磁盘IO（disk）会出现峰值。

#### 处理方法

> 当表过大时，还需要有类似的分页操作，为了能够降低磁盘IO压力，一般的做法是对table进行id编号，该编号上建立index，每次limit分页查找改为对该id的select。


	mysql > ALTER TABLE table ADD id INT AYUTO_INCREMENT FIRST, ADD PRIMARY KEY(id)；
	mysql > CREATE INDEX idx_id ON table(id);
	mysql > SELECT * FROM table WHERE id > 100 and id <= 300;
