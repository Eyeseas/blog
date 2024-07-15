---
title: antd与tailwindcss样式发生冲突
date: 2022-07-15 22:22:28
tags:
    - 前端
    - react
    - css
---

个人觉得更优的解决方案，**不需要禁止tailwindcss的默认属性**，仅供参考[antd按需引入解决tailwindcss冲突](/react/antd按需引入css(less)/)


## 问题描述
项目使用vite构建，模板为react-ts，安装antd与tailwindcss后，按照顺序引入css后发现antd的按钮背景变成透明了
```css
// index.css

@import 'antd/dist/antd.css';
@tailwind base;
@tailwind components;
@tailwind utilities;

```

## 原因

通过查看css发现，造成这个问题的原因是：`@tailwind base`内的基础样式，覆盖掉了`antd.css`的部分样式

## 解决方案

**禁止tailwindcss的默认属性,添加corePlugins对象，并设置preflight为false**

```javascript
module.exports = {
  
  corePlugins: {
    preflight: false
  }
}
```

参考：
[使用tailwindcss与antd冲突，Button按钮透明](https://segmentfault.com/a/1190000041781474)