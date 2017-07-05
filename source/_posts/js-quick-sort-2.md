---
title: js实现的快速排序2
categories: 其他
tags: []
date: 2016-11-30 17:23:00
---

> 看了别人的快速排序实现之后，写了新的一版快排，不同之处就是数组的分割函数。
> 比上一版看起来更容易理解，而且实现的更加高明一些？(不确定)

```javascript
var quickSort = function(arr, left , right) {
    if (left < right) {
        var pivotIndex = partition(arr, left, right);
        quickSort(arr, left, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, right);
    }
}

var partition = function(arr, left, right) {
    var pivotIndex = left;
    var pivotValue = arr[left];
    // 升序排列 所以把基准数 放到后面
    swap(arr, pivotIndex, right);

    var storeIndex = left;
    for (i = left; i < right; i++) {
        if (arr[i] < pivotValue) {
            swap(arr, storeIndex, i);
            storeIndex++;
        }
    }

    swap(arr, storeIndex, right);

    return storeIndex;
}

var swap = function(arr, i, j) {
    var temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

var arr = [15, 4, 23, 32, 2, 7, 14, 24, 3, 1, 44, 51, 17];
quickSort(arr, 0, arr.length - 1);
console.log(arr);
```
