### 图片Loader

通过`url-loader`可以编码图片成base64，解决文件拷贝过程依赖名变化问题~

#### 设置url-Loader

webpack会将样式文件中任何使用`url`声明部分内容进行base64编码，参数为编码的限制条件

```js
{
  test: /\.(jpg|png)$/,
  loader: 'url?limit=25000',
  include: PATHS.images
}
```

#### 设置file-Loader

自定义文件名，非内联

```js
{
  test: /\.(jpg|png)$/,
  loader: 'file?name=[path][name].[hash].[ext]',
  include: PATHS.images
}
```

#### 加载SVGs

```
{
  test: /\.svg$/,
  loader: 'file',
  include: PATHS.images
}
```
>如果需要原生的svg可以通过`raw-loader`实现

#### 压缩图片

使用` image-webpack-loader`或者`svgo-loader`，压缩需要先执行，所以需要放在图片处理laoders最后一个

-----

[`<<上一节:样式Loader`](./Loading-Styles.md)
[`>>下一节:字体loader`](./Loading-Fonts.md)