<!--
 * @Descripttion: 如何接入Angular2 框架
 * @version:
 * @Author: cholee
 * @Date: 2020-08-25 14:34:06
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 14:43:34
-->

> 问题一：如何接入 Angular2 框架？

由于 Angular2 项目中采用了注解的语法，而且@angular/platform-browser 源码中有许多 DOM 操作，配置需要修改为如下：

```js
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "sourceMap": true,
    // 开启对 注解 的支持
    "experimentalDecorators": true,
    // Angular2 依赖新的 JavaScript API 和 DOM 操作
    "lib": [
      "es2015",
      "dom"
    ]
  },
  "exclude": [
    "node_modules/*"
  ]
}
```
