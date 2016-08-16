### 清除build文件夹

到目前，重复构建的时候并没有清除原来的build文件夹，通过命令行`rm -rf ./build && webpack`执行也删除build文件夹后再执行webpack

#### 设置clean-webpack-plugin

安装`npm i clean-webpack-plugin --save-dev`

```js
// libs/parts.js
const webpack = require('webpack');
const CleanWebpackPlugin = require('clean-webpack-plugin');
...
exports.clean = function(path) {
  return {
    plugins: [
      new CleanWebpackPlugin([path], {
        // Without `root` CleanWebpackPlugin won't point to our
        // project and will fail to work.
        root: process.cwd()
      })
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
      common,
		...
      parts.clean(PATHS.build),
      ...
    );
    break;
  default:
    ...
}
module.exports = validate(config);
```
>如果需要保留点开头的文件，可以用`path.join(PATHS.build, '*')`代替`PATHS.build`

------

[`<<上一节:为文件名添加hash`](./Adding-Hashes-to-Filenames.md)
[`>>下一节:拆分CSS文件`](./Separating-CSS.md)