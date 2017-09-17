---
layout: post
title: linux 命令
description: 工作中常用到的命令
category: blog
---



查看 运行 进程
---
	ps-elf|grep tomcat  //查看进程

	kill -9 进程号    //杀掉进程

	tomcat ：

	cd 到 tomcat bin目录 ./startup.sh  //启动

	cd 到 logs tail-f catalina.out      //监控日志 



打包 解压文件
---

	zip：

	打包 ：zip something.zip something （目录请加 -r 参数）

	例如：zip -r something.zip /home/test   //将指定的目录打包

	解包：unzip something

	指定路径：-d 参数

	例如：unzip  something.zip -d /home/test   //解压



	tar：

	打包：tar -zcvf something.tar something

	解包：tar -zxvf something.tar   -c test

	指定路径：-C 参数


cp命令
---

	cp [options] <source file or directory> <target file or directory>

	或
	cp [options] source1 source2 source3 …. directory

上面第一条命令为单个文件或目录拷贝，下一个为多个文件拷贝到最后的目录。

options选项包括：

	- a 保留链接和文件属性，递归拷贝目录，相当于下面的d、p、r三个选项组合。

	- d 拷贝时保留链接。

	- f 删除已经存在目标文件而不提示。

	- i 覆盖目标文件前将给出确认提示，属交互式拷贝。

	- p 复制源文件内容后，还将把其修改时间和访问权限也复制到新文件中。

	- r 若源文件是一目录文件，此时cp将递归复制该目录下所有的子目录和文件。当然，目标文件必须为一个目录名。

	- l 不作拷贝，只是链接文件。

	- s 复制成符号连结文件 (symbolic link)，亦即『快捷方式』档案；

	- u 若 destination 比 source 旧才更新 destination。

cp命令使用范例
---

	1、将文档 file1复制成file2，复制后名称被改file2

	cp -i file1 file2

	或，

	cp file1 file2

	2、将文档 file1复制到dir1目录下，复制后名称仍未file1

	cp -i file1 dir1

	或，

	cp file1 dir1

	3、将目录dir1复制到dir2目录下，复制结果目录被改名为dir2

	cp -r dir1 dir2

	4、将目录dir1下所有文件包括文件夹，都复制到dir2目录下

	cp -r dir1/*.* dir2   
	
	5、将/opt/a/下的a目录复制到 /opt/b/目录下
	
	cp -r /opt/a/ /opt/b/ 

	常见错误：

	1、提示cp: omitting directory错误

	复制目录时，使用-r选项即可递归拷贝，如下：

	cp -r dir1 dir2









[Mukosame]:    http://sun035.github.io  "Mukosame"
