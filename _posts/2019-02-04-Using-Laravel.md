---
layout: post
title: Laravel项目的典型流程
---

做laravel典型项目的流程总结。

1. 创建项目`composer create-project laravel/laravel photoshow`
2. 修改virtual host的目录指向`photoshow/public`
3. 创建Controller `php artisan make:controller AlbumsController`
4. 创建Model `php artisan make:model Album -m`
5. 建立数据库表`php artisan migrate`
6. 修改route设置
