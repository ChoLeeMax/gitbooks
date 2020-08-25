<!--
 * @Descripttion:
 * @version:
 * @Author: cholee
 * @Date: 2020-08-21 16:59:06
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 14:23:01
-->

它提供了一些配置项可以改变 DevServer 的默认行为。 要配置 DevServer ，除了在配置文件里通过 devServer 传入参数外，还可以通过命令行参数传入。
**注意**只有在通过 DevServer 去启动 Webpack 时配置文件里 devServer 才会生效，因为这些参数所对应的功能都是 DevServer 提供的，Webpack 本身并不认识 devServer 配置项。

```js
    //开发中的server
    devServer: {
      hot: true, //配置是否启用模块的热替换功能
      open: true, //是否启用默认浏览器打开网页
      port: cfg.dev.port, //指定要监听请求的端口号
      host: "localhost", //指定主机,可以是ip
      openPage: "/", //指定打开浏览器时导航到的页面
      proxy: cfg.dev.proxy, //代理设置，处理本地跨域
      overlay: {//是否提示编译出错。默认false，可配置显示警告和错误信息
        warnings: true,
        errors: true,
      },
      stats: "", //控制编译的时候shell上输出的内容，不配置答应全部信息.errors-only只打印错误信息。详见http://www.mamicode.com/info-detail-2709301.html,
        headers: {//devServer.headers配置项可以在 HTTP 响应中注入一些 HTTP 响应头
    'X-foo':'bar'
  }
    },
```
