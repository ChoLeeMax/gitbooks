<!--
 * @Descripttion: 其他配置
 * @version:
 * @Author: cholee
 * @Date: 2020-08-25 14:05:49
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 14:12:36
-->

[Target 配置](Module.md#Target配置)  
[devtool 配置](Module.md#devtool配置)  
[Watch 配置](Module.md#Watch配置)  
[Externals 配置](Module.md#Externals配置)  
[ResolveLoader 配置](Module.md#ResolveLoader配置)

# Target 配置

javaScript 的应用场景越来越多，从浏览器到 Node.js，这些运行在不同环境的 JavaScript 代码存在一些差异。target 配置项可以让 Webpack 构建出针对不同运行环境的代码。target 可以是以下之一：

| target 值         | 描述                                             |
| :---------------- | :----------------------------------------------- |
| web               | 针对浏览器**(默认)**，所有代码都集中在一个文件里 |
| node              | 针对 Node.js，使用 require 语句加载 Chunk 代码   |
| async-node        | 针对 Node.js，异步加载 Chunk 代码                |
| webworker         | 针对 WebWorker                                   |
| electron-main     | 针对 Electron 主线程                             |
| electron-renderer | 针对 Electron 渲染线程                           |

例如当你设置 target:'node'时，源代码中导入 Node.js 原生模块的语句 require('fs')将会被保留，fs 模块的内容不会打包进 Chunk 里。

# devtool 配置

devtool 配置 Webpack 如何生成 Source Map，默认值是 false 即不生成 Source Map，想为构建出的代码生成 Source Map 以方便调试，可以这样配置：

```js
module.export = {
  devtool: "source-map",
};
```

# Watch 配置

前面介绍过 Webpack 的监听模式，它支持监听文件更新，在文件发生变化时重新编译。在使用 Webpack 时监听模式默认是关闭的，想打开需要如下配置：

```js
module.export = {
  watch: true,
};
```

在使用 DevServer 时，监听模式默认是开启的。
除此之外，Webpack 还提供了 watchOptions 配置项去更灵活的控制监听模式，使用如下：

```js
module.export = {
  // 只有在开启监听模式时，watchOptions 才有意义
  // 默认为 false，也就是不开启
  watch: true,
  // 监听模式运行时的参数
  // 在开启监听模式时，才有意义
  watchOptions: {
    // 不监听的文件或文件夹，支持正则匹配
    // 默认为空
    ignored: /node_modules/,
    // 监听到变化发生后会等300ms再去执行动作，防止文件更新太快导致重新编译频率太高
    // 默认为 300ms
    aggregateTimeout: 300,
    // 判断文件是否发生变化是通过不停的去询问系统指定文件有没有变化实现的
    // 默认每隔1000毫秒询问一次
    poll: 1000,
  },
};
```

# Externals 配置

Externals 用来告诉 Webpack 要构建的代码中使用了哪些不用被打包的模块，也就是说这些模版是外部环境提供的，Webpack 在打包时可以忽略它们。

有些 JavaScript 运行环境可能内置了一些全局变量或者模块，例如在你的 HTML HEAD 标签里通过以下代码：

```js
<script src="path/to/jquery.js"></script>
```

引入 jQuery 后，全局变量 jQuery 就会被注入到网页的 JavaScript 运行环境里。

如果想在使用模块化的源代码里导入和使用 jQuery，可能需要这样：

```js
import $ from "jquery";
$(".my-element");
```

构建后你会发现输出的 Chunk 里包含的 jQuery 库的内容，这导致 jQuery 库出现了 2 次，浪费加载流量，最好是 Chunk 里不会包含 jQuery 库的内容。

Externals 配置项就是为了解决这个问题。

通过 externals 可以告诉 Webpack JavaScript 运行环境已经内置了那些全局变量，针对这些全局变量不用打包进代码中而是直接使用全局变量。 要解决以上问题，可以这样配置 externals：

```js
module.export = {
  externals: {
    // 把导入语句里的 jquery 替换成运行环境里的全局变量 jQuery
    jquery: "jQuery",
  },
};
```

# ResolveLoader 配置

ResolveLoader 用来告诉 Webpack 如何去寻找 Loader，因为在使用 Loader 时是通过其包名称去引用的， Webpack 需要根据配置的 Loader 包名去找到 Loader 的实际代码，以调用 Loader 去处理源文件。

ResolveLoader 的默认配置如下：

```js
module.exports = {
  resolveLoader: {
    // 去哪个目录下寻找 Loader
    modules: ["node_modules"],
    // 入口文件的后缀
    extensions: [".js", ".json"],
    // 指明入口文件位置的字段
    mainFields: ["loader", "main"],
  },
};
```

该配置项常用于加载本地的 Loader。
