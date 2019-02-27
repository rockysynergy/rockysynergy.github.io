---
layout: post
tags: Web_Dev, Softaware Design
title: 系统ACL设计
---

权限管理（ACL）是网站系统常见的功能需求，简单的说就是根据实际情况赋予用户相应的权限。比如说用户A可以修改员工信息而不能修改商品信息。它主要由认证和授权两部分组成。在实现授权部分的时候需要考虑以下几个方面：

## 权限(privilege)
1. 通常包含受保护的资源(resource)和操作(action)集。
2. `方法调用`：现代的系统开发框架一般都使用控制器作为调用内部功能的入口。对于资源实体与控制器有类似于1对1对应关系的设计，使用方法此种控制方法会比较合适。比如说领域模型有Staff实体，那么控制StaffContror的方法就可以了。
3. `权限标识`：使用统一的规范对权限进行编码，比如说staff_add。
4. `存储`: 权限可以存储在数据库表中也可以使用配置文件的方式进行存储
5. `结构`: 最好不要设计成树形结构，Staff下面有子节点Staff_Add，Staff_Edit, Staff_Delete
6. `参数`: 根据具体的需要，还可以为权限添加参数，比如说针对2年以内入职的员工设置标识`Staff_MOdify_jointime_lessThan_2y`

## 角色（role）
1. 同一个角色可以有0到多个权限
2. 角色和具体的用户进行关联完成赋值
3. 角色一般都会设计成树形结构，方便进行管理。hr_staff有查看员工基本信息的权限，hr_manager继承hr_staff的所有权限。
4. 除非被赋予了权限，否则默认没有任何权限。设置authenticated_user, nobody这2种基类权限可以方便实现。
5. 设置deny_privileges以及单一否定原则，即使role_A继承的角色有privilege_A，假如它包含于deny_privileges，role_A以及继承自role_A的角色都不会被赋予privilege_A。该原则可以降低开发成本和提高易用性。

## 用户(User)
1. 一个用户可以有1到多个role，缺省角色为authenticated_user或nobody。
2. 单一否决原则：所有角色的集合中只要有一个角色deny了privelege_A那么即使有另外的多个角色都有该权限，该用户也不会拥有privelege_A

## 集成
1. 使用AOP（面向切面），对于支持AOP的框架，使用 AOP功能可以大大降低代码的耦合度。
2. 使用hook
3. 每一个Controller方法都核验用户的priviledge