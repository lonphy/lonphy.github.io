---
layout: post
title:  "Go Module 使用私有仓库gitlab"
date:   2019-07-24 20:11:04 +0800
category: golang
tags: gomod
---

#### 前提条件
- git 版本保持最新(v2.22.0)
- golang v1.11.x
- gitlab v8.16.4(比较老的版本了，其他没有验证，高版本应该没有问题)

#### 获取访问令牌
1. 登录gitlab
2. 进入`个人设置`->`访问令牌选项卡`, 创建个人访问令牌(`Scopes`建议选择`read_user`, 其他两项自己随意)
3. 填好后，点击`创建个人访问令牌`, 结果token记录好,后续无法查询


#### 配置 `~/.gitconfig`
```bash
vim ~/.gitconfig
```
输入以下内容(主要是`url`及`http`两小节)

```.gitconfig
[user]
    name = test
    email = test@example.com
[url "git@git.example.com:"]
    insteadOf = http://git.example.com/
[http]
    extraheader = PRIVATE-TOKEN: "你的gitlab访问token"
```

说明:
- `[url "git@git.example.com:"]` : 是要被替换的仓库域名, 替换成自己的即可, 当然也可以写具体项目git地址
- `insteadOf = http://git.example.com/` : 是实际访问使用地址形式, 这里**注意**, 如果gitlab里的配置是http就一定写http， https就写https， 否则不通
- `extraheader = PRIVATE-TOKEN: "你的gitlab访问token"` : http(s)形式访问token, 具体获取方式见上面
