## 请求

一个应用的请求是用 \Lime\Request 对象来表示的，该对象提供了诸如请求参数(通常是 GET 参数或者 POST 参数)、HTTP 头、方法等信息。

### 取得请求实例

每个 Lime 应用实例都有一个默认的 \Lime\Request 对象。虽然是这样，但是你可以在任何需要的地方创建自己的请求对象而完全不影响应用。

你可以用如下方式获得 Lime 应用 \Lime\Request 对象的一个引用：

```php
// 方法 1: 
$request = $app->request;

// 方法 2: 
$request = \Lime\Lime::app()->request;

// 方法 3: 
$request = \Lime\Lime::app('request');

// 方法 4: 
$request = app('request');
```

创建自己的请求对象：

```php
$request = new \Lime\Request();
```

### 取得请求 URI

```php
$request->uri();
```

### 取得请求方法

```php
$request->method();
```

### 判断一个请求是否某方法

```php
$request->isGet();     // GET 请求方法?
$request->isPost();    // POST 请求方法?
$request->isPut();     // PUT 请求方法?
$request->isDelete();  // DELETE 请求方法?
$request->isPatch();   // PATCH 请求方法?
$request->isHead();    // HEAD 请求方法?
$request->isOptions(); // OPTIONS 请求方法?
```

### 判断是否一个 AJAX 请求

```php
$request->isAjax();
```
### 获取请求来源地址

```php
$request->referer();
```

### 取得客户端的IP地址

```php
$request->ip();
```

### 取得服务器和执行环境信息

```php
$request->server('PHP_SELF');
```

### 根据请求地址取得各部分

例如请求地址为：

```php
http://example.com:80/index.php/post/10?page=10
```

```php
$request->scheme();      // http
$request->host();        // example.com:80
$request->hostname();    // example.com
$request->host();        // 80
$request->pathname();    // post/10
$request->scriptName();  // index.php
$request->query();       // page=10
```

### 获取一个请求参数

当前 HTTP 请求发送的 GET 或者 POST 参数都可以通过 `input()` 方法获取。`input()` 方法首先查找 POST 参数，最后才查找 GET 参数。如果没有找到参数，会返回默认值或者 null。

```php
$request->input('name');
$request->input('name', 'admin'); // 此处的 'admin' 为默认返回值
```

查找指定请求方式的参数

```php
// GET 参数
$request->get('name');
// POST 参数
$request->post('name');
```

### 从资源 URI 中获取参数

```php
$request->params('id');
$request->params('id', 1); // 设定默认值
```

### 请求 URLs

\Lime\Request 对象提供了许多方式来检测当前请求的 URL。

假设被请求的 URL 是 `http://example.com/blog/index.php/post/10?page=10`。 
你可以像下面描述的那样获取 URL 的各个部分：

* $request->url():      返回 `/blog/index.php/post/10?page=10`, 不包括 host info 部分
* $request->baseUrl():  返回 `/blog/index.php`, 没有 path info 和查询字符串部分。
* $request->basePath(): 返回 `/blog`, host info 之后, 入口脚本之前的部分。
* $request->pathInfo(): 返回 `/post/10`, 这个是入口脚本之后, 问号之前(查询字符串)的部分。