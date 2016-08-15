### 刷新CSS

#### 加载CSS

在设置css前需要2个加载器`loaders`，安装`npm i css-loader style-loader --save-dev`

相关配置

```js
// libs/parts.js文件
...
exports.setupCSS = function(paths) {
  return {
    module: {
      loaders: [
        {	
        	// 正则匹配文件名
          test: /\.css$/,
          // 先style在css，执行是先编译css，再设置style样式link到html中
          loaders: ['style', 'css'],
          // 包含的文件路径，还有exclude表示排除路径
          include: paths
        }
      ]
    }
  };
}
```

```js
// webpack.config.js文件
...
switch(process.env.npm_lifecycle_event) {
  case 'build':
    // config = merge(common, {});
    config = merge(
      common,
      parts.setupCSS(PATHS.app)
    );
    break;
  default:
    config = merge(
      common,
      parts.setupCSS(PATHS.app),
      ...
    );
}
module.exports = validate(config);
```
>loaders组数中的执行顺序是从右到左，css处理css中`@import`和`url`声明，style处理js中的`require`声明，如果还有预处理器（sass，less）直接将loader加在后面。加载器是处理资源文件，它们可以链式调用。


#### 设置初始化CSS

需要在使用的样式的js文件用`require()`进相关样式文件

```css
// app/main.css文件
body {
  background: cornsilk;
}
```

```js
// app/index.js文件
require('./main.css');
...
```
>最好是将样式entry与js的entry分开，后续会介绍

#### 理解CSS作用域和CSS模块

通过`reuqire`进来的样式文件会在html中生成一个style标签，所以它相当于全局可用。

通过`style!css?modules`(css loader另一种配置写法)方式能够让require只在当前模块生效。如果需要样式全局有效，需要写成`:global(body) { ... }`

如果不用`moulde`参数，需要`:local(.redButton) {background: red;}`实现局部模块有效

------

[`<<上一节:自动刷新浏览器`](./Automatic-Browser-Refresh.md)
[`>>下一节:启用Sourcemaps`](./Enabling-Sourcemaps.md)