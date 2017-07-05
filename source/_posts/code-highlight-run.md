---
title: 代码高亮插件可以跑起来了！
categories: PHP
tags: [php]
date: 2015-09-03 02:10:00
---

经过一晚上的折腾，代码高亮插件终于跑起来了，这是我写的第一个typecho插件~基于Syntaxhighlighter，主题是漂亮的Monokai，感谢[龙哥][1]提供的css文件，以及帮忙修改Syntaxhighlighter源码。
制作插件过程中发现footer里的js总是显示不出来，调整半天无果。最后在[这里][2]找到了答案，原来是我在用的这个主题footer.php 中少了下面一句代码：
```php
<?php $this->footer();?>
```
这个代码高亮插件基于typecho自带的markdown编辑器，插入代码时需要使用markdown语法
有空会继续完善这个插件的

------
PS:之前下了几个代码高亮插件一总是没什么效果，大概就是因为少了上述代码。

  [1]: http://wanglong.name
  [2]: http://segmentfault.com/a/1190000002469981
