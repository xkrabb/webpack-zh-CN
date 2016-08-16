### 编写package

#### 分析npm package

-`index.js` 小项目只要一个index.js入口文件即可

-`package.json` npm安装json格式

-`README.md` 项目介绍文档

-`LICENSE` 协议

-`CONTRIBUTING.md` 贡献列表

-`CHANGELOG.md` 变更日志

-`.travis.yml` 

-`.gitignore` git忽略文件

-`.npmignore` npm安装忽略文件

-`.eslintignore` esling语法检测忽略文件

-`.eslintrc` eslint语法规则

-`webpack.config.js` webpack配置文件

#### 理解package.json

```json
{
  /* Name of the project */
  "name": "react-component-boilerplate",
  /* Brief description */
  "description": "Boilerplate for React.js components",
  /* Who is the author + optional email + optional site */
  "author": "Juho Vepsäläinen <email goes here> (site goes here)",
  /* Version of the package */
  "version": "0.0.0",
  /* `npm run <name>` */
  "scripts": {
    "start": "webpack-dev-server",

    "test": "karma start",
    "test:tdd": "npm run test -- --auto-watch --no-single-run",
    "test:lint": "eslint . --ext .js --ext .jsx --cache",

    "gh-pages": "webpack",
    "gh-pages:deploy": "gh-pages -d gh-pages",
    "gh-pages:stats": "webpack --profile --json > stats.json",

    "dist": "webpack",
    "dist:min": "webpack",
    "dist:modules": "rm -rf ./dist-modules && babel ./src --out-dir ./dist-modules",

    "pretest": "npm run test:lint",
    "preversion": "npm run test && npm run dist && npm run dist:min && git commit --allow-empty -am \"Update dist\"",
    "prepublish": "npm run dist:modules",
    "postpublish": "npm run gh-pages && npm run gh-pages:deploy",
    /* If your library is installed through Git, you may want to transpile it */
    "postinstall": "node lib/post_install.js"
  },
  /* Entry point for terminal (i.e., <package name>) */
  /* Don't set this unless you intend to allow CLI usage */
  "bin": "./index.js",
  /* Entry point (defaults to index.js) */
  "main": "dist-modules",
  /* Package dependencies */
  "dependencies": {},
  /* Package development dependencies */
  "devDependencies": {
    "babel": "^6.3.17",
    ...
    "webpack": "^1.12.2",
    "webpack-dev-server": "^1.12.0",
    "webpack-merge": "^0.7.0"
  },
  /* Package peer dependencies. The consumer will fix exact versions. */
  /* In npm3 these won't get installed automatically and it's up to the */
  /* user to define which versions to use. */
  /* If you want to include RC versions to the range, consider using */
  /* a pattern such as ^4.0.0-0 */
  "peerDependencies": {
    "lodash": ">= 3.5.0 < 4.0.0",
    "react": ">= 0.11.2 < 16.0.0"
  }
  /* Links to repository, homepage, and issue tracker */
  "repository": {
    "type": "git",
    "url": "https://github.com/bebraw/react-component-boilerplate.git"
  },
  "homepage": "https://bebraw.github.io/react-component-boilerplate/",
  "bugs": {
    "url": "https://github.com/bebraw/react-component-boilerplate/issues"
  },
  /* Keywords related to package. */
  /* Fill this well to make the package findable. */
  "keywords": [
    "react",
    "reactjs",
    "boilerplate"
  ],
  /* Which license to use */
  "license": "MIT"
}
```

`webpack`的`output.libraryTarget`和`externals`配置项，自行了解

-----

[`<<上一节:webpack语法分析`](./Linting-in-Webpack.md)

-----

另外：

[`自定义loader`](http://survivejs.com/webpack/advanced-techniques/writing-loaders/)

[`配置react`](http://survivejs.com/webpack/advanced-techniques/configuring-react/)
