## 辅助函数

### app()

获取应用实例或者可用的容器实例

```php
app(); // 获取当前应用实例

app('request');  // 获取请求实例, 别名: request();
app('response'); // 获取响应实例, 别名: response();
```

### services()

获取可用的容器实例

```php
services('database');
services('cache');
```

注意：services() 与 app() 雷同, 其区别主要在语义上

### configure()

```php
configure('debug', true);
configure('views.path', __DIR__ . '/views');

configure([
    'debug' => true,
    'resource.path' => __DIR__ . '/app',
    'views.path' => __DIR__ . '/views',
]);
```

### request()

返回一个 \Lime\Request 请求实例

```php
request()->isPost();
request()->hostname();
```

### response()

返回一个 \Lime\Response 响应实例

```php
response()->setStatus(302);
response()->header('Location', 'index.php');
```

### view()

渲染视图

```php
view('/templates/index.php');
```

### cookie()

返回一个 \Lime\Cookie 响应实例

```php
cookie()->set('username', 'Lime');
cookie()->get('username');
```

### view()

渲染视图

```php
view('/templates/index.php');
```

### url_site()

返回绝对 URL

假设被请求的 URL 是 `http://example.com/blog/index.php`。 

```php
url_site();             // 返回 `http://example.com/blog/`
url_site(true);         // 返回 `http://example.com/blog/index.php`
url_site('login.php');  // 返回 `http://example.com/blog/login.php`
```

### url_base()

返回相对 URL

假设被请求的 URL 是 `http://example.com/blog/index.php`。 

```php
url_base();             // 返回 `/blog/`
url_base(true);         // 返回 `/blog/index.php`
url_base('login.php');  // 返回 `/blog/login.php`
```

`url_site()` 与 `url_base()` 区别主要在于是否包括 host info 部分

### url_for()

返回指定路由的 URL

例如，定义了一个名称为 'post-edit' 的路由:

```php
$app->get('/post/edit/<pid>', function() {
   // code ... 
})->name('post-edit');
```

其 `URI` 表达式为 `/post/edit/<pid>`, 接着我们可以这样：

```php
url_for('post-edit', ['pid' => 10]); // 结果输出： /post/edit/<pid>
```