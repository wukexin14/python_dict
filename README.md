# python_dict
电子词典

功能说明：
1. 用户可以登录和注册
   登录凭借用户名密码即可
   注册要求用户必须填写用户名和密码其他内容自定
   用户名要求不能够重复

2. 用户数据要求使用数据库长期保存
   数据报自定

3. 能够满足多个用户同时登陆操作的需求

4. 功能分为客户单和服务端，客户单主要发起请求，服务    端处理请求，用户启动客户端即进入一级界面
     登陆   注册   退出
5. 用户登录后即进入二级界面
     查单词   查看历史记录   退出
         单词本 ： 每行一个单词
	           单词和解释之间一定有空格
		   后面的单词一定比前面的大

     查单词 ： 输入单词，显示单词意思，可以循环查询。输入 ## 表示退出查词

     查看历史记录： 查看当前用户的历史查词记录
        name     word    time
      
     退出 ： 退出到一级界面，相当于注销


1. 确定技术点
   什么并发，什么套接字  什么数据库？  
   文件处理还是数据库查询？ 
   如果是数据库查询如何将单词存入数据库
   

2. 建立数据表
   建立几个表  每个表结构  表关系
	
   3     用户信息     历史记录        存单词  
   
         注册         查询历史记录    查单词
	       登录         查单词
         

3. 项目分析 仿照ftp和聊天室进行项目分析

4. 搭建通信框架

5. 分析有几个功能，如何封装，每个功能具体实现什么内容



电子词典



项目分析

服务器 ： 登录  注册   查词   历史记录

客户端 ： 打印界面   发出请求    接收反馈   打印结果

技术点 :   并发   sys.fork
           套接字  tcp 套接字
	   数据库  mysql
	   查词    文本

工作流程： 创建数据库，存储数据 ---》 搭建通信框架，            建立并发关系---》实现具体功能封装

1. 创建数据库存储数据
   dict

   user ： id  name   passwd
   hist :  id  name   word   time
   words : id  word   interpret

   create database dict default charset=utf8;

   create table user (id int primary key auto_increment,name varchar(32) not null,passwd varchar(16) default '000000');

   create table hist(id int auto_increment primary key,name varchar(32) not null,word varchar(32) not null,time varchar(64));
   
   create table words (id int auto_increment primary key,word varchar(32) not null,interpret text not null);

2. 搭建基本框架
   
   服务器  创建套接字 ---》 创建父子进程 --》 子进程         等待处理客户单端请求--》父进程继续接收下         一个客户端连接

   客户端  创建套接字 --》发起连接请求 --》一级界面         --》请求（登录，注册，退出）--》登录成功         进入二级界面--》请求（查词，历史记录）
 
