## Dependency Injection

Lime 的服务容器是用于管理类依赖的工具，为将对象（例如数据库，日志等）注入到 Lime 应用或重写应用内置对象（例如 Request、Response、Log）提供了一个非常简单的方法。

### 注入服务

在服务提供者里，总是通过 Lime 应用实例变量使用容器。

```php
$app->service('database', // 服务名称
    ['className' => '\Lime\Pdo',
     'arguments' => [
        [
            'dsn'        => 'mysql:host=127.0.0.1;dbname=demo',
            'username'   => 'root',
            'password'   => '123456',
            'charset'    => 'utf8',
        ],
    ]], // 服务的定义
    false // 是否共享服务
);
```

* `服务名称` 是唯一的
* `服务的定义` 可以是一个匿名函数，一个类实例或者是一个包含 `className` 和 `arguments` 的数组
* `共享服务` 也就是单件模式, 全局只有一个实例

我们还可以通过配置来延迟注入服务

```php
$app = new \Lime\Lime([
    'service' => [
        [
            'database', // 服务名称
            [
                'className' => '\Lime\Pdo',
                'arguments' => [
                    [
                        'dsn'        => 'mysql:host=127.0.0.1;dbname=demo',
                        'username'   => 'root',
                        'password'   => '123456',
                        'charset'    => 'utf8',
                    ]
                ],
            ], // 服务的定义
            false // 是否共享服务
        ],
        // ... 其他服务
    ],
]);
```

### 获取服务

```php
$app->database;
```

传入实参到服务:

```php
$app->database([
    'dsn'        => 'mysql:host=127.0.0.1;dbname=demo',
    'username'   => 'root',
    'password'   => '123456',
    'charset'    => 'utf8',
]);
```