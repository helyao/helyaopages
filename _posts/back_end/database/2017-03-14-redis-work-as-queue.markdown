---
layout: post
title:  "Redis中队列(Queue)的使用"
categories: redis
---

### Redis做消息队列

> 在工业使用中，中央事务处理经常不使用HTTP作为in/out接口，而是使用UDP/UART作为接口。
此时通常会引入消息队列机制进行异步处理。

> Redis被用作缓存，但是同样提供了几个阻塞式的API，这些API使得Redis可以作为消息队列使用。

### 基本操作

#### 插入

> 低优先级事件，先入先出

	127.0.0.1:6379> lpush queue_name 'string'
	127.0.0.1:6379> lpush sndlist "{'ip':'192.168.1.10', 'port':10021, 'cmd':'test'}" 

> 高优先级事件，先入后出

	127.0.0.1:6379> rpush queue_name 'string'

> 高优先级优先，同样保持先入先出

	127.0.0.1:6379> lpush low_queue_name 'string'
	127.0.0.1:6379> lpush high_queue_name 'string'

优先读取high_queue,当为空时再读取low_queue

#### 读取

> 阻塞接口

	127.0.0.1:6379> blpop queue_name 0

### Python中的使用

> Redis的队列提供了一种任务粘连的作用，采集段仅通过确定的接口进行数据采集，对数据进行基本的校验，和任务分类，push到相应task的queue中，
后级应用通过Redis的阻塞接口实现异步处理。从数据库等级上进行项目模块划分，方便多人协作，以及后期项目扩展和维护。

> UDP -> Redis

	import redis
	import socket

	def log_write(addr, mess):
		type = mess[1]
		if type == 2:
			db_redis.lpush('task_queue', '{}'.format(mess))
		else:
			pass

	redis_pool = redis.ConnectionPool(host='localhost', port=6379, db=0)
	db_redis = redis.Redis(connection_pool=redis_pool)

	udp = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
	udp.bind(("0.0.0.0", config[RUN_LEVEL].UDP_REC_PORT))

	while True:
		try:
			message, address = udp.recvfrom(1024)
			print('IP = "{1[0]}" & PORT = {1[1]} SAY: \n\t{0}'.format(message, address))
			log_write(address, message)
		except Exception as ex:
			print_error(ex)

> Redis -> do

	import redis
	import socket

	redis_pool = redis.ConnectionPool(host='localhost', port=6379, db=0)
	db_redis = redis.Redis(connection_pool=redis_pool)

	udp = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
	udp.bind(("0.0.0.0", config[RUN_LEVEL].UDP_REC_PORT))


	while True:
		result = db_redis.brpop('task_queue', 0)
		do_something(record)

