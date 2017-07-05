---
title: PHP 官方文档中 RecursiveArrayIterator 的例子
categories: PHP
tags: []
date: 2017-06-18 10:48:00
---

PHP 官网文档在介绍 RecursiveArrayIterator 时候举了一个例子，其中用到了 iterator_apply 这个函数。

然后我就去查了下 iterator_apply 的用法，介绍是这样说的：
> iterator_apply — 为迭代器中每个元素调用一个用户自定义函数

当然 回调函数中如果不返回 true 迭代就会终止。

那个例子是这样写的
```php
$myArray = array( 
    0 => 'a', 
    1 => array('subA','subB',array(0 => 'subsubA', 1 => 'subsubB', 2 => array(0 => 'deepA', 1 => 'deepB'))), 
    2 => 'b', 
    3 => array('subA','subB','subC'), 
    4 => 'c' 
); 

$iterator = new RecursiveArrayIterator($myArray); 
iterator_apply($iterator, 'traverseStructure', array($iterator)); 

function traverseStructure($iterator) { 
    
    while ( $iterator -> valid() ) { 

        if ( $iterator -> hasChildren() ) { 
        
            traverseStructure($iterator -> getChildren()); 
            
        } 
        else { 
            echo $iterator -> key() . ' : ' . $iterator -> current() .PHP_EOL;    
        } 

        $iterator -> next(); 
    } 
}
```

所以我有点懵逼，既然函数里面做了循环，并且没返回 true，说白了函数只调用了一次，为啥不能好好调一次函数，非要搞这些骚东西。。。

如果把例子中的函数改成这样的话，虽然看起来更复杂了，但是这样才能体现出使用 iterator_apply 函数的意义。如下：
```php
function traverseStructure($iterator) {
    if ( $iterator -> hasChildren() ) {
        $children = $iterator->getChildren();
        iterator_apply($children, 'traverseStructure', [$children]);
    } else {
        echo $iterator -> key() . ' : ' . $iterator -> current() .PHP_EOL;
    }

    return true;
}
```

