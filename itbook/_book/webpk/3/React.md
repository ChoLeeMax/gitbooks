<!--
 * @Descripttion: 接入 React
 * @version:
 * @Author: cholee
 * @Date: 2020-08-25 14:34:06
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 14:54:54
-->

> 要在使用 Babel 的项目中接入 React 框架是很简单的，只需要加入 React 所依赖的 Presets babel-preset-react。  

通过以下命令：

```js
# 安装 React 基础依赖
npm i -D react react-dom
# 安装 babel 完成语法转换所需依赖
npm i -D babel-preset-react
```

安装新的依赖后，再修改.babelrc 配置文件加入 React Presets

```js
"presets": [
    "react"
],
```

就完成了一切准备工作。

再修改 main.js 文件如下：

```js
import * as React from "react";
import { Component } from "react";
import { render } from "react-dom";

class Button extends Component {
  render() {
    return <h1>Hello,Webpack</h1>;
  }
}

render(<Button />, window.document.getElementById("app"));
```

重新执行构建打开网页你将会发现由 React 渲染出来的 Hello,Webpack
