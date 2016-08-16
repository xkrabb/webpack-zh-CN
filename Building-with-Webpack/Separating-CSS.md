### 拆分CSS

目前如果css和js是在一个entry编译的，如果其中任一种内容改变都会重新编译。为了能够`FOUC(Flash of Unstyled Content)`，通过`ExtractTextPlugin`可以完成样式文件的拆分

#### 设置extract-text-webpack-plugin

安装`npm i extract-text-webpack-plugin --save-dev`

```js
// libs/parts.js
const webpack = require('webpack');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');
...
exports.extractCSS = function(paths) {
  return {
    module: {
      loaders: [
        {
          test: /\.css$/,
          // 可以通过!链接更多的loader
          loader: ExtractTextPlugin.extract('style', 'css'),
          include: paths
        }
      ]
    },
    plugins: [
      // Output extracted CSS to a file
      new ExtractTextPlugin('[name].[chunkhash].css')
    ]
  };
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
      parts.extractCSS(PATHS.app)
      // parts.setupCSS(PATHS.app)
    );
    break;
  default:
    config = merge(
      ...
    );
}
module.exports = validate(config);
```

#### 分开应用代码和样式

```js
// app/index.js文件
require('react');
// require('./main.css');
...
```
```js
// webpack.config.js文件
...
const PATHS = {
  app: path.join(__dirname, 'app'),
  // 独立样式文件路径
  style: path.join(__dirname, 'app', 'main.css'),
  build: path.join(__dirname, 'build')
};
const common = {
  entry: {
    style: PATHS.style,
    app: PATHS.app
  },
  ...
};

switch(process.env.npm_lifecycle_event) {
  case 'build':
    config = merge(
      ...
      parts.minify(),
      parts.extractCSS(PATHS.style)
     //parts.extractCSS(PATHS.app)
    );
    break;
  default:
    config = merge(
      ...
      parts.setupCSS(PATHS.style),
      // parts.setupCSS(PATHS.app),
      ...
    );
}
module.exports = validate(config);
```
>如果自己观察会发现执行命令以后会多处一个`style.xxxxxx.js`的文件，内容类似于`webpackJsonp([1,3],[function(n,c){}]);`。这个文件内容并没有实际作用，webpack生成的，HtmlWebpackPlugin也会包含进去，自行找办法解决吧。

-----

[`<<上一节:清除build`](./Cleaning-the-Build.md)
[`>>下一节:清除未使用CSS`](./Eliminating-Unused-CSS.md)