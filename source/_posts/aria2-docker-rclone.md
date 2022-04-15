---
title: 使用docker部署aria2离线下载,自动上传rclone
date: 2022-04-15 00:22:28
tags: 
    - docker
cover: https://raw.githubusercontent.com/konglinghu777/images/main/20220415005114.png
---

{% note success no-icon simple %}
参考自[p3terx](https://p3terx.com/archives/docker-aria2-pro.html)的博客
{% endnote %}

## 部署docker-aria2

```bash
docker run -d \
    --name aria2-pro \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=<改成你自己的TOKEN> \
    -e RPC_PORT=6800 \
    -e LISTEN_PORT=6888 \
    -v ~/aria2-config:/config \
    -v ~/rclone-downloads:/downloads \
    -e SPECIAL_MODE=rclone \
    p3terx/aria2-pro
```
1.然后把`rclone.conf`复制到`aria2-config`目录下
2.修改`aria2-config`目录下的`script.conf`的`drive-name`与`drive-id`

## 部署docker-aria2ng 前端面板

```bash
docker run -d \
    --name ariang \
    --restart unless-stopped \
    --log-opt max-size=1m \
    -p 80:6880 \
    p3terx/ariang
```