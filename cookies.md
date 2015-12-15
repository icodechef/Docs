## Cookies

### 为什么要使用 Lime 的 cookies 方法

本质上，Lime 还是使用 PHP 原生的 `setCookie()` 来对 cookies 进行赋值，
但是对 cookie 的 $name 添加了前缀，而对 $value 添加了验证信息，
所以相对于原生的 `setCookie()` 来说，可以一定程序上避免冲突和增加安全。

### 赋值

```php
cookie()->set('user_id', '10', 3600);
```

创建了一个名为 `user_id`，值为 `10`, 从现在开始1小时后过期的 HTTP cookie。

`set()` 采用了和 PHP 原生的 `setCookie()` 函数相同的参数标识。

```php
cookie()->set(
        $name,
        $value,
        $expire,
        $path,
        $domain,
        $secure,
        $httponly
    );
```

### 返回 Cookie

```php
cookie()->get('user_id');
```

### 删除 Cookie

```php
cookie()->del('user_id');
```