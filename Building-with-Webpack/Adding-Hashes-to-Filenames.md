### 文件名添加hash

webpack为输出文件名提供替换文本：

* `[path]` - 入口文件(entry)路径.
* `[name]` - 入口文件名.
* `[hash]` - 构建生成的hash.
* `[chunkhash]` - 生成文件的hash.

```js
...
{
  output: {
    path: PATHS.build,
    filename: '[name].[chunkhash].js',
  }
}
···
```
>生成的文件名带hash可以有效避免缓存

#### 设置hashing

```js
// webpack.config.js
...
switch(process.env.npm_lifecycle_event) {
  case 'build':
    config = merge(
      common,
      {
        devtool: 'source-map',
        output: {
          path: PATHS.build,
          filename: '[name].[chunkhash].js',
          // This is used for require.ensure. The setup
          // will work without but this is useful to set.
          chunkFilename: '[chunkhash].js'
        }
      },
      ...
    );
    break;
  default:
    ...
}
module.exports = validate(config);
```

-----

[`<<上一节:拆分输出文件`](./Splitting-Bundles.md)
[`>>下一节:清除生成文件夹`](./Cleaning-the-Build.md)