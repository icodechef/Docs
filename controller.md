## 控制器

Lime 虽然有 MVC 设计结构，控制器处理请求和创建一个应答，但与其他框架不同，Lime 的控制器并不要求控制器继承自框架里的某个基类，方法的命名也没有要求，完全由用户自定义，简单地说控制器就是一个类，动作是这个类的一个方法。要使用控制器就需要使用前面所讲的 `资源式路由` 和 `资源目录`。

简单示例：

```php
// 定义路由
$app->post('/post/new', '\Controllers\Post@new');
```
注意：控制器使用命名空间可以减少类的冲突

```php
// 控制器被保存在 resources/Controllers/Post.php

use Controllers;

class Post
{
    public function new()
    {
        // 输入要执行的脚本
    }
}
```
