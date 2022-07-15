---
title: antd按需引入样式
date: 2022-07-15 23:22:28
tags:
    - react
    - css
    - antd
cover: https://raw.githubusercontent.com/konglinghu777/images/main/20220715234155.png
---

{% note success no-icon simple %}
通过该方法按需引入antd样式，也可以解决[antd与tailwindcss样式冲突](/react/antd与tailwindcss样式冲突/)的问题
{% endnote %}


## 解决方案

使用[vite-plugin-imp](https://www.npmjs.com/package/vite-plugin-imp)，对antd的样式文件进行按需引入，思路来自[github issue](https://github.com/vitejs/vite/issues/1389#issuecomment-762128018)

```typescript
// vite.config.ts
// 需要安装less， antd的使用的less
export default defineConfig({
  plugins: [
    vitePluginImp({
      libList: [
        {
          libName: "antd",
          style: (name) => `antd/lib/${name}/style/index.css`,
        },
      ],
    }),
  ],
});
```