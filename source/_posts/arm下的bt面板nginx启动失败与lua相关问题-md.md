---
title: arm下的bt面板nginx启动失败与lua相关问题
date: 2022-07-17 23:05:35
tags: 
    - 运维
    - nginx
---

## 报错原因

**ARM架构下安装Nginx并增加对LuaJIT的支持**
宝塔面板的 nginx 编译脚本目前是直接忽略 ARM 对 LuaJIT 的支持，这导致了许多依赖 lua 语言的插件失效，比如 Nginx 防火墙、网站监控报表。

```shell
cat>/www/server/panel/install/nginx_prepare.sh<<EOL

#!/bin/bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
 
wget -c -O LuaJIT-2.1.zip https://github.com/LuaJIT/LuaJIT/archive/refs/heads/v2.1.zip -T 10
unzip LuaJIT-2.1.zip
if [ -e LuaJIT-2.1 ]; then
    cd LuaJIT-2.1
    make linux
    make install
    export LUAJIT_LIB=/usr/local/lib
    export LUAJIT_INC=/usr/local/include/luajit-2.1/
    ln -sf /usr/local/lib/libluajit-5.1.so.2 /usr/local/lib64/libluajit-5.1.so.2
    if [ `grep -c /usr/local/lib /etc/ld.so.conf` -eq 0 ]; then
        echo "/usr/local/lib" >> /etc/ld.so.conf
    fi
    ldconfig
    cd ..
fi
rm -rf LuaJIT-2.1*
Install_cjson
EOL
```

```shell
sed -i 's/\r//g' /www/server/panel/install/nginx_prepare.sh
```

```shell
cat>/www/server/panel/install/nginx_configure.pl<<EOL
--add-module=/www/server/nginx/src/ngx_devel_kit --add-module=/www/server/nginx/src/lua_nginx_module
EOL
```

```shell
# Debian/Ubuntu
# 手动安装lua5，防止在安装nginx后加载lua引擎出错
apt install lua5* -y
```

最后面板升级一下 nginx 或者终端执行以下命令，安装或升级现有Nginx。
其中 1.20 需要修改成您现在在用的 nginx 版本。
**Ubuntu 第一次安装 Nginx 务必使用编译安装（或使用下面的安装命令），极速安装不会调用自定义脚本！**

```shell
# 来源：https://www.xeath.cc/2021/08/07/archives-481/
# 未安装Nginx，直接安装
cd /www/server/panel/install && bash install_soft.sh 0 install nginx 1.20
 
# 已安装Nginx，升级
cd /www/server/panel/install && bash install_soft.sh 0 update nginx 1.20
```



参考自:

[甲骨文ARM架构下安装宝塔面板及防火墙 - 让宝塔面板的 Nginx 在 ARM 下也能支持 LuaJIT](https://www.emengweb.com/p/ARM%E6%9E%B6%E6%9E%84%E4%B8%8B%E5%AE%89%E8%A3%85%E5%AE%9D%E5%A1%94%E9%9D%A2%E6%9D%BF%E5%8F%8A%E9%98%B2%E7%81%AB%E5%A2%99-%E8%AE%A9%E5%AE%9D%E5%A1%94%E9%9D%A2%E6%9D%BF%E7%9A%84-Nginx-%E5%9C%A8-ARM-%E4%B8%8B%E4%B9%9F%E8%83%BD%E6%94%AF%E6%8C%81-LuaJIT)

[nginx: [emerg] unknown directive "lua_shared_dict" in /www/server/panel/vhost/nginx/total.conf:1](https://hostloc.com/forum.php?mod=viewthread&tid=927779&page=1#pid11420831)

[让宝塔面板的 Nginx 在 ARM 下也能支持 LuaJIT](https://www.bt.cn/bbs/forum.php?mod=viewthread&tid=73777)