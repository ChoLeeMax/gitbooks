<!--
 * @Descripttion: resolve配置
 * @version:
 * @Author: cholee
 * @Date: 2020-08-21 16:59:06
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-25 14:21:41
-->

[alias 配置](Resolve.md#alias配置)  
[mainFields 配置](Resolve.md#mainFields配置)  
[extensions 配置](Resolve.md#extensions配置)  
[modules 配置](Resolve.md#modules配置)

# alias配置

resolve.alias 配置项通过别名来把原导入路径映射成一个新的导入路径。例如使用以下配置：

```js
// Webpack alias 配置
resolve: {
  alias: {
    components: "./src/components/";
  }
}
```

当你通过 import Button from 'components/button'导入时，实际上被 alias 等价替换成了 import Button from './src/components/button'。

以上 alias 配置的含义是把导入语句里的 components 关键字替换成./src/components/。

这样做可能会命中太多的导入语句，alias 还支持\$符号来缩小范围到只命中以关键字结尾的导入语句：

```js
    resolve: {
      //通过别名把原导入路径映射成一个新的导入路径
      alias: {
        vue$: "vue/dist/vue.esm.js", //精准匹配
        "@": resolve("src"), //import Button from '../../components/Button' => import Button from '@/components/Button'
        // components: './src/components/',//import Button from './src/components/button' => import Button from 'components/button'
      },
    },
```

vue\$只会命中以 vue 结尾的导入语句，即只会把 import 'vue'关键字替换成 import '/path/to/vue.min.js'。

# mainFields配置

有一些第三方模块会针对不同环境提供几分代码。 例如分别提供采用 ES5 和 ES6 的 2 份代码，这 2 份代码的位置写在 package.json 文件里，如下：

```js
{
  "jsnext:main": "es/index.js",// 采用 ES6 语法的代码入口文件
  "main": "lib/index.js" // 采用 ES5 语法的代码入口文件
}
```

Webpack 会根据 mainFields 的配置去决定优先采用那份代码，mainFields 默认如下：

```js
mainFields: ["browser", "main"];
```

Webpack 会按照数组里的顺序去 package.json 文件里寻找，只会使用找到的第一个。

假如你想优先采用 ES6 的那份代码，可以这样配置：

mainFields: ['jsnext:main', 'browser', 'main']

# extensions配置

在导入语句没带文件后缀时，Webpack 会自动带上后缀后去尝试访问文件是否存在。resolve.extensions 用于配置在尝试过程中用到的后缀列表，默认是：

```js
extensions: [".js", ".json"];
```

也就是说当遇到 require('./data')这样的导入语句时，Webpack 会先去寻找./data.js 文件，如果该文件不存在就去寻找./data.json 文件， 如果还是找不到就报错。

假如你想让 Webpack 优先使用目录下的 TypeScript 文件，可以这样配置：

```js
extensions: [".ts", ".js", ".json"];
```

# modules配置

resolve.modules 配置 Webpack 去哪些目录下寻找第三方模块，默认是只会去 node_modules 目录下寻找。 有时你的项目里会有一些模块会大量被其它模块依赖和导入，由于其它模块的位置分布不定，针对不同的文件都要去计算被导入模块文件的相对路径， 这个路径有时候会很长，就像这样 import '../../../components/button'这时你可以利用 modules 配置项优化，假如那些被大量导入的模块都在./src/components 目录下，把 modules 配置成

```js
modules: ["./src/components", "node_modules"];
```

后，你可以简单通过 import 'button'导入。
