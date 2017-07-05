---
title: mysql 创建新用户并赋予权限
categories:
    - PHP
tags: 
    - mysql
date: 2015-11-26 21:02:00
---

1、以管理员身份登录mysql
mysql -u root -p

2、选择mysql数据库  
use mysql

3、创建用户并设定密码
insert into user (Host,User,Password) values ('localhost', 'test', password('1234'))

4、使操作生效
flush privileges

5、为用户创建数据库 
create database testdb

6、为用户赋予操作数据库testdb的所有权限
grant all privileges on testdb.* to test@localhost identified '1234'

7、使操作生效
flush privileges

8、用新用户登录
mysql -u test -p
