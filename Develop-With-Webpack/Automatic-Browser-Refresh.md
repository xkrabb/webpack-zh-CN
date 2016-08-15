### 浏览器自动刷新

`LiveReload`或者`Browsersync`能够检测我们的开发代码变化而自动刷新浏览器，在webpack也有相同方式。

在开发环境中，命令行下可以通过`webpack --watch`方式，在开发代码变化时候会自动重新编译打包代码。另外`webpack-dev-server`插件也是一个很好的方式，它还支持`Hot Module Replacement (HMR)`热替换，自动刷新浏览器。

>如果不是开发环境下，可以考虑其他方式，如`apache`或者`nginx`
>
>在使用IDE开放的时候，需要取消写安全模式，才能让HMR生效。


#### 开始使用webpack-dev-server

安装`npm i webpack-dev-server --save-dev`

快速开启方式`webpack-dev-server --inline`

在`package.json`中配置

```json
...
"scripts": {
  "start": "webpack-dev-server",
  "build": "webpack"
},
...
```
>可以添加`--inline`在`start`命令中。此时代码变更会重新编译但是浏览器并不自动刷新。

#### 配置热替换(HMR)

命令行中开启hmr：`webpack-dev-server --inline --hot`

定义HMR配置，创建`libs/parts.js`文件

```js
// libs/parts.js文件
const webpack = require('webpack');
exports.devServer = function(options) {
  return {
    devServer: {
      // 启用HTML5 Histort API（实现ajax的回退）
      historyApiFallback: true,
      // 这里并需要设置 HotModuleReplacementPlugin!
      hot: true,
      inline: true,
      // 错误时候输出
      stats: 'errors-only',
      // 设置host和port
      host: options.host, // Defaults to `localhost`
      port: options.port // Defaults to 8080
    },
    plugins: [
      // 启用`multi-pass`在大的项目中优化执行
      // in larger projects. Good default.
      new webpack.HotModuleReplacementPlugin({
        multiStep: true
      })
    ]
  };
}
```

```js
// webpack.config.js文件

const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const merge = require('webpack-merge');
const validate = require('webpack-validator');
const parts = require('./libs/parts');

...
switch(process.env.npm_lifecycle_event) {
  case 'build':
    config = merge(common, {});
    break;
  default:
    // config = merge(common, {});
    // 合并在/libs/parts.js里的配置
    config = merge(
      common,
      parts.devServer({
        // Customize host/port here if needed
        host: process.env.HOST,
        port: process.env.PORT
      })
    );
}

module.exports = validate(config);
```
>默认启动链接`localhost:8080`

#### HMR在windows，ubuntu和vagrant上处理

以上步骤在windows,ubuntu和vargrant上可能会有问题，需要额外处理

```
libs/parts.js

const webpack = require('webpack');

exports.devServer = function(options) {
  return {
    watchOptions: {
      // Delay the rebuild after the first change
      aggregateTimeout: 300,
      // Poll using interval (in ms, accepts boolean too)
      poll: 1000
    },
    devServer: {
      ...
    },
    ...
  };
}
```

#### 另一种方式

除了使用`webpack-dev-server`还有另外方式，可以在Express里皮遏制，通过`middleware`插件，或者是查阅webpack的[`Node.js API`](https://webpack.github.io/docs/webpack-dev-server.html#api)

>`dotenv`插件，允许通过**.env**读取相关需要设置的环境变量

CLI和Nodejs API两种方式是不同的，[有些情况下](https://github.com/webpack/webpack-dev-server/issues/106)建议使用Nodejs API方式

-----

[`<<上一节:拆分配置文件`](./Splitting-the-Configuration.md)
[`>>下一节:刷新CSS`](./Refreshing-CSS.md)


