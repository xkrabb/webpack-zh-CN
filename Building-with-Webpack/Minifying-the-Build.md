### 压缩构建

#### 生成基本构建

安装react并在index.js模块中加载：`npm i react --save`， `require('react');`

执行`npm run build`构建：

```sh
[webpack-validator] Config is valid.
Hash: dde0419cac0674732a83
Version: webpack 1.13.0
Time: 1730ms
     Asset       Size  Chunks             Chunk Names
    app.js     133 kB       0  [emitted]  app
app.js.map     157 kB       0  [emitted]  app
index.html  157 bytes          [emitted]
   [0] ./app/index.js 123 bytes {0} [built]
  [37] ./app/component.js 136 bytes {0} [built]
    + 36 hidden modules
Child html-webpack-plugin for "index.html":
        + 3 hidden modules
```
>依赖了`react`后，生成的`app.js`有133k

#### 压缩代码

命令行中`webpack -p`其中`-p`是`--optimize-minimize`的简写。

另外也可以通过`UglifyJsPlugin`插件实现，有更多的配置

```js
// libs/parts.js文件
...
exports.minify = function() {
  return {
    plugins: [
      new webpack.optimize.UglifyJsPlugin({
        compress: {
          warnings: false
        }
      })
    ]
  };
}
```

```js
// webpack.config.js文件
...
// 
switch(process.env.npm_lifecycle_event) {
  case 'build':
    config = merge(
      common,
      {
        devtool: 'source-map'
      },
      parts.minify(),
      parts.setupCSS(PATHS.style)
    );
    break;
  default:
    ... // 开发环境就不需要压缩了
}

module.exports = validate(config);
}
```
>压缩后`app.js`只有38k

#### 更多UglifyJS配置

```js
new webpack.optimize.UglifyJsPlugin({
  // 格式化代码
  beautify: false,
  // 清除备注
  comments: false,
  // 压缩选项
  compress: {
    warnings: false,
    // 删除`console`语句，也可以用babel-plugin-remove-console插件
    drop_console: true
  },
  // 压缩说明
  mangle: {
    // 不压缩变换$，因为有jquery
    except: ['$'],
    // 忽略IE8
    screw_ie8 : true,
    // 不压缩变换函数名
    keep_fnames: true
  }
})
```
>通过`uglify-loader`也可以实现


[`<<上一节:启用sourcemaps`](../Develop-With-Webpack/Enabling-Sourcemaps.md)
[`>>下一章:设置环境变量`](./Setting-Environment-Variables.md)