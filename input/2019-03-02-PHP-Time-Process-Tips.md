---
layout: post
title: PHP时间处理tips
tags: PHP
---

使用PHP进行时间处理时的技巧总结

## 时间戳

```PHP
// 时间转成时间戳
$timestamp = strtotime("2019-03-01 12:20:32")
// or
$dtime = DateTime::createFromFormat("d/m G:i", "13/10 15:00");
$timestamp = $dtime->getTimestamp();

//显示的时候
$time = date('Y-m-d G:i:s', $data['create_time']);


```

## 获取某月的第一天和最后一天

```PHP
// 获取当月的第一天和最后一天
$dt = "2018-02-31";
echo date('Y-m-01', strtotime($dt)) . ' - ' . date('Y-m-t', strtotime($dt));
/** Warning: 如果系统是32位机器的话时间戳的方法在2038年以后可能会失效所以最好使用Datetime方法 **/
$d = new DateTime( '2040-11-23' ); 
echo $d->format( 'Y-m-t' );
```

## 获取相对时间

```PHP
$d = new DateTime('2019-05-01');
// 期间表达式以P开始，家人有时间部分则要以T开始， 如2秒（PT2S）、3周5小时（P3W6H）
$d->sub(new \DateInterval('P1M')); //前一个月
echo $d->format('Y-m-d H:i:s');
```
