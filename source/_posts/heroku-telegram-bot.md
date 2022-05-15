---
title: 在heroku上使用webhook上部署telegram bot
date: 2022-05-15 00:31:10
cover: https://raw.githubusercontent.com/konglinghu777/images/main/20220515011152.png
tags: 
    - telegram
    - nodejs
---

## 前言
本文只是基础实现使用`webhook`消息处理方式开发telegram bot并且部署在`heroku`平台上

## 开发准备
- 开发语言: {% label nodejs green %}
- bot-api框架: {% label telegraf green %}
- 部署平台: {% label heroku green %}

## 常见问题

### 1.heroku的`configVar`与nodejs的`.env`处理
使用`heroku CLI`设置`configVar`
```shell
heroku config:set CONFIG_NAME=CONFIG_VALUE
```

![heroku config](https://raw.githubusercontent.com/konglinghu777/images/main/20220515005658.png)

比如把{% label BOT_TOKEN %}设置在`config var`内,但是当本地node项目测试时需要从`.env`文件中读取变量,可以通过已下命令把heroku的`config var`写入到`.env`文件
```shell
heroku config -s >> .env
```

### 2.heroku web应用端口的问题

`heroku` 启动 {% label web blue %} 应用时会随机生成一个`端口值`记录在 {% label process.env.PORT green %} 内,所以在使用`telegraf`的api时,在`webhook`的配置项内有端口值需要根据`dev/prod`的环境来设置
```JavaScript
bot.launch({
    webhook: {
        domain:
            process.env.NODE_ENV === 'production'
                ? 'https://xxxxxxx.herokuapp.com/'
                : '个人使用本地localtunnel通道用于本地启动',
        port: process.env.PORT || 6342,
    },
});
```



