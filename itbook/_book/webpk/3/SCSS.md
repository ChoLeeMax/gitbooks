<!--
 * @Descripttion:
 * @version:
 * @Author: cholee
 * @Date: 2020-08-25 14:34:05
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 14:39:11
-->

> 问题一：SCSS 的优点？

方便地管理代码，抽离公共的部分，通过逻辑写出更灵活的代码

> 问题二：如何接入 SCSS？

最适合的方式是使用 Loader，Webpack 官方提供了对应的 sass-loader。

Webpack 接入 sass-loader 相关配置如下：

```js
module.exports = {
  module: {
    rules: [
      {
        // 增加对 SCSS 文件的支持
        test: /\.scss\$/,
        // SCSS 文件的处理顺序为先 sass-loader 再 css-loader 再 style-loader
        use: ["style-loader", "css-loader", "sass-loader"],
      },
    ],
  },
};
```

> 问题三：webpack 处理 SCSS 流程？

1、通过 sass-loader 把 SCSS 源码转换为 CSS 代码，再把 CSS 代码交给 css-loader 去处理。
2、css-loader 会找出 CSS 代码中的@import 和 url()这样的导入语句，告诉 Webpack 依赖这些资源。同时还支持 CSS Modules、压缩 CSS 等功能。处理完后再把结果交给 style-loader 去处理。
3、style-loader 会把 CSS 代码转换成字符串后，注入到 JavaScript 代码中去，通过 JavaScript 去给 DOM 增加样式。如果你想把 CSS 代码提取到一个单独的文件而不是和 JavaScript 混在一起，可以使用 ExtractTextPlugin。
