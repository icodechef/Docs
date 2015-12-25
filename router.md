## 路由

Lime 框架的路由功能将 URI 映射到特定 HTTP 请求方法（比如：GET，POST，PUT，DELETE，OPTIONS 或 PATCH）的回调函数上。如果 Lime 应用未找到与 HTTP 请求 URI 和方法相配备的路由，将返回一个 `404 Not Found` 响应。

### 基本路由

大多数基本的 Lime 路由都接受一个 `资源 URI` 和 一个 `闭包(Closure)` 参数。

一个 `HTTP 请求方法`，

#### 基本 GET 路由

```php
$app = new \Lime\Lime();
$app->get('/', function() {
    return 'Hello World!';
});
```

#### 其他基础路由(POST，PUT，DELETE，OPTIONS 或 PATCH)

请求方法主要通过第一个参数来设定：

```php
$app = new \Lime\Lime();

$app->post('/users/id', function() {
    return 'Hello World!';
});

$app->put('/users/id', function() {
    return 'Hello World!';
});

$app->delete('/users/id', function() {
    return 'Hello World!';
});
```

#### 为多种请求注册路由

```php
$app->map(['GET', 'POST'], '/users/id', function() {
    return 'Hello World!';
});
```

#### 注册路由响应所有 HTTP 请求

```php
$app->any('/users/id', function() {
    return 'Hello World!';
});
```

### Method 覆盖

浏览器并不原生支持 HTTP 的 PUT、DELETE、OPTIONS 请求。为了解决这个问题，请确保你的 HTML 表单的请求方式为 `post`，然后像下面例子一样，在 HTML 表单中添加一项请求方式覆盖参数 `_METHOD`：

```php
<form action="/user/1" method="post">
    ...
    <input type="hidden" name="_METHOD" value="DELETE">
    <input type="submit" value="Delete">
</form>
```

### 路由参数

可以在路由的 `资源 URI` 中加入参数。

#### 基础路由参数

在下面例子中，在路由 `URI` 中加入了一个参数：`<name>`。

```php
$app->get('/user/<name>', function($name) {
    return "Hello, {$name}!";
});
```

#### 可选路由参数

在路由 `URI` 中，`“(”`，`“)”` 之间的内容为可选：

```php
$app->get('/user(/<name>)', function($name = null) {
    $name = is_null( $name ) ? 'World' : $name;
    return "Hello, {$name}!";
});
```

路由字段 `name` 是可选的，这个路由将会接受如下 HTTP 请求：

```php
- /user
- /user/admin
- /user/visitor
```

#### 带默认值的路由参数

```php
$app->get('/user(/<name>)', function($name = 'World') {
    return "Hello, {$name}!";
});
```

如果一个 HTTP 请求的可选参数被省略，回调函数中定义的默认值会被代替使用。

#### 使用条件限制数组

指定路由参数的满足条件。如果未满足指定条件，那么路由就不会执行。

```php
$app->get('/post/<id>', function($id) {
    return $id;
})->conditions(['id' => '[0-9]+']);
```

上面的例子，路由字段 `id` 是有条件限制的，必须为数字，这个路由将会接受如下 HTTP 请求：

```php
- /post/10
- /post/250
```

### 资源式路由

在路由中指定网络上的资源，也可以说是控制器和动作，我们只要将`闭包(Closure)` 参数改为字符串，并使用 `controller@action` 形式。使用资源式路由前要先指定资源目录 `resource.path`，这个下面会说。例如，你的控制器类全名为 `\Controllers\Post`，调用的方法为 `new()`，你可以像这样注册一个路由：

```php
$app->post('/post/new', '\Controllers\Post@new');
```

### 资源目录

你可以在你的资源目录中存放控制器、模型等。可以使用下面的方法设置资源目录：

新建一个 Lime 应用实例时：

```php
$app = new \Lime\Lime([
    'resource.path' => '/app',
]);
```

或者

```php
$app->configure('resource.path', '/app');
```

这时资源式的路由会将资源定位到 `/app` 文件夹下。

### 命名路由

命名路由可以让你使用特定路由生成 URL。

```php
$app->get('/user/<name>', function($name) {
    // ...
})->name('user');
```

### 路由分组

```php
$app->group('/post', function() {
    $this->get('/new', function() {
        // code ...
    });

    $this->post('/del', function() {
        // code ...
    });
});
```

路由分组的过滤函数

```php
$app->group('/post', function() {
    // code ...
}, function() {
    // code ..
});
```

### 路由过滤

路由过滤会在路由回调函数被调用前按顺序被执行。我们可以在路由过滤中进行判断或者其他操作：

```php
$app->post('/user/add', function() {
    // ...
})->filter(function($params) {
    if ($_POST['token']) {
        header('Location', '/index.php');
    }
});
```

如果调用每个路由过滤时返回 `false` 则路由过滤会中断：

```php
$app->get('/post/list', function() {
    // ...
})->filter(function() {
    // 1
})->filter(function() {
    // 2
    return false;
})->filter(function() {
    // 3
    // 不会被执行
});
```

此路由的第三个路由过滤函数不会被执行。

### 添加参数

```php
$app->get('/user/<name>', function($name) {
    // ...
})->params('id', 10);
```