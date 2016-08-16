### 字体Loader

#### 选择一种字体

大部分浏览器支持`.woff`格式字体，除Opera Mini，所以如果不需要支持Opera，则：

```js
{
  test: /\.woff$/,
  loader: 'url?limit=50000',
  include: PATHS.fonts
}
```
```js
// 支持woff2格式
{
  // Match woff2 in addition to patterns like .woff?v=1.1.1.
  test: /\.woff(2)?(\?v=[0-9]\.[0-9]\.[0-9])?$/,
  loader: 'url',
  query: {
    limit: 50000,
    mimetype: 'application/font-woff',
    name: './fonts/[hash].[ext]'
  }
}
```

#### 多字体支持

```js
{
  test: /\.woff$/,
  // Inline small woff files and output them below font/. ,Set mimetype just in case.
  loader: 'url',
  query: {
    name: 'font/[hash].[ext]',
    limit: 5000,
    mimetype: 'application/font-woff'
  },
  include: PATHS.fonts
},
{
  test: /\.ttf$|\.eot$/,
  loader: 'file',
  query: {
    name: 'font/[hash].[ext]'
  },
  include: PATHS.fonts
}
```

------

[`<<上一节:图片Loader`](./Loading-Images.md)
[`>>下一节:理解Chuncks`](../Advanced-Techniques/Understanding-Chunks.md)