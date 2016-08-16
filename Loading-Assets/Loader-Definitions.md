### Loader定义

建议在loader中指定`include`

#### Loader顺序

`loaders: ['style', 'css']`等价于`style(css(input))` 执行是从右往左的，如果是字符串loader之间用`!`链接

#### 指定Loader参数

字符串形式，在`?`后面跟上参数

```js
{
  test: /\.jsx?$/,
  loaders: [
    'babel?cacheDirectory,presets[]=react,presets[]=es2015'
  ],
  include: PATHS.app
}
```

或者添加`query`字段添加

```js
{
  test: /\.jsx?$/,
  loader: 'babel',
  query: {
    cacheDirectory: true,
    presets: ['react', 'es2015']
  },
  include: PATHS.app
}
```
>第二种方式每次参数只能应用到一个加载器上，限制了在多加载器上的使用

-----

[`<<上一节:支持的依赖格式`](./Formats-Supported.md)
[`>>下一节:样式loader`](./Loading-Styles.md)