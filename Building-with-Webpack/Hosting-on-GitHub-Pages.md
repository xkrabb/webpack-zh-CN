### 部署Github-Pages

#### 设置gh-pages

安装`npm i gh-pages --save-dev`

```json
// package.json文件
{
  ...
  "scripts": {
    "deploy": "gh-pages -d build",
    ...
  },
  ...
}
```
>确保能再github-pages上使用，需要配置`output.publicPage`
>
>更多可以查阅nodejs API的`gp-pages`

-----

[`<<上一节:构建分析`](./Analyzing-Build-Statistics.md)
[`>>下一章:格式化支持`](../Loading-Assets/Formats-Supported.md)