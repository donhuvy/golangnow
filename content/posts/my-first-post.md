---
title: "Create a static blog by Hugo"
date: 2023-04-26T10:29:47+07:00
draft: false
---

Build static blog by Hugo CMS

Windows 11, run CMD as Administrator

```bash
choco install hugo-extended
hugo version

cd /d D:\
mkdir goblog
cd goblog

hugo new site quickstart

cd quickstart
dir
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke themes/ananke
```

Add line to end of file `config.toml`

```bash
hugo serve
hugo new posts/my-first-post.md
```

```toml
theme = 'ananke'
```

![build_production_website.png](/img/vy_hugo/build_production_website.png)

![goblog_ide.png](/img/vy_hugo/goblog_ide.png)

![hugo.png](/img/vy_hugo/hugo.png)

Command

```bash
hugo serve
```

![hugo_serve.png](/img/vy_hugo/hugo_serve.png)

![hugo_version.png](/img/vy_hugo/hugo_version.png)

![result_of_build.png](/img/vy_hugo/result_of_build.png)

See result in web browser at localhost

![site_live.png](/img/vy_hugo/site_live.png)

First post

![hugo_first.png](/img/vy_hugo/hugo_first.png)

Add first post

![first_post.png](/img/vy_hugo/first_post.png)

Inside IDE

![goblog_ide.png](/img/vy_hugo/goblog_ide.png)

Fast render mode

![fast_redner.png](/img/vy_hugo/fast_redner.png)

When update website, re-build, you must delete directory/folder `public` manually.

Change `draft: true` to `draf: false`

```yaml
---
title: "Create a static blog by Hugo"
date: 2023-04-26T10:29:47+07:00
draft: false
---
```

```bash
hugo --cleanDestinationDir
```

Config DNS

![dns.png](/img/vy_hugo/dns.png)

Result: https://golangnow.com for you.