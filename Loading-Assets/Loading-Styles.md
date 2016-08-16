### 样式Loader

#### 加载CSS

```js
const common = {
  ...
  module: {
    loaders: [
      {
        test: /\.css$/,
        loaders: ['style', 'css'],
        include: PATHS.style
      }
    ]
  },
  ...
};
```
>如果css需要输出sourcemaps，设置`['style', 'css?sourceMap']`和`output.publicPath`

#### 预处理器

less,sass,postcss,cssnext...

```js
{
  test: /\.less$/,
  loaders: ['style', 'css', 'less'],
  include: PATHS.style
}
```

```js
{
  test: /\.scss$/,
  loaders: ['style', 'css', 'sass'],
  include: PATHS.style
}
```

```js
const autoprefixer = require('autoprefixer');
const precss = require('precss');
module.exports = {
  module: {
    loaders: [
      {
        test: /\.css$/,
        loaders: ['style', 'css', 'postcss'],
        include: PATHS.style
      }
    ]
  },
  // autoprefixer自动兼容添加前缀
  postcss: function () {
      return [autoprefixer, precss];
  }
};
```



[`<<上一节:定义Loader`](./Loader-Definitions.md)
[`>>下一节:图片loader`](./Loading-Images.md)