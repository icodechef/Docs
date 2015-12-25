## 系统需求

* PHP >= 5.4.0

## 安装

Lime 的安装由如下两步组成:

* 从 [Github](https://github.com/icodechef/Lime) 下载 Lime 框架
* 将 Lime 压缩包解压至一个 Web 可访问的目录

## 示例

[examples](https://github.com/icodechef/Lime/tree/master/example)

## 基本用法

一个 Lime 应用主要包含三部分，生成 Lime 应用实例，定义路由，执行应用。

```php
// 加载 Lime 框架
require 'Lime/Lime.php';

// 生成一个 Lime 应用实例
$app = new \Lime\Lime();

// 定义一个 HTTP GET 请求路由：
$app->get('/', function() {
    echo 'hello world';
});

// 执行 Lime 应用
$app->run();
```