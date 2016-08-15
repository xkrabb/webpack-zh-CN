### 开始

安装最新的Node.Js，推荐使用LTS版本。在往下看之前，确保`node`和`npm`命令在终端可用。（可用`nvm`管理nodejs版本，老版本有些功能没有需要额外处理）

#### 项目构建

创建的基础命令，参考如下：（也可用手动创建）

```sh
mkdir webpack-demo
cd webpack-demo
npm init -y # -y generates *package.json*, skip for more control
```
>package.json文件可用手动更改，[更多配置](https://docs.npmjs.com/files/package.json)后续也有介绍

#### 安装Webpack

在项目目录下通过可以通过`npm i webpack -g`安装在全局，也可以`npm i webpack --save-dev`安装在项目中。

#### 执行Webpack

如果安装在全局可以直接`webpak`，安装在项目中，需要在项目目录下执行`node_modules/.bin/webpack`。（后续以全局安装为例）

#### 目录结构

```
-app/
    -index.js
    -component.js
-build/
-package.json
-webpack.config.js
```

#### 资源配置

app/component.js

```js
module.exports = function () {
  var element = document.createElement('h1');

  element.innerHTML = 'Hello world';

  return element;
};
```

app/index.js

```js
var component = require('./component');
document.body.appendChild(component());
```

#### webpack配置文件

`webpack.config.js`是配置文件，webpack和开发服务器就可以更方便的通过该配置文件找到相关文件。

使用[`html-webpack-plugin`](https://www.npmjs.com/package/html-webpack-plugin)插件，让我们的项目更容易维护。通过它可以直接生成简单的`index.html`文件，或通过相关的html模板生成，更多配置查看插件本身介绍。

在项目中安装`npm i html-webpack-plugin --save-dev`

`webpack.config.js`配置文件：

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

const PATHS = {
  app: path.join(__dirname, 'app'),
  build: path.join(__dirname, 'build')
};

module.exports = {
  // 入口文件，后续会有更复杂配置
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
```
>`entry`路径可以是相对也可以是绝对路径。

执行`webpack`（全局安装）后，终端输出：

```sh
Hash: 2a7a7bccea1741de9447
Version: webpack 1.13.0
Time: 813ms
     Asset       Size  Chunks             Chunk Names
    app.js    1.69 kB       0  [emitted]  app
index.html  157 bytes          [emitted]
   [0] ./app/index.js 80 bytes {0} [built]
   [1] ./app/component.js 136 bytes {0} [built]
Child html-webpack-plugin for "index.html":
        + 3 hidden modules
```
> `hash`：构建过程生成的标志，可以通过[hash]获取
> 
> `version`：webpack的版本号
> 
> `Time`：执行时间
> 
> `app.js 1.69 kB 0 [emitted] app`：资源名称，大小，chunks的id，生成状态信息，chunks名字
> 
> `[0] ./app/index.js 80 bytes {0} [built]`：生成输出的资源相关信息
> 
> `Child html-webpack-plugin for "index.html"`：插件`html-webpack-pulugin`相关输出
> 
> `+ 3 hidden modules`：webpack省略的相关输出，主要是`node_modules`依赖包里的相关模块，在命令行通过`--display -modules`可以显示隐藏项

* 安装`npm i server -g`服务器，打开build生成目录
* 上面用`path.join`拼接路径，当然`path.resolve`也是一个很好选择，具体参看nodejs的path相关API
* `favicons-webpack-plugin`插件可以在webpack中更好的处理favicons

#### 简便构建方式

如果webpack不是全局安装，需要通过`node_modules/.bin/webpack`指定webpack命令路径才能执行。一种更简便的方式是在`package.json`的`scripts`字段设置任务

```json
...
"scripts": {
  "build": "webpack"
},
...
```
>通过`npm run xxx`执行相关任务，此处使用`npm run build`。

#### 总结

虽然我完成了一个基本webpack设置，但是正式开发中需要每次都`build`是件非常痛苦的事情。所以还需要更高级的配置。。。

-----

[`>>下一节:拆分配置文件`](./Splitting-the-Configuration.md)









