---
layout: post
title: PHP使用PDO
tags: PHP, Database
---
使用PDO进行数据库操作的方法。

## 查询数据

```php
// 方法1： 执行SQL语句
$stmt = $db_conn->query('SELECT name, chef FROM recipes');

// 方法2: 使用预处理语句（可以使用`?`代替名称占位符）
$sql = 'SELECT name, description FROM recipes WHERE category = :category_name AND chef = :chef';
$stmt = $db_conn->prepare($sql);

// 可以使用下面2种方式或execute时给定查询条件
$stmt->bindValue(':chef', 'Lorna');
$stmt->bindParam(':category_name', $category); // 需要在execute之前设置$category变量并赋值

$stmt->execute(array(
 "category_name" => 'starter')
);


// 获取执行结果，PDO::FETCH_ASSOC， PDO::FETCH_ASSOC，PDO::FETCH_BOTH， PDO::FETCH_CLASS
while($row = $stmt->fetch()) {
 echo $row['name'] . ' by ' . $row['chef'] . "\n";
}
```

## 插入数据

```php
$db_conn = new PDO('mysql:host=localhost;dbname=recipes', 'php-user', 'secret');
$sql = 'INSERT INTO recipes (name, description, chef, created)
	VALUES (:name, :description, :chef, NOW())';
$stmt = $db_conn->prepare($sql);
$stmt->execute(array(
	':name' => 'Weekday Risotto',
	':description' => 'Creamy rice-based dish, boosted by in-season
	ingredients. Otherwise known as \'raid-the-fridge risotto\'',
	':chef' => 'Lorna')
);
echo "New recipe id: " . $db_conn->lastInsertId();
```

## 获取修改记录数目

```php
$db_conn = new PDO('mysql:host=localhost;dbname=recipes', 'php-user', 'secret');
$sql = 'UPDATE recipes SET category_id = :id
 WHERE category_id is NULL';
$stmt = $db_conn->prepare($sql);
$stmt->execute(array(':id' => 2));
echo $stmt->rowCount() . ' rows updated';
```

## 删除记录

```php
$db_conn = new PDO('mysql:host=localhost;dbname=recipes', 'php-user', 'secret');
$stmt = $db_conn->prepare('DELETE FROM categories WHERE name = :name');
$stmt->execute(array(':name' => 'Starter'));
echo $stmt->rowCount() . ' row(s) deleted';
```

## 使用PDO的过程中需要处理有可能产生的异常和错误
除了下面的错误处理，在实际项目中有可能需要处理`fetch`产生的错误（返回`false`，更多的信息可以通过`PDOStatement::errorInf()`获取）

```php
try {
	$db_conn = new PDO('mysql:host=localhost;dbname=recipes', 'php-user', 'secret');
} catch (PDOException $e) {
	echo "Could not connect to database";
	exit;
}
$stmt = $db_conn->prepare($sql);
if($stmt) {
	// perform query
	$result = $stmt->execute(array(
		"recipe_id" => 1)
	);

	if($result) {
		$recipe = $stmt->fetch();
		print_r($recipe);
	} else {
		$error = $stmt->errorInfo();
		echo "Query failed with message: " . $error[2];
	}
}
```

## 事务

```php
try {
	$db_conn = new PDO('mysql:host=localhost;dbname=recipes', 'php-user', 'secret');
} catch (PDOException $e) {
	echo "Could not connect to database";
	exit;
}
try {
	$db_conn->beginTransaction();
	$db_conn->exec('UPDATE categories SET id=17 WHERE➥
	name = "Pudding"');
	$db_conn->exec('UPDATE recipes SET category_id=17 WHERE➥
	category_id=3');
	$db_conn->commit();
} catch (PDOException $e) {
	$db_conn->rollBack();
	echo "Something went wrong: " . $e->getMessage();
}
```

## Stored Procedure
1. 在`MYSQL`里执行以下操作
```sql
delmiter $$
CREATE PROCEDURE get_recipes()
BEGIN
 SELECT name, chef
 FROM recipes
 ORDER BY created DESC ;
END $$
delimiter ;
```
2. `PHP`代码
```php
$db_conn = new PDO('mysql:host=localhost;dbname=recipes', 'php-user', 'secret');
$stmt = $db_conn->query('call get_recipes()');
$results = $stmt->fetchAll();
```

