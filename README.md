# 阅读sphinx源码笔记（一）

##目录结构：
sphinx系统由多个可执行程序和一套api组成。

* 1. 可执行程序主要由下面这些组成：

		a) 索引建立和维护程序(索引程序indexer)
		b) 查询服务程序(后台服务程序searchd)
		c) 辅助工具程序(search, spelldump等)
	
* 2. Api主要由下面两个组成
	
		a) 应用程序api（包括ruby，C/C++, Python, php的程序api）
		b) Mysql的sphinxSE引擎接口

流程图：
		
![流程](http://hi.csdn.net/attachment/201105/5/0_1304592845133O.gif)


## 逐行调试、分析 - 准备调试环境
	
		环境：mac osx，需要提前安装mysql（以下测试以mysql做为数据源）
		
* 1：选择调试入口：从索引程序开始调试
		
		src/indexer.cpp，代码一共2000行左右
* 2：测试准备工作
		//本地安装mysql（brew 快速安装mysql)，导入部分测试数据
		根目录下的example.sql

		升级gcc版本4.8.2 (费了很长时间，因为sphinx编译需要-std=c++11 参数，老版的gcc不支持)
		./configure --prefix=/usr/local/sphinx/ 
		
* 3：修改代码，在main函数第一行，执行输出"hello world" 编译并执行

		vim src/indexer.cpp
			printf("hello word\n");
			return 0;
		cd src
		make indexer (编译indexer.cpp文件，会生成indexer可执行程序)
		./indexer
		# 输出 hello world

## indexer功能具体分析
* main入口
	
		1: 获取命令行参数，判断参数
		2：读取config文件
		3：根据用户传入的操作类型: merge、indexer等进行相应操作
		4：继续根据indexer的相关操作，执行 DoIndex 创建索引
	** 每一步都可以单独调试 ：./indexer -c ../sphinx-min.conf.dist test1 (需要手动调整配置文件中的mysql数据库帐号 及 索引文件的存放目录) **

* DoIndex函数是一个重要入口，负责创建索引(本次分析的重点)
	
		1：indexer的参数验证
		2：pIndex->build()
		3:


源码阅读小技巧：
	1：首先要了解全文检索的原理  推荐 《这就是搜索引擎》 https://www.amazon.cn/%E8%BF%99%E5%B0%B1%E6%98%AF%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E-%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E8%AF%A6%E8%A7%A3-%E5%BC%A0%E4%BF%8A%E6%9E%97/dp/B006J9MSD8/

	2：阅读代码时如果遇到不明确的数据结构，尽量快速理解，不要过分的深入分析，只要理解其功能
	


