---
layout: post
title:  "golang 分析工具使用"
date:   2019-02-11 01:00:04 +0800
category: golang
---

## 基本命令
```sh
go tool pprof executable-file profile-file
```

##### 解释
`top` 输出格式
1. 采样点落在该函数中的次数
2. 采样点落在该函数中的百分比
3. 上一项的累积百分比
4. 采样点落在该函数， 以及被它调用的函数中的总次数
5. 采样点落在该函数，以及被它调用的函数中的总次数百分比
6. 函数名

## CPU性能分析测试
1. **生成cpu profile**
```sh
go test -bench=".*" -cpuprofile=cpu.prof
```
2. **生成可供分析的2进制文件**
```sh
go test -bench=".*" -cpuprofile=cpu.prof -c
```
> 会生成一个以包名+`.test`命名的2进制文件

3. **分析**
```sh
go tool pprof xxx.test cpu.prof
```