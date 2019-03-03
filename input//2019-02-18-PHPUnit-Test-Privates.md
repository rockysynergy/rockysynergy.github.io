---
layout: post
title: PHPUnit测试私有方法
tags: PHP
---

# 测试private方法的代码
```PHP
    
    protected static function getMethod($name) {
        $class = new \ReflectionClass('MyClass');
        $method = $class->getMethod($name);
        $method->setAccessible(true);
        return $method;
    }

    public function testFoo() {
        $foo = self::getMethod('foo');
        $obj = new MyClass();
        $foo->invokeArgs($obj, []);
    }

```