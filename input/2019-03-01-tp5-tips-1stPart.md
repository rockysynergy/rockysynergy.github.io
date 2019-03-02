---
layout: post
title: TP5使用总结（1）
tags: Learn, TP5
---

TP5使用过程中踩过的坑与经验总结

## 时间戳
1. 数据库使用`int`类型
2. PHP代码可参考[PHP时间处理技巧](./PHP时间处理tips.html)

## DB和模型

1. 更新模型的update静态方法不支持`allowField`方法。

```PHP
// 方法1：数组里和数据库表里都有的字段都会被更新。即使开启了autoWriteTimestamp，但是数据库表中没有update_time字段，也不会报错
        $data = [
            'member_id' => 11083,
            'earnings' => 580,
            'performance' => 40000,
            'status' => 1,
            'gash' => 'aaaa'
        ];
        $income = model('\app\common\model\AgentIncome');
        $var = \app\common\model\AgentIncome::update($data, ['member_id' => $data['member_id']]);
        
// 方法2：只是更新status和earnings字段。开启了autoWriteTimestamp，但是数据库表中没有update_time字段会报错，除非关闭updateTime

        $data = [
            'member_id' => 11083,
            'earnings' => 250,
            'performance' => 40000,
            'status' => 1,
            'gash' => 'aaaa' 
        ];
        $income = model('\app\common\model\AgentIncome');
        $var = $income->allowField(['earnings', 'status'])->save($data, ['member_id' => $data['member_id']]);
```

2. 通过模型中的获取器得到的属性在前端不显示的问题

```PHP
// 模型中增加
protected static $append_attr = ['applytime_title', 'status_title', 'method_title'];
// 查询数据的时候
$list ['data'] = self::where($where)->field($param['field'])->order()->select()->append(self::$append_attr)->toArray();
```

3. 多条件查询

```PHP
// 版本5.1.21以后可以使用数组方式
use think\db\Where;
...
 $cond = new Where;   
 $cond['pay_way'] = ['<>', 'granary'];
 $cond['member_id'] = ['>', $memberId];
 $cond['pay_status'] = ['=', 0];
 
$var = \think\Db::table('hl_team_order')->where($cond)
  ->fetchSql()
  ->whereTime('create_time', 'between', ['2019-02-21', date('Y-m-t', strtotime($period))])            
  ->select();
  
// 方法2：使用多个where
$var = \app\common\model\TeamOrder::where('pay_way', '<>', 'granary')
                ->where('pay_status', 0)
                ->where('member_id', $memberId)
                ->whereTime('create_time', 'between', [date('Y-m-01', strtotime($period)), date('Y-m-t', strtotime($period))])
                ->fetchSql()
                ->select();
 dump($var);
 ```
