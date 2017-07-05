---
title: SegmentFault 双11题目 golang 解法
categories: Golang,其他
tags: []
date: 2016-11-11 18:43:00
---

```go
package main

import (
    "encoding/base64"
    "fmt"
    "io"
    "os"
    "regexp"

    "github.com/imroc/biu"
)

func main() {
    
    s := `二进制字符串`
    s = strings.Replace(s, "_", "1", -1)
    arr := regexp.MustCompile("\\d+").FindAllString(s, -1)

	var r = make([]byte, 0, len(arr))

	for _, x := range arr {
		var a byte
		biu.ReadBinaryString(x, &a)
		r = append(r, a)
	}

	wireteString, _ := base64.StdEncoding.DecodeString(string(r))
	// fmt.Println(wireteString)
	f, err := os.Create("/Users/jiayx/workspace/jiayx/go/src/lean/a.tar.gz") //创建文件
	if err != nil {
		panic(err)
	}
	n, _ := io.WriteString(f, string(wireteString))
	println("写入文件成功, 长度：", n)
}
```
