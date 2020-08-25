<!--
 * @Descripttion: 使用自动刷新
 * @version:
 * @Author: cholee
 * @Date: 2020-08-25 16:11:34
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 16:44:21
-->

> 问题一：如何配置实现文件监听？

Webpack 支持文件监听相关的配置项如下：

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
    // 监听到变化发生后会等 300ms 再去执行动作，防止文件更新太快导致重新编译频率太高
    // 默认为 300ms
    aggregateTimeout: 300,
    // 判断文件是否发生变化是通过不停的去询问系统指定文件有没有变化实现的
    // 默认每隔 1000 毫秒询问一次
    poll: 1000,
  },
};
```

> 问题二：要让 Webpack 开启监听模式，有几种方式：？

两种

1.在配置文件 webpack.config.js 中设置 watch: true。  
2.在执行启动 Webpack 命令时，带上--watch 参数，完整命令是 webpack --watch。
