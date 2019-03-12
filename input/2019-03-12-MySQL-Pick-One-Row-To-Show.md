---
layout: post
title: 同一用户有多条记录显示所有用户的一条特定记录
tags: Software Design
---

问题： 一组用户都有一组存储在同一数据表里的记录，但是当前时间段内只能显示符合规则的一条。

== 添加字段法
1. 在数据库表中添加is_next标记下次要显示的记录
2. 添加last_process_time标记最近一次处理的时间
3. 符合is_next == true && last_process_time not between firstDayOfMonth and lastDayOfMonth的记录会被显示

== 相关子查询
通常情况下这类记录都有一个排序字段

```SQL
select 
  *
from 
  tbname ta
where
  order_field = (
  select
     min(order_field)
   from 
     tbname tb
   where
     ta.id = tb.id
    )
 group by user_id;
 ```
 
 == 创建视图
 创建视图，使用触发器生成需要的信息
