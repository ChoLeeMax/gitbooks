<!--
 * @Descripttion: webpack章节
 * @version: 1
 * @Author: cholee
 * @Date: 2020-08-17 17:32:44
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-21 17:28:56
-->
# modules

> 问题一：什么是模块化？

```
模块化是指把一个复杂的系统分解到多个模块以方便编码。
```

> 问题二：为什么出现模块化

很久以前，开发网页要通过命名空间的方式来组织代码，例如 jQuery 库把它的 API 都放在了 window.$下，在加载完 jQuery 后其他模块再通过window.$去使用 jQuery。  
这样做有很多问题，其中包括：

- 命名空间冲突，两个库可能会使用同一个名称，例如 Zepto 也被放在 window.\$下；
- 无法合理地管理项目的依赖和版本；
- 无法方便地控制依赖的加载顺序。
  当项目变大时这种方式将变得难以维护，需要用模块化的思想来组织代码。

# keyconcept

> 问题一：Webpack 的几个核心概念

- **Entry**：入口，Webpack 执行构建的第一步将从 Entry 开始，可抽象成输入 -
- **Module**：模块，在 Webpack 里一切皆模块，一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块 -
- **Chunk**：代码块，一个 Chunk 由多个模块组合而成，用于代码合并与分割
- **Loader**：模块转换器，用于把模块原内容按照需求转换成新内容。
- **Plugin**：扩展插件，在 Webpack 构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要的事情 -
- **Output**：输出结果，在 Webpack 经过一系列处理并得出最终想要的代码后输出结果。

# construct

> 问题一：构建的作用及常见功能是什么？


构建就是当源代码无法直接运行时，通过转化将源代码转换成可执行的 JavaScript、Css、HTML代码。


##### 一般包括如下内容：

- **代码转换**：TypeScript 编译成 JavaScript、SCSS 编译成 CSS 等。
- **文件优化**：压缩 JavaScript、CSS、HTML 代码，压缩合并图片等。
- **代码分割**：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载。
- **模块合并**：在采用模块化的项目里会有很多个模块和文件，需要构建功能把模块分类合并成一个文件。
- **自动刷新**：监听本地源代码的变化，自动重新构建、刷新浏览器。
- **代码校验**：在代码被提交到仓库前需要校验代码是否符合规范，以及单元测试是否通过。
- **自动发布**：更新完代码后，自动构建出线上发布代码并传输给发布系统。

# loader

> 问题一：Loader 机制的作用是什么？

  Loader 可以看作具有文件转换功能的翻译员，配置里的 module.rules 数组配置了一组规则，告诉 Webpack 在遇到哪些文件时使用哪些 Loader 去加载和转换。

> 问题二：css-loader 与 style-loader 的作用

  css-loader 读取 CSS 文件  
  style-loader 把 CSS 内容注入到 JavaScript 里

> 问题三：配置 Loader 时需要注意的地方？

- use 属性的值需要是一个由 Loader 名称组成的数组，Loader 的执行顺序是由后到前的；
- 每一个 Loader 都可以通过 URL querystring 的方式传入参数，例如 css-loader?minimize 中的 minimize 告诉 css-loader 要开启 CSS 压缩。

# plugin

> 问题一：Plugin（插件）的作用是什么

Plugin 是用来扩展 Webpack 功能的，通过在构建流程里注入钩子实现，它给 Webpack 带来了很大的灵活性。  
Webpack 是通过plugins属性来配置需要使用的插件列表的。plugins属性是一个数组，里面的每一项都是插件的一个实例，在实例化一个组件时可以通过构造函数传入这个组件支持的配置属性。

> 问题二：ExtractTextPlugin插件的作用

ExtractTextPlugin插件的作用是提取出 JavaScript 代码里的 CSS 到一个单独的文件。
对此你可以通过插件的filename属性，告诉插件输出的 CSS 文件名称是通过[name]_[contenthash:8].css字符串模版生成的，里面的[name]代表文件名称，[contenthash:8]代表根据文件内容算出的8位 hash 值， 还有很多配置选项可以在ExtractTextPlugin的主页上查到。

# devserver  

> 问题一：DevServer开发工具

DevServer 会启动一个 HTTP 服务器用于服务网页请求，同时会帮助启动 Webpack ，并接收 Webpack 发出的文件更变信号，通过 WebSocket 协议自动刷新网页做到实时预览。  
安装DevServer

```js
npm i -D webpack-dev-server
```
> 问题二：实时预览

Webpack 在启动时可以开启监听模式，开启监听模式后 Webpack 会监听本地文件系统的变化，发生变化时重新构建出新的结果。Webpack 默认是关闭监听模式的，你可以在启动 Webpack 时通过webpack --watch来开启监听模式。
通过 DevServer 启动的 Webpack 会开启监听模式，当发生变化时重新执行完构建后通知 DevServer。 DevServer 会让 Webpack 在构建出的 JavaScript 代码里注入一个代理客户端用于控制网页，网页和 DevServer 之间通过 WebSocket 协议通信， 以方便 DevServer 主动向客户端发送命令。 DevServer 在收到来自** Webpack 的文件变化**通知时通过注入的客户端控制网页刷新。

> 问题三：什么是模块热替换？

模块热替换能做到在不重新加载整个网页的情况下，通过将被更新过的模块替换老的模块，再重新执行一次来实现实时预览。  
模块热替换相对于默认的刷新机制能提供更快的响应和更好的开发体验。 模块热替换默认是关闭的，要开启模块热替换，你只需在启动 DevServer 时带上--hot参数，重启 DevServer 后再去更新文件就能体验到模块热替换的神奇了。

> 问题四：什么是Source Map 及其使用

Source Map能够提供将压缩文件恢复到源文件原始位置的映射代码的方式。这意味着你可以在优化压缩代码后轻松的进行调试。在编译器输出的代码上进行断点调试是一件辛苦和不优雅的事情， 调试工具可以通过Source Map映射代码，让你在源代码上断点调试。

Source Map使用：Webpack 支持生成 Source Map，只需在启动时带上--devtool source-map参数。 加上参数重启 DevServer 后刷新页面，再打开 Chrome 浏览器的开发者工具，就可在 Sources 栏中看到可调试的源代码了。
