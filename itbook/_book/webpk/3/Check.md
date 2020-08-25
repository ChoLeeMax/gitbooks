<!--
 * @Descripttion: 检查代码
 * @version:
 * @Author: cholee
 * @Date: 2020-08-25 14:34:06
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 14:52:17
-->

> 问题一：代码检查具体是做什么？

检查代码主要检查以下几项：

- 代码风格：让项目成员强制遵守统一的代码风格，例如如何缩进、如何写注释等，保障代码可读性，不把时间浪费在争论如何写代码更好看上；
- 潜在问题：分析出代码在运行过程中可能出现的潜在 Bug。

> 问题二：怎么做代码检查？

在做代码风格检查时需要按照不同的文件类型来检查，下面来分别介绍。

## 检查 JavaScript

目前最常用的 JavaScript 检查工具是 ESlint，它不仅内置了大量常用的检查规则，还可以通过插件机制做到灵活扩展。

**结合 Webpack**
eslint-loader 可以方便的把 ESLint 整合到 Webpack 中，使用方法如下：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        // node_modules 目录的下的代码不用检查
        exclude: /node_modules/,
        loader: "eslint-loader",
        // 把 eslint-loader 的执行顺序放到最前面，防止其它 Loader 把处理后的代码交给 eslint-loader 去检查
        enforce: "pre",
      },
    ],
  },
};
```

接入 eslint-loader 后就能在控制台中看到 ESLint 输出的错误日志了。

## 检查 TypeScript

TSLint 是一个和 ESlint 相似的 TypeScript 代码检查工具，区别在于 TSLint 只专注于检查 TypeScript 代码

**结合 Webpack**
tslint-loader 是一个和 eslint-loader 相似的 Webpack Loader， 能方便的把 TSLint 整合到 Webpack，其使用方法如下：

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.ts$/,
        // node_modules 目录的下的代码不用检查
        exclude: /node_modules/,
        loader: "tslint-loader",
        // 把 tslint-loader 的执行顺序放到最前面，防止其它 Loader 把处理后的代码交给 tslint-loader 去检查
        enforce: "pre",
      },
    ],
  },
};
```

## 检查 CSS

stylelint 是目前最成熟的 CSS 检查工具，内置了大量检查规则的同时也提供插件机制让用户自定义扩展。 stylelint 基于 PostCSS，能检查任何 PostCSS 能解析的代码，诸如 SCSS、Less 等。

**结合 Webpack**
StyleLintPlugin 能把 stylelint 整合到 Webpack，其使用方法很简单，如下：

```js
const StyleLintPlugin = require("stylelint-webpack-plugin");

module.exports = {
  // ...
  plugins: [new StyleLintPlugin()],
};
```

> 问题三：代码检查功能整合到 Webpack 中导致的问题和解决方法？

把代码检查功能整合到 Webpack 中会导致以下问题：

- 由于执行检查步骤计算量大，整合到 Webpack 中会导致构建变慢；
- 在整合代码检查到 Webpack 后，输出的错误信息是通过行号来定位错误的，没有编辑器集成显示错误直观；
  为了避免以上问题，还可以这样做：

- 使用集成了代码检查功能的编辑器，让编辑器实时直观地显示错误；
- 把代码检查步骤放到代码提交时，也就是说在代码提交前去调用以上检查工具去检查代码，只有在检查都通过时才提交代码，这样就能保证提交到仓库的代码都是通过了检查的。
  如果你的项目是使用 Git 管理，Git 提供了 Hook 功能能做到在提交代码前触发执行脚本。
