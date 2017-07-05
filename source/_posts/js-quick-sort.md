---
title: js 实现的快速排序
categories: 其他
tags: []
date: 2016-11-21 13:03:00
---

js 实现的快速排序
目前是升序排列，改成降序只需要将 `arr[j] >= mid` 和 `arr[i] <= mid` 中的 >= 和 <= 互换
算法优化：
* 随机取基准数
* 采用三数取中的方式取基准数
* 多种排序算法配合 - 当数组元素比较少的时候采用插入排序


```js
const quickSort = (arr, left, right) => {
    if (left < right) {
        // 取第一个数为基准数
        let mid = arr[left];
        let i = left;
        let j = right;
        let temp = 0;

        while (i < j) {
            // 先从 *右边* 开始找 *小于* 基准数的值
            while (i < j && arr[j] >= mid) {
                // 在 i < j 的前提下 直到找到小于基准数的值 本次循环结束
                j--;
            }
            // 再从 *左边* 开始找 *大于* 基准数的值
            while (i < j && arr[i] <= mid) {
                // 在 i < j 的前提下 直到找到大于基准数的值 本次循环结束
                i++;
            }
            // console.log(i, j);
            // 交换 - 这里 肯定是 i <= j 的，加上 i < j 的判断可以少一次无用的赋值
            if (i < j) {
                temp = arr[j];
                arr[j] = arr[i];
                arr[i] = temp;            
            }
        }

        // 到这里的话 i = j
        arr[left] = arr[i];
        arr[i] = mid;
    
        // 再排左半边
        quickSort(arr, left, i - 1);
        // 再排右半边
        quickSort(arr, i + 1, right);
    }
}

var arr = [15, 4, 23, 32, 2, 7, 14, 24, 3, 1, 44, 51, 17];
// var arr = [153, 234, 4364, 565, 32423, 2, 5, 55, 35, 23, 24, 23, 32, 22, 27, 14, 4, 3, 1, 14];

quickSort(arr, 0, arr.length - 1);
console.log(arr);

```
