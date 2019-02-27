---
layout: post
tags: Web_Dev, PHP
title: PHP调试技巧
---

## 使用`Xdebug`

可以参考[Windows PHP开发环境快速搭建](./Windows_PHP开发环境快速搭建.html)

## 把调试信息输出到文件
1. 使用`error_log`
```PHP
error_log("You messed up!", 3, "/var/tmp/my-errors.log");
```


2. 使用buffer

```PHP
<?php
namespace My\Package\Util;

class Debug {
    /**
     * @param $var the variable want to peek
     * @param $label 
     * @param $file
     * @return void
     */
    public static function varDump($var, $label = '', $file = 'debug.txt') {
        ob_start(function ($buffer) use ($label, $file) {
            $content = date('Y-m-d G:i:s') . " => " .$label . ": \n" . $buffer;
            file_put_contents($file, $content, FILE_APPEND);
        });

        var_dump($var);
        ob_end_clean();
    }
}
```