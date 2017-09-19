---
layout: post
title: spring 事务
description: 关于事务的内容
category: blog
---



1、理解Spring事务管理的基本概念

2、掌握Spring事务管理的应用场景

3、掌握声明式事务管理和编程式事务管理的实现方式





事务是什么？
---

事务指的是逻辑上的一组操作，这组操作要么全部成功 要么全部失败




事务的四大特性
---

原子性 一致性 隔离性 持久性



	原子性 指事务是不可分割的工作单位 事务中的操作要么发生要么都不发生

	一致性 事务执行前后数据的完整性是一致的

	隔离性 多个用户并发访问时 一个用户的事务不能被其他用户的事务干扰，
		   多个并发事务之间数据要隔离

	持久性 事务一旦提交 它对数据库中数据的改变将是永久性的，不会受到影响



spring 常用接口
---


	1 platfromTransactionManager
	  事务管理器

	2 TransactionDefinition
	  事务定义信息（隔离 传播 超时 只读）

	3 TransactionStatus
	  事务具体运行状态



	  
	  
1.spring 为不同的持久化框架提供不同的platfromTransactionManager


![事务管理器](https://raw.githubusercontent.com/sun035/images/master/%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86%E5%99%A8.png)



2.隔离性问题：脏读 幻读 不可重复读

	脏读：一个事务读取了另一个事务改写但还未提交的数据
	如果这些数据被回滚 则读到的数据是无效的

	不可：在同一个事务中 多次读取同一个数据但会的结果有所不同

	幻读：一个事务读取了几行数据后 另一个事务插入一些数据
	幻读就产生了 再后来的查询中 第一个事务就会发现有些原来没有的记录


![事务隔离级别](https://raw.githubusercontent.com/sun035/images/master/%E4%BA%8B%E5%8A%A1%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB.png)


spring  default 默认数据库的隔离级别

	mysql repeatable_read级别

	oracle read_committed




![事务传播行为](https://raw.githubusercontent.com/sun035/images/master/%E4%BA%8B%E5%8A%A1%E4%BC%A0%E6%92%AD%E8%A1%8C%E4%B8%BA.png)




声明式的事务管理方式一
---

1.引入spring aop的jar


事务xml配置信息

![事务xml配置信息](https://raw.githubusercontent.com/sun035/images/master/%E4%BA%8B%E5%8A%A1xml%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF.png)

业务类注入

![业务类注入](https://raw.githubusercontent.com/sun035/images/master/%E4%B8%9A%E5%8A%A1%E7%B1%BB%E6%B3%A8%E5%85%A5.png)





声明式事务管理方式二
---

aspectj方式的xml配置（常用）

![aspectj方式的xml配置](https://raw.githubusercontent.com/sun035/images/master/%E5%A3%B0%E6%98%8E%E5%BC%8Faspectj%E7%9A%84xml%E9%85%8D%E7%BD%AE.png)

配置事务增强属性信息图

![配置事务增强属性信息图](https://raw.githubusercontent.com/sun035/images/master/%E9%85%8D%E7%BD%AE%E4%BA%8B%E5%8A%A1%E5%A2%9E%E5%BC%BA%E5%B1%9E%E6%80%A7%E4%BF%A1%E6%81%AF.png)









[Mukosame]:    http://sun035.github.io  "Mukosame"
