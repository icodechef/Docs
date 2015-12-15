## 数据库

PDO 是一个数据库连接抽象类库 — 自 5.1.0 版本起内置于 PHP 当中 — 它提供了一个通用的接口来与不同的数据库进行交互。

Lime 的 \Lime\Pdo 数据库类只是简单地对 PDO 包装起来, 方便使用而已。

### 创建一个 \Lime\Pdo 对象

```php
$pdo = new \Lime\Pdo([
    'dsn'        => 'mysql:host=127.0.0.1;dbname=demo',
    'username'   => 'root',
    'password'   => '123456',
    'charset'    => 'utf8',
    'prefix'     => 'blog_',
]);
```

### 预处理语句

预处理语句并返回一个 PDOStatement 对象。

使用预处理语句的另外一个好处是，如果你在同一会话中多次执行相同的语句，
只被解析和编译一次，提高了速度。

```php
$sql = 'SELECT name, colour, calories
    FROM fruit
    WHERE calories < :calories AND colour = :colour';

$sth = $pdo->prepare($sql, array(PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY));
$sth->execute(array(':calories' => 150, ':colour' => 'red'));
$red = $sth->fetchAll();
$sth->execute(array(':calories' => 175, ':colour' => 'yellow'));
$yellow = $sth->fetchAll();
```

###  执行语句 exec()

执行一条 SQL 语句，并返回受影响的行数

```php
/*  删除 FRUIT 数据表中满足条件的所有行 */
$count = $pdo->exec("DELETE FROM fruit WHERE colour = 'red'");
```

### 执行语句 query()

执行 SQL 语句并并返回一个 PDOStatement 对象

```php
$sql = 'SELECT name, colour, calories
    FROM fruit
    WHERE calories < :calories AND colour = :colour';

$sth = $pdo->query($sql, [':calories' => 150, ':colour' => 'red']);

while($row = $sth->fetch()) {
    // ...
}
```

### 设置字符集

```php
$pdo->setCharset('utf8');
```

### 转义

```php
$pdo->quote("it's a apple.");
```

### 添加表前缀

如果创建 \Lime\Pdo 对象时，设置了 `prefix` 选项，Lime 会自动帮你添加表前缀。

例如，`'prefix' => 'wp_'`

```php
$pdo->query("SELECT * FROM {{post}}");
// 相当于
$pdo->query("SELECT * FROM wp_post");
```

还可以直接返回：

```php
$table = $pdo->tableName('post'); // wp_post
```

### 最后插入行的ID或序列值

```php
$pdo->lastInsertId();
```

### 返回处理后的 SQL 语句

此方法主要用于调试使用

```php
$sql = 'SELECT name, colour, calories
    FROM fruit
    WHERE calories < :calories AND colour = :colour';

echo $pdo->SQL($sql, [':calories' => 150, ':colour' => 'red']);
```

打印输出:

```php
SELECT name, colour, calories FROM fruit WHERE calories < 150 AND colour = 'red'
```

### 事务

```php
$pdo->beginTransaction(); // 开始一个事务

try {
    $sth = $pdo->exec("DROP TABLE fruit");
    $sth = $pdo->exec("UPDATE dessert
        SET name = 'hamburger'");
} catch(Exception $e) {
    $pdo->rollBack(); // 出错了，回滚事务
    exit();
}

$pdo->commit(); // 提交事务
```

如果只是执行一条 SQL 语句，可以这样：

```php
$pdo->transaction('UPDATE dessert SET name = :name', ['name' => 'hamburger']);
```
