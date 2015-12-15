## 视图

视图为包含HTML代码的脚本文件，Lime 应用通过 \Lime\View 对象来管理， 该对象主要提供通用方法帮助视图构造和渲染，支持 `视图继承` 和 `视图片段` 功能，帮助你灵活地控制页面布局。

例如视图被保存在 `resources/views` 文件夹内。

```html
<!-- 视图被保存在 resources/views/index.php -->
<html>
    <body>
        <h1>Hello, <?php echo $name; ?></h1>
    </body>
</html>
```

这个视图可以使用以下的代码传递到用户的浏览器：

```php
$app->get('/', function() {
    $view = new \Lime\View();
    return $view->render('resources/views/index.php', array(
        'name' => 'World!',
    ));
});
```

如你所见，`render()` 方法的第一个参数会对应到视图文件的位置；第二个参数是一个能够在视图内取用的数据数组。

### `view` 辅助函数

同样地，每个 Lime 应用实例都有一个默认的 \Lime\View 对象，你可以像上面代码一样创建自己的视图对象。如果觉得自己创建视图对象太麻烦了，可以使用 `view()` 辅助函数简化步骤：

```php
$app->get('/', function() {
    return view('resources/views/index.php', array(
        'name' => 'World!',
    ));
});
```

### 视图目录

在配置中，我们提及 `views.path` 视图目录这个配置项。下面我们来说怎么使用它。

例如视图都被保存在 `/resources/views` 文件夹内的话，可以这样：

```php
$app = new \Lime\Lime([
    'views.path' => '/resources/views',
]);

...
    return view('index.php', array(
        'name' => 'World!',
    ));
...
```

### 视图数据

视图数据是以键值数组存储的。传递数据到视图的方式有两种，

一种方式，前面我们已经见过了，就是通过视图对象的 `render()` 方法或者 `view()` 辅助函数的第二个参数直接传递一个数组：

```php
...
    return view('视图文件', array(
        'total' => 100,
        'fruits' => array('apple', 'banana'),
    ));
...
```

另一种是使用视图对象的 `set()` 函数：

```php
$view->set('total', 100);
$view->set('fruits', array('apple', 'banana'));

// 或者
$view->set(array(
    'total' => 100,
    'fruits' => array('apple', 'banana'),
));
```

向视图传递数据后，可以在视图文件中获取以 `键名` 作为变量名的值。
在上面的例子代码中，视图将可以使用 `$total`，`$fruits` 来取得数据，其值分别为 `100`，`array('apple', 'banana')`。

在视图文件中：

```html
<p><?php echo $total; ?></p>
<ul>
<?php foreach ($fruits as $fruit) { ?>
    <li><?php echo $fruit; ?></li>
<?php } ?>
</ul>
```

> 在视图中，可访问 $this 指向 \Lime\View 对象来管理和渲染这个视图文件。

所以我们可以使用用视图对象的 `get()` 方法来获取视图数据，使用 `get()` 方法的好处是可以设置数据不存在时的默认值：

```html
<p><?php echo $this->get('total'); ?></p>
<ul>
<?php foreach ($this->get('fruits', []) as $fruit) { ?>
    <li><?php echo $fruit; ?></li>
<?php } ?>
</ul>
```

### 视图继承和视图片段

大多数页面应用都有相同的页头和页尾，传统的方法是把相同的页头和页尾存在不同的文件或者函数，然后按顺序加载。

```html
<!-- 页头 header.php -->
<html>
<head>
    <title>title</title>
</head>
<body>
<header>Header</header>
```

```html
<!-- 页尾 footer.php -->
<footer>footer</footer>
</body>
</html>
```

```html
<!-- 视图 -->
<?php require('header.php') ?>
<article>
    ......
</article>
<?php require('footer.php') ?>
```

我们可以看到这种方法将一个 HTML 整体分割为三部分，但这样做，当每一个页面的页头和页尾不同时，我们就需要添加判断或者新建不同的页头和页尾文件。

视图继承和视图片段将这些公共的部分(如页头和页尾)放到一个布局中，渲染内容视图后在合适的地方嵌入到布局中。

定义一个页面布局

```html
<!-- 视图被保存在 resources/views/layout/main.php -->
<html>
<head>
    <meta charset="utf-8">
    <title>Name: <?php $this->with('title', '视图标题'); ?></title>
</head>
<body>
    <?php $this->with('header') ?>
    <?php $this->content(); ?>
    <footer>我是页脚</footer>
</body>
</html>
```

在视图模板中使用页面布局

```html
<!-- 视图被保存在 resources/views/index.php -->
<?php $this->extend('main.php'); // 视图继承 ?>

<!-- 这区域是视图的内容 -->
Hello, <?php echo $name; ?>
<!-- 这区域是视图的内容 -->

<?php $this->section('title', '我是首页'); // 定义一个视图片段 ?>

<?php $this->section('header'); // 开始定义一个视图片段 ?>
<header>我是头部</header>
<?php $this->end(); // 结束定义一个视图片段 ?>
```

如你所见，在视图模板中，使用 `extend()` 方法继承一个页面布局，方法的参数为页面布局所在的文件。

使用 `with()` 方法在视图中定义一个视图片段，参数为 `片段的名称` 和 `片段的默认内容`，如 `$this->with('title', '视图标题')`、 `$this->with('header')`。

视图片段的内容定义在 `section()` 和 `end()` 之间。如：

```php
<?php $this->section('header'); // 开始定义一个视图片段, 片段的名称为 header ?>
<header>我是头部</header>
<?php $this->end(); // 结束定义一个视图片段 ?>
```

注意，一般 `section()` 和 `end()` 方法应该成对出现, 但传递第二个参数，则不需要 `end()` 了，否则输出并不是你想要的。

```php
$this->section('title', '这里是一个标题');

// 或者
$this->section('title');
这里是一个标题。
$this->end();
```

在页面布局(也就是父模板)中使用 `content()` 方法返回继承自其的视图内容。

```php
<div><?php $this->content(); ?></div>
```

### 嵌套视图

```html
<html>
...
    <?php $this->nest('header.php'); ?>
    ...
    <?php $this->nest('footer.php'); ?>
...
</html>
```