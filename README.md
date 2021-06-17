# php-debug

使用Docker Compose搭建php7.4调试环境，支持gdb, xdebug, vld等工具调试php。


## xdebug

configure PHP to get Xdebug running:  [https://xdebug.org/wizard.php](https://xdebug.org/wizard.php)

* analysize php

```php
<?php
$a = "hello";
$b = $a;
xdebug_debug_zval('a');
?>
```

## VLD

使用VLD扩展查看opcode

```shell
php -dvld.active=1 test.php
# 设置显示详细内容级别
php -dvld.active=1 -dvld.verbosity=3 test.php
# 不执行PHP代码
php -dvld.active=1 -dvld.execute=0 test.php
```



## 运行

```shell
docker-compose build
docker-compose up -d
docker-compose exec php bash
```
进入容器www目录后执行gdb即可调试

```shell
gdb php
b main
r test.php
```# php-debug
# php-debug
# php-debug
