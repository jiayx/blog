---
title: PHP框架从零到一 - 入口文件
categories: 
    - PHP
tags: 
    - 框架
    - 教程
    - php框架
date: 2016-04-28 20:12:00
---

不知道我的步骤对不对 反正就记在这了。

先简单介绍下我的目录规划，以后会有所变动，现在一切从简。
```php
/
---app
    ---Controller
        Home.php
    ---Config
        ---local
            db.php
            ...
    ---Model
    ...
---system
    Loader.php
    Uri.php
    Route.php
    Config.php
---runtime
    ...
index.php
```

第一步：入口文件；
一件很重要的事情，原因看注释。

```php
<?php
// 修正cli模式下运行目录不同部分系统函数取值不同的问题 比如: getcwd
defined('STDIN') AND chdir(__DIR__);
```

定义一些必要的常量

```php
<?php
define('APP_FOLDER', 'app');
define('SYSTEM_FOLDER', 'system');

define('ROOT_PATH', __DIR__ . DIRECTORY_SEPARATOR);
define('APP_PATH', ROOT_PATH . APP_FOLDER . DIRECTORY_SEPARATOR);
define('SYSTEM_PATH', ROOT_PATH . SYSTEM_FOLDER . DIRECTORY_SEPARATOR);
```

然后要手动导入自动加载类，并且添加自动加载路径，注册自动加载函数。
这样之后的类就可以自动加载了。

```php
include SYSTEM_PATH . 'Loader.php';

Loader::addAutoLoadPath(SYSTEM_PATH);
Loader::addAutoLoadPath(APP_PATH);
Loader::register();
```

下一篇是Loader类的具体实现。
