---
title: 商老板面试题
categories: Golang
tags: [go]
date: 2015-12-27 12:17:00
---

闲来无事把周五商老板说的面试题解了一下
具体代码如下(Golang)
```go
package main

import (
    "fmt"
)

const SIZE = 6
var a = [SIZE][SIZE] int {
    {2, 2, 3, 3, 5, 6},
    {2, 1, 3, 3, 4, 6},
    {2, 2, 2, 1, 1, 6},
    {1, 2, 4, 4, 5, 5},
    {2, 2, 2, 1, 1, 5},
    {1, 4, 4, 1, 3, 3},
}

var count int = 0 // 计数
var mark = [SIZE][SIZE] int {} // 用来标记该数字是否被遍历过

func main() {

    fmt.Println("原数据:", "\n")
    lenA := len(a); 
    for i := 0; i < lenA ; i++ {
        fmt.Println(a[i])
    }

    fmt.Println("\n计算结果:")

    for i := 0; i < lenA; i++ {
        lenX := len(a[i])
        for j := 0; j < lenX; j++ {
            if mark[i][j] == 0 {
                findSame(i, j, a[i][j])
                count ++
            }
        }
    }

    fmt.Printf("共%d个簇\n\n", count)

    fmt.Println("具体情况如下:\n")
    lenMark := len(mark)
    for i := 0; i < lenMark ; i++ {
        fmt.Println(mark[i])
    }
}

func findSame(i, j, x int) {

    tempI := i
    tempJ := j
    if mark[i][j] == 0 {

        mark[i][j] = count + 1
        // 往上找相同的数
        for {
            i--
            if i < 0 || a[i][j] != x {
                break
            }
            findSame(i, j, a[i][j])
        }
        // 往下
        i = tempI
        for {
            i++
            if i > SIZE - 1 || a[i][j] != x {
                break
            }
            findSame(i, j, a[i][j])
            
        }
        // 往左
        i = tempI
        for {
            j--
            if j < 0 || a[i][j] != x {
                break
            }
            findSame(i, j, a[i][j])
        }
        // 往右
        j = tempJ
        for {
            j++
            if j > SIZE - 1 || a[i][j] != x {
                break
            }
            findSame(i, j, a[i][j])
        }
    }
}
```
### 更新
经商老板指点，findSame中的for循环没必要。下面是改过之后的findSame
```go
func findSame(i, j, x int) {

    if mark[i][j] == 0 {
        mark[i][j] = count + 1
        // 往上找相同的数
        if i-1 >= 0 && a[i-1][j] == x {
            findSame(i-1, j, a[i-1][j])
        }
        // 往下
        if i+1 < SIZE && a[i+1][j] == x {
            findSame(i+1, j, a[i+1][j])
        }
        // 往左
        if j-1 >= 0 && a[i][j-1] == x {
            findSame(i, j-1, a[i][j-1])
        }
        if j+1 < SIZE && a[i][j+1] == x {
            findSame(i, j+1, a[i][j+1])
        }
    }
}
```
对递归理解还是不够到位啊~不过现在应该OK了^_^
