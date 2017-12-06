# 区别(CommonJs 与 ESModule)

##### CommonJs(Node)

* 模块运行时动态加载

```js
const fileName = 'xx.js';
const xx = require(fileName);
```

##### ESModule


* 默认开启严格模式('use strict')

```js
function name(){
    console.log(this);
}
name();
```

以上代码在nodejs中this指向全局变量global，而在ESModule中由于开启了严格模式所以指向undefined。

* 静态解析

```js
const fileName = 'xx.js';
import fileName; //error
```

##### [Tree Shaking](https://webpack.js.org/guides/tree-shaking)

利用ES2015(es6)模块语法静态解析的特性，删除没有使用的代码，减小文件大小，对代码进行优化。
webpack 2.0+ 加入了这一特性

webpack Tree Shaking 开启条件:

* 使用es2015模块语法(import 与 export)
* 使用支持无用代码移除(dead code removal)的插件，如 UglifyJSPlugin
* 去除babel-loder的模块转换插件，交给webpack来做模块转换，[babel 6.0+ 配置](http://2ality.com/2015/12/webpack-tree-shaking.html) 






##### 结论

* 浏览器端代码使用es2015 module，充分利用Tree Shaking减小代码体积
* 服务端node暂时使用node提供的commonjs规范，可以拥有更好的debug支持