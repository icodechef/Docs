## 模型

在 Lime 中模型的作用是暂存数据以及与其相关的业务逻辑，这些数据将被视图所显示。模型是 `\Lime\Model` 或其子类的实例。

模型类的例子：

```php
class User extends \Lime\Model
{
    public function __construct()
    {
        //...
    }
}
```

### 添加数据

使用 `add()` 方法将数据添加到模型中：

```php
class Friut extends \Lime\Model
{
    public function __construct($friuts = [])
    {
        foreach ($friuts as $friut) {
            $this->add($friut);
        }
    }
}
```

```php
$friuts = [
    ['name' => 'apple', 'qty' => 10],
    ['name' => 'banana', 'qty' => 20],
    ['name' => 'lemon', 'qty' => 15],
];

$friut = new Friut($friuts);
```

### 遍历数据

#### 重置位置

```php
$friut->rewind();
```

#### 遍历数据

```php
$friut->each();
```

例子：

```php
while($friut->each()) {
    echo sprintf("%s (%d)", $friut->row('name'), $friut->row('qty')), PHP_EOL;
}
```

#### 序号值

```php
while($friut->each()) {
    echo $friut->seq(), PHP_EOL;
}
```

### 将实例赋值给变量

```php
(new Friut($friuts))->to($friut);
```

### 获取所有数据

```php
$friut->all();
```

### 当前数据

```php
$friut->row();
```

> 方法 `row` 是和方法 `each` 一起使用的

#### 获取当前数据中的某一值

例如当前数据是：

```php
[
    ['name'] => 'apple',
    ['qty'] => '10',
]
```

```php
$friut->row('name');
```

#### 以 `@` 开头表示一个希望使用方法(get前缀的方法)访问数据

```php
class Friut extends \Lime\Model
{
    public function __construct($friuts = [])
    {
        foreach ($friuts as $friut) {
            $this->add($friut);
        }
    }

    public function getName($name)
    {
        return '我是 ' . $name;
    }
}

// ... 省略的代码

$friut->row('@name'); // 我是 apple
```

#### 添加回调函数

```php
$friut->row('name', function($name) {
    return ucfirst($name);
});
```