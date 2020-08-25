<!--
 * @Descripttion:Vue的使用
 * @version:
 * @Author: cholee
 * @Date: 2020-08-25 14:34:06
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 14:44:11
-->

> 问题一：如何接入 Vue 框架？

目前最成熟和流行的开发 Vue 项目的方式是采用 ES6 加 Babel 转换 Vue 官方提供了对应的 vue-loader 可以非常方便的完成单文件组件的转换。

修改 Webpack 相关配置如下：

```js
module: {
  rules: [
    {
      test: /\.vue\$/,
      use: ["vue-loader"],
    },
  ];
}
```

安装新引入的依赖：

```js
# Vue 框架运行需要的库
npm i -S vue
# 构建所需的依赖
npm i -D vue-loader css-loader vue-template-compiler
```

> 问题二：所装依赖的作用？

- vue-loader：解析和转换.vue 文件，提取出其中的逻辑代码 script、样式代码 style、以及 HTML 模版 template，再分别把它们交给对应的 Loader 去处理。
- css-loader：加载由 vue-loader 提取出的 CSS 代码。
- vue-template-compiler：把 vue-loader 提取出的 HTML 模版编译成对应的可执行的 JavaScript 代码，这和 React 中的 JSX 语法被编译成 JavaScript 代码类似。预先编译好 HTML 模版相对于在浏览器中再去编译 HTML 模版的好处在于性能更好。
