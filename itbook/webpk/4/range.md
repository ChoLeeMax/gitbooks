<!--
 * @Descripttion:
 * @version:
 * @Author: cholee
 * @Date: 2020-08-25 16:11:29
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 16:41:54
-->

> 问题一：为什么要缩小文件搜索范围？

Webpack 启动后会从配置的 Entry 出发，解析出文件中的导入语句，再递归的解析。 在遇到导入语句时 Webpack 会做两件事情：

1、根据导入语句去寻找对应的要导入的文件。例如 require('react')导入语句对应的文件是./node_modules/react/react.js，require('./util')对应的文件是./util.js。  
2、根据找到的要导入文件的后缀，使用配置中的 Loader 去处理文件。例如使用 ES6 开发的 JavaScript 文件需要使用 babel-loader 去处理。

以上两件事情虽然对于处理一个文件非常快，但是当项目大了以后文件量会变的非常多，这时候构建速度慢的问题就会暴露出来。 虽然以上两件事情无法避免，但需要尽量减少以上两件事情的发生，以提高速度。

> 问题二：优化缩小文件的搜索范围的途径有哪些？

- 优化 loader 配置
- 优化 resolve.modules 配置
- 优化 resolve.mainFields 配置
- 优化 resolve.alias 配置
- 优化 resolve.extensions 配置

# loader 配置

```js
module.exports = {
  module: {
    rules: [
      {
        // 如果项目源码中只有 js 文件就不要写成 /\.jsx?$/，提升正则表达式性能
        test: /\.js$/,
        // babel-loader 支持缓存转换出的结果，通过 cacheDirectory 选项开启
        use: ["babel-loader?cacheDirectory"],
        // 只对项目根目录下的 src 目录中的文件采用 babel-loader
        include: path.resolve(__dirname, "src"),
      },
    ],
  },
};
```

# resolve.modules 配置

```js
module.exports = {
  resolve: {
    // 使用绝对路径指明第三方模块存放的位置，以减少搜索步骤
    // 其中 __dirname 表示当前工作目录，也就是项目根目录
    modules: [path.resolve(__dirname, "node_modules")],
  },
};
```

# resolve.mainFields 配置

```js
module.exports = {
  resolve: {
    // 只采用 main 字段作为入口文件描述字段，以减少搜索步骤
    mainFields: ["main"],
  },
};
```

# resolve.alias 配置

```js
    resolve: {
      //导入的时候不用写拓展名
      extensions: [".js", ".vue", ".json", ".ts"], //import Button from '@/components/Button'
      //通过别名把原导入路径映射成一个新的导入路径
      alias: {
        //通过配置resolve.alias可以让 Webpack 在处理 vue 库时，直接使用单独完整的vue.esm.js文件，从而跳过耗时的递归解析操作
        vue$: "vue/dist/vue.esm.js", //精准匹配
        "@": resolve("src"), //import Button from '../../components/Button' => import Button from '@/components/Button'
        // components: './src/components/',//import Button from './src/components/button' => import Button from 'components/button'
      },
    },
```

# resolve.extensions 配置

配置 resolve.extensions 时你需要遵守以下几点，以做到尽可能的优化构建性能：

- 后缀尝试列表要尽可能的小，不要把项目中不可能存在的情况写到后缀尝试列表中。
- 频率出现最高的文件后缀要优先放在最前面，以做到尽快的退出寻找过程。
- 在源码中写导入语句时，要尽可能的带上后缀，从而可以避免寻找过程。例如在你确定的情况下把 require('./data')写成 require('./data.json')。

```js
module.exports = {
  resolve: {
    // 尽可能的减少后缀尝试的可能性
    extensions: ["js"],
  },
};
```
