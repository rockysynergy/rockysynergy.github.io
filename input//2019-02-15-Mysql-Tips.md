---
layout: post
title: Mysql 技巧汇总
tags: MySql, 数据库
---

汇总一下搭建开发环境经常用到的技巧

## 给数据库授权

1. CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
2. GRANT ALL PRIVILEGES ON dbname . * TO 'newuser'@'localhost';
3. FLUSH PRIVILEGES;

### 删除用户 `drop user newuser`

## 部分导出数据库表 

1. mysqldump -t -uroot -p db_name table_name -w"id<1000000" > file_name.dump 
2. SELECT * INTO OUTFILE 'data_path.sql' from table where id<100000