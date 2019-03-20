---
layout: post
title: PHP判断变量的常用函数
tags: PHP
---

对于经常使用到的变量判断函数进行整理，方便查阅

## 结果对照表

| 	“”	| “apple”	| NULL	| FALSE |	0 |	undefined |
| ---| ---| ---	| --- |	--- |	--- |
| empty()	|	TRUE	|	FALSE	|	TRUE	|	TRUE	|	TRUE	|	TRUE	|
| is_null()	|	FALSE	|	FALSE	|	TRUE	|	FALSE	|	FALSE	|	ERROR	|
| isset()	|	TRUE	|	TRUE	|	FALSE	|	TRUE	|	TRUE	|	FALSE	|
