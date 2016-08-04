阅读sphinx源码笔记

目录结构：
整个coreseek系统由多个可执行程序和一套api组成。
1. 可执行程序主要由下面这些组成：
	a) 索引建立和维护程序(索引程序indexer)

	b) 查询服务程序(后台服务程序searchd)

	c) 辅助工具程序(search, spelldump等)

2. Api主要由下面两个组成
	a)         应用程序api（包括ruby，C/C++, Python, php的程序api）
	b)        Mysql的sphinxSE引擎接口

功能简图：
	http://hi.csdn.net/attachment/201105/5/0_1304592845133O.gif


学习之路-调试代码 
	1：从索引程序开始调试
		src/indexer.cpp，代码一共2000行左右
	2：测试准备工作
		升级gcc版本4.8.2 (费了很长时间，因为sphinx编译需要-std=c++11 参数，老版的gcc不支持)
		./configure --prefix=/usr/local/sphinx/  --without-mysql
		make
		cd src
		make indexer (编译indexer.cpp文件，会生成indexer可执行程序)
	3：修改代码，在main函数第一行，执行输出"hello world" 编译并执行

		
