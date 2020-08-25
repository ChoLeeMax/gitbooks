<!--
 * @Descripttion:
 * @version:
 * @Author: cholee
 * @Date: 2020-08-25 16:11:34
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 16:50:38
-->

> 问题一：模块热替换原理

原理是当一个源码发生变化时，只重新编译发生变化的模块，再用新输出的模块替换掉浏览器中对应的老模块。

> 问题二：模块热替换技术的优势有

实时预览反应更快，等待时间更短。
不刷新浏览器能保留当前网页的运行状态，例如在使用 Redux 来管理数据的应用中搭配模块热替换能做到代码更新时 Redux 中的数据还保持不变。

> 问题四：开启模块热替换模式

````js

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
      stats: "", //控制编译的时候shell上输出的内容，不配置答应全部信息.errors-only只打印错误信息。详见http://www.mamicode.com/info-detail-2709301.html
    },
    ```
````
