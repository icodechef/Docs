## 响应

一个应用的响应是用 \Lime\Response 对象来表示的，响应对象包含的信息有 HTTP 状态码，HTTP 头和主体内容等

### 取得响应实例

与请求一样，每个 Lime 应用实例都有一个默认的 \Lime\Response 对象，你可以在任何需要的地方创建自己的响应对象而完全不影响应用。

你可以用如下方式获得 Lime 应用 \Lime\Response 对象的一个引用：

```php
// 方法 1: 
$response = $app->response;

// 方法 2: 
$response = \Lime\Lime::app()->response;

// 方法 3: 
$response = \Lime\Lime::app('response');

// 方法 4: 
$response = app('response');
```

创建自己的响应对象：

```php
$response = new \Lime\Response();
```

### 基本响应

从路由返回字符串

```php
$app->get('/', function() {
    return 'Hello World!';
});
```

直接输出字符串

```php
$app->get('/', function() {
    echo 'Hello World!';

    // ...

    echo 'End';
});
```

### HTTP 响应的状态码

设置

```php
$response->setStatus(404);
```

返回

```php
$response->getStatus();
```

### HTTP 响应首部字段

设置

```php
$response->header('Content-Type', 'application/json');
$response->header('Content-Type', 'application/json', true); // true 表示替换前面相同类型的字段
```

返回

```php
$response->getHeader('Content-Type');
```

删除

```php
$response->removeHeader('Content-Type');
```

清空

```php
$response->clean();
```

### HTTP 响应主体

设置

```php
$response->setBody('Hello');
$response->setBody(' world!'); // 覆盖现有的报文主体
// 主体内容为 world!

$response->setBody('Hello');
// 追加响应主体
$response->setBody('world!', false);
// 或者
$response->write('Bar');
```

返回

```php
$response->getBody();
```

### 重定向

设置 `response 状态码` 和 `Location：header` 的响应。

```php
$response->redirect('/login', 303);
```