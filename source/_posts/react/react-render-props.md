---
title: React 使用render props共享代码(官方文档的说法)
date: 2022-04-04 00:48:45
tags:
    - react
    - 前端
---

## 介绍
在react官方文档的高级指引中有一页[`Render Props`](https://zh-hans.reactjs.org/docs/render-props.html)的文档,作为一个react初学者对这个概念浅薄的理解有点像vue中
常用的对组件使用props传入一个{% label 回调函数 blue%},则可以通过回调函数在组件外部拿到组件内部的数据的一个操作

## 例子

### Mouse.tsx文件如下
```tsx File:Mouse.tsx
import { useState } from 'react';

function Mouse(props) {
  const [position, setPosition] = useState({
    x: 0,
    y: 0,
  });

  const handleMouseMove = (e) => {
    setPosition({
      x: e.clientX,
      y: e.clientY,
    });
  };

  const { render } = props;
  return (
    <div className="mouse" onMouseMove={handleMouseMove}>
      <div className="mouse-position">
        {position.x},{position.y}
      </div>
      {render(position)}
    </div>
  );
}
export default Mouse;

```

### Cat.tsx文件如下
```tsx File: Cat.tsx
import React, { Fragment } from 'react';
function Cat(props) {
  const { mouse } = props;

  return (
    <Fragment>
      <div
        className="cat-box"
        style={{
          position: 'absolute',
          left: mouse.x,
          top: mouse.y,
        }}
      ></div>
    </Fragment>
  );
}
export default Cat;

```

### App.tsx调用代码

关键代码在第五行,render是一个函数传给了Mouse,在Mouse内部调用了这个render
```tsx File:App.tsx

function App() {
  return (
    <div className="App">
      <Mouse render={(mouse: any) => <Cat mouse={mouse} />}></Mouse>
    </div>
  );
}

```