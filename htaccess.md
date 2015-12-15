## 单入口

在单入口 Web 程序, 可以通过 `Apache` 的 `htaccess` 文件移除 URL 中的 index.php。

如果你的 `Apache` 服务器启用了 `mod_rewrite` ，你可以简单的通过一个 `.htaccess` 文件再加上一些简单的规则就可以移除 `index.php` 了。下面是这个文件的一个例子：

```php
# Turn on URL rewriting
RewriteEngine On

# Installation directory
#RewriteBase /

# Protect hidden files from being viewed
<Files .*>
    Order Deny,Allow
    Deny From All
</Files>

# Allow any files or directories that exist to be displayed directly
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d

# Rewrite all other URLs to index.php/URL
RewriteRule .* index.php/$0 [PT]
```

注意：这些规则并不是对所有 Web 服务器都有效。