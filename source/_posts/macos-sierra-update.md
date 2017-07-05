---
title: macOS Sierra 更新
categories: Golang,其他
tags: [golang,brew,macos]
date: 2016-09-21 22:11:00
---

一直以来我都是看到新系统就不能忍。。想在第一时间更新，今天 macOS 正式版推送了，就立马更新了一波。
说一说遇到的问题吧。

### golang 遇到的问题

更新之后，执行 `go build` 会提示错误

    fatal error: MSpanList_Insert

我还以为我昨天把代码改坏了。。。一番查证之后，发现是由于我目前的 golang 1.6 不支持 macOS，更新 golang 到1.7.1之后问题解决。

### brew 遇到的问题

还有更新 golang 过程中遇到的问题。
更新 golang 就少不了 brew 的帮忙，在我执行 `brew upgrade go` 的时候注意到命令行提示：

    /usr/local is not writable. You should change the ownership 
    and permissions of /usr/local back to your user account: 
    sudo chown -R $(whoami) /usr/local

(百度是真的不靠谱...) google 查到是因为 mac 系统升级的问题。只需要按照提示执行 `sudo chown -R $(whoami) /usr/local` 就 ok 了。
> 更新: homebrew 更新了，不再需要 修改 `/usr/local` 的权限了

这是目前我发现的升级 macOS Sierra 遇到的问题，之后遇到会继续更新。
