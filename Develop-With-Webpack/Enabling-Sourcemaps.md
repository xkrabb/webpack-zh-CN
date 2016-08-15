### 启用Sourcemaps

sourcemaps是编译前源代码和生成代码的映射关系文件，有了它在调试的时候能更快的定位到源代码的位置和错误原因。

#### 开发过程启用sourcemaps

配置`eval-source-map`选项

```js
// webpack.config.js
...
switch(process.env.npm_lifecycle_event) {
  case 'build':
    config = merge(
      common,
      {
        devtool: 'source-map'
      },
      parts.setupCSS(PATHS.app)
    );
  default:
    config = merge(
      common,
      {
      	  // 第一次构建慢，但是之后构建会更快，更快的方式cheap-module-eval-source-map，eval
        devtool: 'eval-source-map'
      },
      parts.setupCSS(PATHS.app),
      ...
    );
}
module.exports = validate(config);
```

#### webpack支持的sourcemaps类型

开发环境下建议使用：`eval`,`cheap-eval-source-map`,`cheap-module-eval-source-map`,`eval-source-map`(从左到右质量越高，执行越耗时间)

生产环境：`cheap-source-map`，`cheap-module-source-map`，`source-map`(执行质量和时间，同上)

完整的sourcemaps选项

```js
const config = {
  output: {
    // sourcemaps输出名 [file], [id], [hash]替换，默认即可
    sourceMapFilename: '[file].map', // Default

    // sourcemap文件名模板. 根据devtol来，不建议修改
    devtoolModuleFilenameTemplate: 'webpack:///[resource-path]?[loaders]'
  },
  ...
};
```

#### SourceMapDevToolPlugin插件

如果希望能有更多的soucremaps生成配置，可以使用`SourceMapDevToolPlugin`插件。

[更多插件配置](https://webpack.github.io/docs/list-of-plugins.html#sourcemapdevtoolplugin)

#### 样式文件生成sourcemaps

在配置文件中加入sourcemaps参数，例如`style!css?sourceMap`（建议在output中指定`output.publicPath`来解析服务器url）

-----


[`<<上一节:刷新CSS`](./Refreshing-CSS.md)
[`>>下一章:最小化build（生产构建）`](./Enabling-Sourcemaps.md)