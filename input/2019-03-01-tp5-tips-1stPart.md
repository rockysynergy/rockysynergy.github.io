---
layout: post
title: TP5 使用总结（1）
tags: Learn, TP5
---

TP5使用过程中踩过的坑与经验总结

## 时间戳
1. 数据库使用`int`类型
2. PHP代码

```PHP
// 时间转成时间戳
$timestamp = strtotime("2019-03-01 12:20:32")
// or
$dtime = DateTime::createFromFormat("d/m G:i", "13/10 15:00");
$timestamp = $dtime->getTimestamp();

//显示的时候
$time = date('Y-m-d G:i:s', $data['create_time']);
```

## DB和模型
1. 模型的update静态方法貌似不支持`allowFields`方法
2. 通过模型中的获取器得到的属性在前端不显示的问题

```PHP
// 模型中增加
protected static $append_attr = ['applytime_title', 'status_title', 'method_title'];
// 查询数据的时候
$list ['data'] = self::where($where)->field($param['field'])->order()->select()->append(self::$append_attr)->toArray();
```
