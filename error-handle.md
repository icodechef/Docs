## 错误处理

自定义你的错误处理程序

```php
$app->middleware('errorHandler', function($e) {
    return view('error.php', ['exception' => $e]);
});
```

自定义你的 Not found 处理程序

```php
$app->middleware('notFoundHandler', function($e) {
    return view('404.php', ['exception' => $e]);
});
```

自定义你的错误处理视图文件, 例如上例中的 `404.php`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Not Found</title>
</head>
<body>
    <section>
        <header>
            <h1 class="page-title">Oops! That page can't be found.</h1>
        </header>
        <div>
            <p>It looks like nothing was found at this location</p>
        </div>
    </section>
</body>
</html>
```