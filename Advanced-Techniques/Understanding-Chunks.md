### 理解Chunks

#### Chunks类型

* `entry chunks`:webpack运行时候和模块加载的入口文件
* `normal chunks`:除了entry chunks外，使用过程中动态加载的内容，例如jsonp
* `initial chunks`:通过`CommonsChunkPlugin`生成的相关文件

`entry chunks`之前已经涉及不再深究，下面介绍的是`normal chunks`（懒加载）的主要应用。

webpack的强大之处在于，能够拆分应用代码成更小的文件，实现动态加载。

#### 通过lunr实例介绍懒加载

`lunr`是一个前端实现全文检索的插件，以下为实现代码

[参考react-lunr应用代码](http://survivejs.com/webpack/advanced-techniques/understanding-chunks/#introduction-to-lazy-loaded-search-with-lunr-)

>webpack检测`require.ensure`，然后拆分被依赖的文件，这种方式同样适用于路由，在用户点击相应路由后，开始加载相应的内容。通过`require.ensure`也可以是先滚动加载等。


#### webpack的动态加载

除了`require.ensure`还有一个配置项需要知道`require.context`,它允许`require`进在编译时候不确定的内容。这种技术应用在生成静态，测试时候。

例如静态网站构建：（markdown内容在pages下）

```js
// 通过加载器处理页面，此时markdown还没有，此时加载会在有md文件时候处理
const req = require.context(
  'json!yaml-frontmatter!./pages',
  true, // Load files recursively. Pass false to skip recursion.
  /^\.\/.*\.md$/ // Match files ending with .md.
);
```
>实际应用项目[Antwar](https://github.com/antwarjs/antwar)，静态网站生成器

-----

[`<<上一节:字体Loader`](../Loading-Assets/Loading-Fonts.md)
[`>>下一节:webpack语法分析`](./Linting-in-Webpack.md)