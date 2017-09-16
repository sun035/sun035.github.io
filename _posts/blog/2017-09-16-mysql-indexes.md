---
layout: post
title: mysql indexes
description: 数据库索引的内容
category: blog
---



&emsp;索引用于快速找出在某列中有一特定值的行。

如果不使用索引，MySQL必须从第一条记录开始读完整个表，直到找出相关的行。

表越大，查询数据所花费的时间越多。

如果表中查询的列有一个索引，MySQL能快速到达一个位置去搜寻数据文件，而不必查看所有的数据




索引是什么
---


&emsp;索引是对数据库一列或多列的值进行排序的一种结构 使用索引可以提高数据库查询效率



索引的含义和特点
---


&emsp;索引是以索引文件的形式存储在磁盘中，它们包含数据表中记录的引用，使用索引快速定位数据在数据表中的位置

 例如：数据库中有2万条记录，现在要执行这样一个查询：
  
 select * from table where num=10000。
 
 如果没有索引，必须遍历整个表，直到num等于10000的这一行被找到为止；
 
 如果在num列上创建索引，MySQL不需要任何扫描，直接在索引里面找10000，
 
 就可以得知这一行的位置。可以，创建索引可以提高数据库的查询速度。
 
 
 缺点：创建索引和维护索引也需要时间，并且随着数据量的增加而增加
 
 索引会需要磁盘空间，对表数据的增删改都要对索引表进行维护
 
 
 
 索引的创建
 ---
 
&emsp;mysql支持多种方法在单个列或多列上创建索引<br/>

<1>在定义表的定义语句CREATE TABLE 创建索引
---


---

1.	创建表的时候创建索引

&emsp;当我们指定主键的时候默认也为主键创建了单列索引,创建索引的格式：


``` sql ```


``` sql

CREATE TABLE table_name [col_name data_type]

[ UNIQUE | FULLTEXT | SAPTIAL ] [ INDEX | KEY ] [ index_name ] ( col_name[length] ) [ ASC | DESC ]
 
 
``` 
 
参数说明：

        UNIQUE、FULLTEXT和SAPTIAL为可选参数，分别表示唯一索引、全文索引和空间索引；
		
        INDEX与KEY为同义词，两者作用相同，用来指定创建索引；
		
        col_name为需要创建索引的字段列，该列必须从数据表中定义的多个列中选择；
		
        index_name指定索引的名称，为可选参数，如果不指定，MySQL默认col_name为索引值；
		
        length为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度；
		
        ASC或DESC指定升序或者降序的索引值存储。
 

&emsp;<1.1> 创建普通索引
---
``` sql ```


``` sql
create table code{
	id int not null,
	name char(10),
	index(name)
}

``` 

&emsp;<1.2> 创建唯一索引
---

``` sql ```


``` sql
create table code{
	id int not null,
	name char(10),
	unique index name_index(name)
}


```

创建唯一索引的列不能重复（null除外）



&emsp;<1.3> 创建组合索引
---

``` sql ```


``` sql

create table code{
	id int not null,
	name char(10),
	age  int,
	index name_index(id，name(10),age)
}


``` 
对于组合索引的匹配规则遵从“最左前缀”，例如上面索引，索引匹配：（id,name),id,(id,name,age)
假如为（id,age,name）则不匹配，而且局部（name,age）也是不匹配的
 
 ---
 
 

 
 

<2>使用ALTER TABLE 在以存在的表中创建索引
---

---



在已存在的表上创建索引

        在已存在的表中创建索引，可以使用ALTER TABLE语句或者CREATE INDEX语句。
		
        使用ALTER TABLE语句创建索引
		
        ALTER TABLE table_name ADD [ UNIQUE | FULLTEXT | SPATIAL ] [ INDEX | KEY ]
		
        [ index_name ] (col_name[length], ...) [ ASC | DESC ]
 
在book表的name字段上建立索引：

        ALTER TABLE book ADD INDEX BkNameIdx( bookname(30) );   
      
        ALTER TABLE book ADD UNIQUE INDEX UniqidIdx(bookId);
 
在book表的comment字段上建立单列索引：

        ALTER TABLE book ADD INDEX BkcmtIdx(comment(50));

		语句执行之后会在book表的comment字段上建立名称为BkcmgIdx的索引，长度为50，在查询时只需要检索前50个字符。
 
在book表的authors 和info字段上建立组合索引：

        ALTER TABLE book ADD INDEX BkAuAndInfoIdx(author(20), info(50))




---




<3>使用CTEATE INDEX在已存在的表中创建索引
---

  create index语句可以在已经存在的表上添加索引，MySQL中create index被映射到一个alter table语句上：
       
	   CREATE [UNIQUE | FULLTEXT | SPATIAL ] INDEX index_name
	   
        ON table_name (col_name[length], ...) [ASC | DESC]
		
 
        其实这两个在已有的表上创建的方式没啥差别。
		
        create index BkNameIdx ON book(bookname);
		
        create UNIQUE index UniqidIdx ON book (bookid);
		
        create fulltext index on t6(info);
		
		
---



索引的删除
---


   MySQL中使用ALTER TABLE或者DROP INDEX语句删除索引，DROP INDEX被映射到ALTER TABLE语句中。
 
1. 使用ALTER TABLE删除索引

        ALTER TABLE table_name DROP INDEX index_name;
 
        alter table book drop index UniqidIdx;
		
        添加了auto_increment约束字段的唯一索引不能被删除。
 
2. 使用DROP INDEX语句删除索引

        DROP INDEX index_name ON table_name;
 
        删除表中的列时，如果要删除的列为索引的组成部分，则该列也会从索引中删除。如果组成索引的所有列都被删除，则整个索引将被删除。
		

		
显示索引信息
---

你可以使用 SHOW INDEX 命令来列出表中的相关的索引信息。可以通过添加 \G 来格式化输出信息。

尝试以下实例:

	mysql> SHOW INDEX FROM table_name\G		
	
	
	EXPLAIN 命令可以查询索引的相关信息

		


来源网络.......


[Mukosame]:    http://sun035.github.io  "Mukosame"
