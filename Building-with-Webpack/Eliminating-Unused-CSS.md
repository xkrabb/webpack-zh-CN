### 清除未使用CSS

像Bootstrap框架的CSS文件，我们只会用到其中的一部分。`purifycss`可以帮助解决这个问题。

#### 设置purifycss

安装`npm i purifycss-webpack-plugin --save-dev`，另外安装`npm i purecss --save-dev`css框架方便演示

```js
// webpack.config.js文件
...
const PATHS = {
  app: path.join(__dirname, 'app'),
  style: [
    path.join(__dirname, 'node_modules', 'purecss'),
    path.join(__dirname, 'app', 'main.css')
  ],
  //style: path.join(__dirname, 'app', 'main.css'),
  build: path.join(__dirname, 'build')
};
...
```
```js
// app/component.js文件
module.exports = function () {
  var element = document.createElement('h1');
  element.className = 'pure-button';
  element.innerHTML = 'Hello world';
  return element;
};
```
```js
// libs/parts.js 文件
const webpack = require('webpack');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');
const PurifyCSSPlugin = require('purifycss-webpack-plugin');
...
exports.purifyCSS = function(paths) {
  return {
    plugins: [
      new PurifyCSSPlugin({
        basePath: process.cwd(),
        // 不处理路径部分，可以使用 glob's documentation
        paths: paths
      }),
    ]
  }
}
```
```js
// webpack.config.js
...
switch(process.env.npm_lifecycle_event) {
  case 'build':
    config = merge(
      ...
      parts.minify(),
      parts.extractCSS(PATHS.style),
      //必须在执行extractCSS之后
      parts.purifyCSS([PATHS.app])
    );
  default:
    ...
module.exports = validate(config);
```
>执行`npm run build`生成的css文件会变小，[更多purifyCss配置](https://github.com/purifycss/purifycss#the-optional-options-argument)

------


[`<<上一节:拆分CSS`](./Separating-CSS.md)
[`>>下一节:分析构建静态文件`](./Analyzing-Build-Statistics.md)