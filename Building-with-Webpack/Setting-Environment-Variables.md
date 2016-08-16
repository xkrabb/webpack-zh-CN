### 设置环境变量

#### DefinePlugin插件

webpack提供`defineplugin`插件，允许重写符合的自由变量`free variables`。

自由变量即没有`var`申明的全局变量，例如：

```
if(bar === 'bar') {
  console.log('bar');
}
```
>如果bar是全局变量且为"bar", 那么该语句等效于`console.log('bar');`，这是defineplugin插件的核心

#### 设置process.env.NODE_ENV

如果我们有类似`if(process.env.NODE_ENV === 'development') `语句，那么可以直接用`development`替换`process.env.NODE_ENV`


```js
// libs/parts.js
...
exports.setFreeVariable = function(key, value) {
  const env = {};
  env[key] = JSON.stringify(value);

  return {
    plugins: [
      new webpack.DefinePlugin(env)
    ]
  };
}
```
```js
// webpack.config.js
...
// Detect how npm is run and branch based on that
switch(process.env.npm_lifecycle_event) {
  case 'build':
    config = merge(
      common,
      {
        devtool: 'source-map'
      },
      parts.setFreeVariable(
        'process.env.NODE_ENV',
        'production'
      ),
      parts.minify(),
      parts.setupCSS(PATHS.style)
    );
    break;
  default:
    ...
}
module.exports = validate(config);
```
>最终`app.js`压缩结果是25.4k，另外如果浏览器支持，还可以用`gzip`进一步压缩
>
>[`babel-plugin-transform-inline-environment-variables`](https://www.npmjs.com/package/babel-plugin-transform-inline-environment-variables)插件可以实现同样的效果
>
>还有`react-dom`没有加入，当然可以选择更小的`preact`或者`react-lite`他们提供更少的功能，但是却更小


[`<<上一节:压缩构建`](./Minifying-the-Build.md)
[`>>下一节:拆分生成文件`](./Splitting-Bundles.md)