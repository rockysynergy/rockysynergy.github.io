---
layout: post
title: Laravel和MySQL使用utf8mb4字符编码
tags: Learn, Database
---

小程序昵称使用的字符有可能会超出BMP的编码范围，插入数据库时会产生错误`General error: 1366 Incorrect string value: '\\xF0\\x9F\\x92\\x93' for column 'nicknam`
, 所以需要进行修改字符编码。

1. 修改列的字符编码

```SQL
ALTER TABLE `users` CHANGE `nickname` `nickname` VARCHAR(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci DEFAULT NULL COMMENT '用户昵称'
```

2. 假如使用Laravel的migration

```PHP
$table->string('nickname')->nullable()->comment('用户昵称')->charset('utf8mb4')->collation('utf8mb4_general_ci');
```

3. `Laravel`的数据库连接配置

```PHP

// config/database.php

 'charset' => 'utf8mb4',
'collation' => 'utf8mb4_unicode_ci',
```

4. 测试字符串`邓叶林€😄A𐍈¢`
