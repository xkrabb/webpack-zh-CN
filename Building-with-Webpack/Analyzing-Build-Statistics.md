### 分析构建信息

分析构建信息能更好的帮助我们理解webpack

#### 配置webpack

为了能够输出构建信息，需要两个配置

-- `profile`获取相关构建时间

-- `json`输出webpack构建信息文件

```json
// package.json 文件
{
  ...
  "scripts": {
    "stats": "webpack --profile --json > stats.json",
    ...
  },
  ...
}
```
```js
// webpack.config.js文件
...
// Run validator in quiet mode to avoid output in stats
module.exports = validate(config, {
  quiet: true
});
```
>执行`npm run stats`输出stats.json文件
>
>通过解析工具分析构建过程会更方便，推荐工具有：
>
>[The official analyse tool](http://webpack.github.io/analyse/),
>
>[Webpack Visualizer](https://chrisbateman.github.io/webpack-visualizer/),
>
>[Webpack Chart ](https://alexkuz.github.io/webpack-chart/),
>
>[robertknight/webpack-bundle-size-analyzer](https://github.com/robertknight/webpack-bundle-size-analyzer)

#### stats插件

当然还可以使用`stats-webpack-plugin`插件实现更多控制的输出

-----

[`<<上一节:清除未使用CSS`](./Eliminating-Unused-CSS.md)
[`>>下一节:部署Github-Pages`](./Hosting-on-GitHub-Pages.md)