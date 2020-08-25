<!--
 * @Descripttion:  如何接入ES6
 * @version:
 * @Author: cholee
 * @Date: 2020-08-25 14:33:56
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 14:36:55
-->

> 问题一：为什么 ES6 要转换为 ES5？

虽然目前部分浏览器和 Node.js 已经支持 ES6，但由于它们对 ES6 所有的标准支持不全，这导致在开发中不敢全面地使用 ES6。

> 问题二：Babel 有什么作用？

Babel 是一个 JavaScript 编译器，能将 ES6 代码转为 ES5 代码，让你使用最新的语言特性而不用担心兼容性问题，并且可以通过插件机制根据需求灵活的扩展。

> 问题三：Babel 有什么属性以及作用？

1、plugins 属性告诉 Babel 要使用哪些插件，插件可以控制如何转换代码。
2、presets 属性告诉 Babel 要转换的源码使用了哪些新的语法特性，一个 Presets 对一组新语法特性提供支持，多个 Presets 可以叠加。

> 问题四：如何接入 Babel？

由于 Babel 所做的事情是转换代码，所以应该通过 Loader 去接入 Babel，Webpack 配置如下：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js\$/,
        use: ["babel-loader"],
      },
    ],
  },
  // 输出 source-map 方便直接调试 ES6 源码
  devtool: "source-map",
};
```

配置命中了项目目录下所有的 JavaScript 文件，通过 babel-loader 去调用 Babel 完成转换工作。 在重新执行构建前，需要先安装新引入的依赖：

# Webpack 接入 Babel 必须依赖的模块

```js
npm i -D babel-core babel-loader
```

# 根据你的需求选择不同的 Plugins 或 Presets

```js
npm i -D babel-preset-env
```
