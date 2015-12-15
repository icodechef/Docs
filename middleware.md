## 中间件(Middleware)

可能你接触过 Larval 或者 slim framework 里的中间件功能，但 Lime 的中间件有所不同，Lime 的中间件更像 Hooks （钩子），可以在某些位置的前后触发你的脚本。

中间件示例:

```php
$app->middleware('router:before', function() {
    if (is_admin()) {
        header('Location', '/login');
        return false;
    }
});

$app->middleware('dispatch', function() {
    // ...
});

$app->middleware('dispatch:after', function() {
    // ...
});
```

Lime 应用中内置执行中间件的位置，分别是 `router`、`dispatch`、`errorHandler` 和 `notFoundHandler`。