---
title: PHP 输出 PDF 文档
categories: PHP
tags: []
date: 2016-10-12 15:49:00
---

需求是用 HTML 生成 PDF 所以就找啊找
找了半天，有以下几个主流的库
1. tcpdf
2. html2pdf
3. dompdf

试用之后，只有 dompdf 能比较完美的还原 css 样式
但是默认对中文支持不好。
暂时不知道怎么解决，百度搜到的全是 N 年前的解决方案，而且千篇一律 --

UPDATE: 
下午看 dompdf 项目的时候，发现一个项目 https://github.com/dompdf/utils 。
这个项目就是用来导入其他字体的，我用它导入了微软雅黑。
这个工具 依赖 https://github.com/PhenX/php-font-lib 和 https://github.com/PhenX/php-svg-lib
我是把 https://github.com/dompdf/utils 下载到 dompdf 的根目录下使用的
执行 `php load_font.php font_name /path/to/font.ttf` 会在 dompdf/lib/fonts/ 下生成 以 font_name 命名的两个文件。这时候就可以在生成的 PDF 里愉快的显示中文了。
不过执行上述命令时，会报错 
>PHP Warning:  require_once(/Users/jiayx/workspace/fdd/trade-center/vendor/dompdf/dompdf/lib/php-font-lib/src/FontLib/Autoloader.php): failed to open stream: No such file or directory in /Users/jiayx/workspace/fdd/trade-center/vendor/dompdf/dompdf/autoload.inc.php on line 20

这种情况需要把 php-font-lib 和 php-svg-lib 复制一份到 dompdf/lib/ 目录下，他俩在 vendor/phenx/ 下
然后再执行上述命令，这时候就成功了。(虽然还是会报一堆 warning --

示例代码
```php
<?php

use Dompdf\Dompdf;
use Dompdf\Options;

$html = '<html>......</html>';

$options = new Options();
// 是否加载远程文件 比如互联网上的图片、字体等。
$options->setIsRemoteEnabled(true);
// 这个不能没有
$options->setIsFontSubsettingEnabled(true);
// 设置字体
$options->setDefaultFont('msyh');    

$dompdf = new Dompdf($options);
$dompdf->loadHtml($html);
$dompdf->setPaper('A4');
$dompdf->render();
// 第二个参数加上可以在 chrome 上预览 PDF，比较方便，不加会直接下载
$dompdf->stream('收款清单', ["Attachment" => 0]);

```

遇到的一个小问题：
不能 在 html 里设置 font-family 否则会按照 html 中的设置显示。


