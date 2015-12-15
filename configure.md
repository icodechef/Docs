## 配置

Lime 几乎不需要任何配置就能开箱即用。你能立即开始你的编码工作！

### 基本用法：

Lime 提供了两种方式对其进行配置。一种是在生成实例的时候进行参数设置：

```php
$app = new \Lime\Lime([
    'debug' => true,
    'resource.path' => __DIR__ . '/app',
    'views.path' => __DIR__ . '/views',
]);
```

另一种则是在生成实例之后：

注意：这里的 $app 为你创建的 Lime 应用实例，其他例子雷同。

```php
$app->configure('debug', true);
$app->configure('views.path', __DIR__ . '/views');

$app->configure([
    'debug' => true,
    'resource.path' => __DIR__ . '/app',
    'views.path' => __DIR__ . '/views',
]);
```

ini 文件：

```php
$app = new \Lime\Lime(parse_ini_file('config.ini'));
```

简单的来说，配置以一个 `数组` 形式传入。

### 获取配置值

```php
$app->configure('views.path');

configure('views.path'); // 这是辅助函数，请参考辅助函数部分
```

### 应用配置

* `debug` 调试
* `views.path` 视图目录
* `resource.path` 资源目录
* `services` 服务
* `middleware` 中间件

视图目录和资源目录具体内容请参考视图和资源式路由。