---
layout: post
title: 数据库系统概述(1)之SQL语句
tags: Learn, Database
---

[第一部分](./数据库系统概述.html)对数据库系统做了概述性的总结，本文总结SQL语句。

## SQL分为：

1. 定义语句(DDL): Create database以及create table
2. 数据修改语句(DML)：Insert, Delete, Update和Select。
3. 控制语句(DCL): Grant以及Revoke等

## 示例关系

1. `学生`：学号S#， 姓名Sname, 性别Ssex, 年龄Sage, 所属系别D#, 班级Sclass : Student(S# char(8), Sname char(10), Ssex char(2), Sage integer, D# char(2), Sclass char(6))
2. `院系`: 系别D#，系名Dname, 系主任Dean : Dept (D# char(2), Dname char(10), Dean char(10))
3. `课程`: 课号C#, 课名Cname, 教师编号T#, 学时Chours，学分Credit : Course ( C# char(3), Cname char(12), Chours integer,
Credit float(1), T# char(3) )
4. `教师`: 教师编号T#，教师名Tname, 所属院系D#，工资Salary : Teacher ( T# char(3), Tname char(10), D# char(2),
Salary float(2) )
5. `选课`: 学号S#, 课号C#, 成绩Score : SC ( S# char(8), C# char(3), Score float(1) )



