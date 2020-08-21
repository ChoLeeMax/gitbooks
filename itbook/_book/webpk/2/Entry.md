<!--
 * @Descripttion: webpack Entry配置
 * @version: 1.0
 * @Author: cholee
 * @Date: 2020-08-20 17:58:03
 * @LastEditors: cholee
 * @LastEditTime: 2020-08-21 17:17:13
-->

> 问题一：什么是Entry？

entry是配置模块的入口，可抽象成输入，Webpack 执行构建的第一步将从入口开始搜寻及递归解析出所有入口依赖的模块。

entry配置是必填的，若不填则将导致 Webpack 报错退出

> 问题二：什么是context？

Webpack 在寻找相对路径的文件时会以context为根目录，context默认为执行启动 Webpack 时所在的当前工作目录。 如果想改变context的默认配置，则可以在配置文件里这样设置它：

```js
module.exports = {
  context: path.resolve(__dirname, 'app')
}
```

注意，context必须是一个绝对路径的字符串。 除此之外，还可以通过在启动 Webpack 时带上参数webpack --context来设置context。

之所以在这里先介绍context，是因为 Entry 的路径和其依赖的模块的路径可能采用相对于context的路径来描述，context会影响到这些相对路径所指向的真实文件。

> 问题三：Entry 类型有哪些？

Entry 类型可以是以下三种中的一种或者相互组合：

| 类型 | 例子 | 含义 |
| :-----| :---- | :---- |	
|string	| './app/entry'| 入口模块的文件路径，可以是相对路径。| 
|array | ['./app/entry1', './app/entry2']	| 入口模块的文件路径，可以是相对路径。| 
|object	|{ a: './app/entry-a', b: ['./app/entry-b1', './app/entry-b2']}	| 配置多个入口，每个入口生成一个 Chunk| 

如果是array类型，则搭配output.library配置项使用时，只有数组里的最后一个入口文件的模块会被导出。

> 问题四：Chunk 名称

Webpack 会为每个生成的 Chunk 取一个名称，Chunk 的名称和 Entry 的配置有关：

- 如果entry是一个string或array，就只会生成一个 Chunk，这时 Chunk 的名称是main；
- 如果entry是一个object，就可能会出现多个 Chunk，这时 Chunk 的名称是object键值对里键的名称。  

> 问题五：如何配置动态 Entry

假如项目里有多个页面需要为每个页面的入口配置一个 Entry ，但这些页面的数量可能会不断增长，则这时 Entry 的配置会受到到其他因素的影响导致不能写成静态的值。其解决方法是把 Entry 设置成一个函数去动态返回上面所说的配置，代码如下：

```js
// 同步函数
entry: () => {
  return {
    a:'./pages/a',
    b:'./pages/b',
  }
};
// 异步函数
entry: () => {
  return new Promise((resolve)=>{
    resolve({
       a:'./pages/a',
       b:'./pages/b',
    });
  });
};
```