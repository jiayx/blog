---
title: PHP框架从零到一 - 自动加载类
categories: 
	- PHP
tags: 
	- 教程
	- php框架
date: 2016-04-28 20:45:00
---

自动加载简单考虑的话，需要一个数组存放自动加载目录。
并使用系统函数spl_autoload_register注册自定义的自动加载函数。
自动加载要符合psr规范

```php
<?php

/**
* 自动加载类 
*/
class Loader
{
	// 自动加载搜索目录
	private static $loadPaths = [];

	// 添加自动加载路径
	public static function addAutoLoadPath($path)
	{
		self::$loadPaths[] = $path;
	}

	// 注册自动加载函数
	public static function register()
	{
		spl_autoload_register('self::autoload');
	}

	// 自动加载函数
	public static function autoload($className)
	{
		$className = str_replace(['\\', '_'], DIRECTORY_SEPARATOR, $className);
		foreach (self::$loadPaths as $loadPath) {
			$loadPath = rtrim($loadPath, DIRECTORY_SEPARATOR);
			$path = $loadPath . DIRECTORY_SEPARATOR . $className . '.php';
			if (self::import($path)) {
				break;
			}
		}
	}

	// 包含一个已存在的文件
	public static function import($path)
	{
		return file_exists($path) ? include_once($path) : false;
	}

}
```
有这个类之后，配合前文中的入口文件，就可以继续了。
接下来我们需要一个路由。
