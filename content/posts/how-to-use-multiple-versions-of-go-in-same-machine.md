---
categories:
  - Golang
  - Go
tags:
  - Golang
  - Go
  - Windows
title: "How to use multiple versions of Go in same machine?"
date: 2023-04-26T21:30:49+07:00
draft: false
---

You have many Golang project in same machine, therefore, you will be hard if switching by reconfig Go path.

## Solution 1: Switch Go lang version by JetBrains GoLand IDE

![goland.png](/img/multiple_versions/goland.png)

## Solution 2: Use commands

Go to https://go.dev/dl/ to see exactly `major.minor.patch` version of installer.

```
go install golang.org/dl/go1.19.8@latest
go1.19.8 download
go1.10 version

go install golang.org/dl/go1.10.7@latest
go1.10.7 download
go1.10 version
```

![multiple_versions.png](/img/multiple_versions/multiple_versions.png)

Reference document at https://go.dev/doc/manage-install

![check_versions.png](/img/multiple_versions/check_versions.png)

