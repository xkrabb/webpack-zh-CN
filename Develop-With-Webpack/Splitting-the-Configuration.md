### 拆分配置文件

在小项目中，webpack配置文件可以写在一个文件中，随着项目变大，将会变得难以维护。所以根据不同的运行环境拆分配置文件很有必要，对于如何拆分是件仁者见仁智者见智的事件，没有什么完全正确方法，有如下几种可行的方式：

* 在命令行中，`--config`指定所需要的`webpack.config.js`配置文件
* 使用第三方插件，例如：[`HenrikJoreteg/hjs-webpack`](https://github.com/HenrikJoreteg/hjs-webpack)
* 根据不同的`npm`运行命令，触发不同的js设置webpack配置信息。

下面针对最后一种方式作相关说明，这里需要[`webpack-merge`](https://www.npmjs.org/package/webpack-merge)插件(它会合并配置文件中相同的属性名下的值)。

#### 设置webpack-merge

安装`npm i webpack-merge --save-dev`

基本的`webpack.config.js`配置

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const merge = require('webpack-merge');

const PATHS = {
  app: path.join(__dirname, 'app'),
  build: path.join(__dirname, 'build')
};

// module.exports = {
const common = {
  // 后续将扩展更多入口文件
  entry: {
    app: PATHS.app
  },
  output: {
    path: PATHS.build,
    filename: '[name].js'
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: 'Webpack demo'
    })
  ]
};

var config;
// 根据不同的`npm run`命令合并成不同的配置内容
switch(process.env.npm_lifecycle_event) {
  case 'build':
    config = merge(common, {});
    break;
  default:
    config = merge(common, {});
}
module.exports = config;
```

#### 整合wepback验证插件

[` webpack-validator`](https://www.npmjs.com/package/webpack-validator)插件验证配置文件的正确性，安装`npm i webpack-validator --save-dev`

`webpack.config.js`配置

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const merge = require('webpack-merge');
const validate = require('webpack-validator');
...

// module.exports = config;
module.exports = validate(config);
```

#### 总结

这种方式适合小-中项目，大型项目需要其他方式。

-----

[`<<上一节:开始`](./Getting-Started.md)
[`>>下一节:自动刷新浏览器`](./Automatic-Browser-Refresh.md)