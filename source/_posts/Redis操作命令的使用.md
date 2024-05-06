---
layout: post
title: Redis操作命令的使用
toc: true
reward: true
date: 2023-08-11 14:22:24
tags: [Redis]
categories: [中间件]
---
Redis操作命令的使用笔记，涵盖基本数据类型的常见操作。以下是这份笔记：

---

## Redis 操作命令使用指南

### 1. 字符串（String）

- 设置指定key的值: `SET key value`
- 获取指定key的值: `GET key`
- 设置指定key的值，并将 key 的过期时间设为 seconds 秒：`SETEX key seconds value`
- 只有在 key 不存在时设置 key 的值: `SETNX key value`
- 追加字符串：`APPEND key value`
- 获取字符串长度：`STRLEN key`
- 自增：`INCR key`
- 自增指定值：`INCRBY key increment`
- 自增浮点数：`INCRBYFLOAT key increment`
- 设置并获取旧值：`GETSET key value`
- 设置多个键值对：`MSET key1 value1 [key2 value2 ...]`
- 获取多个键值：`MGET key1 [key2 ...]`
- 设置键的过期时间：`EXPIRE key seconds`
- 设置键的过期时间（毫秒）：`PEXPIRE key milliseconds`
- 获取键的剩余生存时间：`TTL key`
- 获取键的剩余生存时间（毫秒）：`PTTL key`
- 移除键的过期时间：`PERSIST key`	
<!-- more -->

### 2. 哈希（Hash）

- 设置哈希字段值：`HSET key field value`
- 获取哈希字段值：`HGET key field`
- 获取整个哈希表：`HGETALL key`
- 获取哈希表所有字段：`HKEYS key`
- 获取哈希表所有值：`HVALS key`
- 删除存储在哈希表中的指定字段: `HDEL key field	`

### 3. 列表（List）

- 在列表头部插入元素：`LPUSH key value`
- 在列表尾部插入元素：`RPUSH key value`
- 弹出并返回列表头部元素：`LPOP key`
- 弹出并返回列表尾部元素：`RPOP key`
- 获取列表范围内的元素：`LRANGE key start stop`

### 4. 集合（Set）

- 向集合添加一个或多个成员：     `SADD key member1 [member2]`
- 删除集合中一个或多个成员:     `SREM key member1 [member2]`
- 获取集合所有成员：           `SMEMBERS key`
- 判断元素是否在集合中：        `SISMEMBER key member`
- 获取集合的成员数:            `SCARD key`
- 返回给定所有集合的交集:       `SINTER key1 [key2]`
- 返回所有给定集合的并集:       `SUNION key1 [key2] 	` 

### 5. 有序集合（Sorted Set）

- 获取有序集合成员的分数：`ZSCORE key member`
- 获取有序集合的排名：`ZRANK key member`
- 向有序集合添加一个或多个成员: `ZADD key score1 member1 [score2 member2]`
- 通过索引区间返回有序集合中指定区间内的成员: `ZRANGE key start stop [WITHSCORES]`
- 获取有序集合范围内的元素：`ZRANGEBYSCORE key min max`		
- 有序集合中对指定成员的分数加上增量increment: `ZINCRBY key increment member`			 
- 移除有序集合中的一个或多个成员:   `ZREM key member [member ...] `			

### Redis的通用命令是不分数据类型的，都可以使用的命令
- 查找所有符合给定模式( pattern)的 key:  `KEYS pattern`		
- 检查给定 key 是否存在: `EXISTS key`	
- 返回 key 所储存的值的类型: `TYPE key`
- 该命令用于在 key 存在是删除 key: `DEL key`

### 其他操作

- 发布消息：`PUBLISH channel message`
- 订阅消息：`SUBSCRIBE channel`
- 事务操作：`MULTI`、`EXEC`、`WATCH`
- 执行 Lua 脚本：`EVAL`、`EVALSHA`


---