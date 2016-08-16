### 支持的依赖格式

#### js模块格式

webpack2将支持ES6，现在只能通过`Bable`和`bable-laoder`来实现ES6

* CommonJS

```js
var MyModule = require('./MyModule');
// export at module root
module.exports = function() { ... };
// alternatively, export individual functions
exports.hello = function() {...};
```

* ES6

```js
import MyModule from './MyModule.js';
// export at module root
export default function () { ... };
// or export as module function,
export function hello() {...};
```

* AMD

```js
define(['./MyModule.js'], function (MyModule) {
  // export at module root
  return function() {};
});
define(['./MyModule.js'], function (MyModule) {
  // export as module function
  return {
    hello: function() {...}
  };
});

define(['require'], function (require) {
  var MyModule = require('./MyModule.js');
  return function() {...};
});
```

* UMD

通用模块定义(`Universal Module Definition`)，[参考](https://github.com/umdjs/umd)

----

[`<<上一节:github-pages部署`](../Building-with-Webpack/Hosting-on-GitHub-Pages.md)
[`>>下一节:Loader定义`](./Loader-Definitions.md)