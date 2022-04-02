---
title: Hexo CloudFlare Pages Error
date: 2022-04-02 21:53:22
tags: 
    - Hexo
    - Cloudflare
cover: https://s2.loli.net/2022/04/02/jwmZ8yJg5nHR4aX.png
---

# Hexo 部署在Cloudflare Pages时遇到
{% note red 'fa-solid fa-bug' modern%}
报错信息: error occurred while updating repo submodules
{% endnote %}

## 问题描述
由于用了[Hexo Next](https://github.com/theme-next/hexo-theme-next.git)主题,并没有使用`npm install`安装主题,而是使用如下安装方式,在cloudflare pages部署时遇到了问题
```bash
$ cd hexo
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
```

## 解决方案:
在项目根目录添加`.gitmodules`文件,写入Next主题git的相关信息
```
[submodule "next"]
path = themes/next
url = git@github.com:theme-next/hexo-theme-next.git
```

方案来源:[Pages build error: Failed: error occurred while updating repo submodules
](https://community.cloudflare.com/t/pages-build-error-failed-error-occurred-while-updating-repo-submodules/356890)