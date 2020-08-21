> filename 配置

output.filename 配置输出文件的名称，为 string 类型。 如果只有一个输出文件，则可以把它写成静态不变的：

```js
filename: "bundle.js";
```

但是在有多个 Chunk 要输出时，就需要借助模版和变量了。前面说到 Webpack 会为每个 Chunk 取一个名称，可以根据 Chunk 的名称来区分输出的文件名：

```js
filename: "[name].js";
```

代码里的[name]代表用内置的 name 变量去替换[name]，这时你可以把它看作一个字符串模块函数， 每个要输出的 Chunk 都会通过这个函数去拼接出输出的文件名称。

内置变量除了 name 还包括：

| 变量名    | 含义                       |
| :-------- | :------------------------- |
| id Chunk  | 的唯一标识，从 0 开始      |
| name      | Chunk 的名称               |
| hash      | Chunk 的唯一标识的 Hash 值 |
| chunkhash | Chunk 内容的 Hash 值       |

其中 hash 和 chunkhash 的长度是可指定的，[hash:8]代表取 8 位 Hash 值，默认是 20 位。

**注意：** ExtractTextWebpackPlugin 插件是使用 contenthash 来代表哈希值而不是 chunkhash， 原因在于 ExtractTextWebpackPlugin 提取出来的内容是代码内容本身而不是由一组模块组成的 Chunk。

> chunkFilename 配置

output.chunkFilename 配置无入口的 Chunk 在输出时的文件名称。 chunkFilename 和上面的 filename 非常类似，但 chunkFilename 只用于指定在运行过程中生成的 Chunk 在输出时的文件名称。 常见的会在运行时生成 Chunk 场景有在使用 CommonChunkPlugin、使用 import('path/to/module')动态加载等时。 chunkFilename 支持和 filename 一致的内置变量。

> path 配置

output.path 配置输出文件存放在本地的目录，必须是 string 类型的绝对路径。通常通过 Node.js 的 path 模块去获取绝对路径：

```js
path: path.resolve(__dirname, "dist_[hash]");
```

> publicPath 配置

在复杂的项目里可能会有一些构建出的资源需要异步加载，加载这些异步资源需要对应的 URL 地址。

output.publicPath 配置发布到线上资源的 URL 前缀，为 string 类型。 默认值是空字符串''，即使用相对路径。

这样说可能有点抽象，举个例子，需要把构建出的资源文件上传到 CDN 服务上，以利于加快页面的打开速度。配置代码如下：

```js
filename: "[name]_[chunkhash:8].js";
publicPath: "https://cdn.example.com/assets/";
```  
这时发布到线上的 HTML 在引入 JavaScript 文件时就需要：

```js
<script src="https://cdn.example.com/assets/a_12345678.js"></script>
```

使用该配置项时要小心，稍有不慎将导致资源加载 404 错误。

output.path 和 output.publicPath 都支持字符串模版，内置变量只有一个：hash 代表一次编译操作的 Hash 值。
