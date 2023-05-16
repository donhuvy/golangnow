---
categories:
  - Golang
tags:
  - Golang
  - Go
  - Git
  - GitHub
  - GitLab
title: "Get dependencies from private Git repository and from branch or specific commit hash string"
date: 2023-04-26T21:07:02+07:00
draft: false
---

## Solution for branch

```bash
go get github.com/someone/some_module@master
go get github.com/someone/some_module@dev_branch
```

Or another way to sum up

```bash
go get repo@branchname
go get repo@tag
go get repo@commithash
```

See https://stackoverflow.com/a/55190362/3728901

## Solution for specific commit, then generated automatically to pseudo version

```bash
go get github.com/x/sys@c856192   # records v0.0.0-20180517173623-c85619274f5d
```
See https://jfrog.com/blog/go-big-with-pseudo-versions-and-gocenter/

## Solution for private repository on Windows
Windows + R, type (3 dots)

```bash
...
```

![net_rc_windows.png](/img/dependency_git/net_rc_windows.png)

![net_rc_content_windows.png](/img/dependency_git/net_rc_content_windows.png)

