### webpack语法分析

在编写js代码的时候容易犯语法错误，IDE提供比较好的语法分析检测工具

#### js语法分析简明史

`JSLint`->`JSHint`->`ESLint`

#### webpack中的JSHint（略）

#### 设置ESLint

与另外相比，它允许自定义规则

安装`npm i eslint --save-dev`

```json
// package.json文件
"scripts": {
  ...
  "test:lint": "eslint . --ext .js --ext .jsx --cache"
}
...
```
>执行`npm run test:lint`

如果要排除某些文件检测，可以在根目录下创建`.eslintignore`文件，当然在package.json中指定也是可以的

```json
"scripts": {
  ...
  "test:lint": "eslint . --ext .js --ext .jsx --ignore-path .gitignore --cache"
}
...
```

配置ESLint，配置文件为`.eslintrc`，[更多配置规则](http://eslint.org/docs/rules/)

```json
{
  // Extend existing configuration
  // from ESlint and eslint-plugin-react defaults.
  "extends": [
    "eslint:recommended", "plugin:react/recommended"
  ],
  // Enable ES6 support. If you want to use custom Babel
  // features, you will need to enable a custom parser
  // as described in a section below.
  "parserOptions": {
    "ecmaVersion": 6,
    "sourceType": "module"
  },
  "env": {
    "browser": true,
    "node": true
  },
  // Enable custom plugin known as eslint-plugin-react
  "plugins": [
    "react"
  ],
  "rules": {
    // Disable `no-console` rule
    "no-console": 0,
    // Give a warning if identifiers contain underscores
    "no-underscore-dangle": 1,
    // Default to single quotes and raise an error if something
    // else is used
    "quotes": [2, "single"]
  }
}
```
>还有更多文件类型检测...，通过`eslint --init`会生成`.eslintrc`文件

#### Babel中使用ESLint

安装`npm i babel-eslint --save-dev`

更改`.eslintrc`配置文件，注释为删除部分

```json
{
  ...
  "parser": "babel-eslint",
  //"parserOptions": {
  //  "ecmaVersion": 6,
  //  "sourceType": "module"
  //},
  ...
}
```
>报错等级：0：禁用规则，1：警告，2：错误

处理如果ESLint报错信息输出：

```json
"scripts": {
  ...
  "test:lint": "eslint . --ext .js --ext .jsx || true"
}
...
```

#### Webpack中使用ESLint

安装加载器`npm i eslint-loader --save-dev`

通过`preLoaders`设置

```js
// webpack.config.js文件
const common = {
  ...
  module: {
    preLoaders: [
      {
        test: /\.jsx?$/,
        loaders: ['eslint'],
        include: PATHS.app
      }
    ]
  },
  ...
};
```
>这样如果有语法错误，在构建过程就会中断

#### 定制ESLint

* 跳过ESLint规则

```js
// 所有
/* eslint-disable */
...
/* eslint-enable */

// 指定规则
/* eslint-disable no-unused-vars */
...
/* eslint-enable no-unused-vars */

// 设定规则
/* eslint no-comma-dangle:1 */

// 指定行
alert('foo'); // eslint-disable-line no-alert
```

#### 设置环境

```json
// .eslintrc
{
  "env": {
    "browser": true,
    "node": true,
    "mocha": true
  },
  ...
}
```
>自定义规则（略）

#### CSS语法分析

安装`npm i stylelint postcss-loader --save-dev`

```js
// webpack.config.js文件
...
const stylelint = require('stylelint');
...
const common = {
  ...
  module: {
    preLoaders: [
      {
        test: /\.css$/,
        loaders: ['postcss'],
        include: PATHS.app
      },
      ...
    ],
    ...
  },
  postcss: function () {
    return [
      stylelint({
        rules: {
          'color-hex-case': 'lower'
        }
      })
    ];
  },
  ...
}
```
>16进制颜色小写，[更多规则](https://www.npmjs.com/search?q=stylelint-config)

使用：

```js
const configSuitcss = require('stylelint-config-suitcss');
...
stylelint(configSuitcss)
```

#### EditorConfig

用来统一代码风格~~~(放置在根目录下)

```js
// .editorconfig
root = true

# General settings for whole project
[*]
indent_style = space
indent_size = 4

end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

# Format specific overrides
[*.md]
trim_trailing_whitespace = false

[app/**.js]
indent_style = space
indent_size = 2
```


[`<<上一节:理解chunks`](./Understanding-Chunks.md)
[`>>下一节:编写package`](./Authoring-Packages.md)