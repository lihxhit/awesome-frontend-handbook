# 区别(CommonJs 与 ESModule)

##### CommonJs(Node)

* 模块运行时动态加载

node中模块导入require是一个内置的函数，因此只有在运行后我们才可以得知模块导出内容，无法做静态分析

```js
const fileName = 'xx.js';
const xx = require(fileName);
```

* 默认未开启严格模式

```js
console.log(this); // 指向module.exports
function name(){
    console.log(this);// 指向global
}
name();

```



node中未开启严格模式情况下全局this指向module.exports。

##### ESModule

* 默认开启严格模式('use strict')

```js
console.log(this);// undefined
function name(){
    console.log(this);// undefined
}
name();
```

* 静态解析
  * 只能作为模块顶层的语句出现，不能出现在 function 里面或是 if 里面
  * import 的模块名只能是字符串常量
  * import 声明提前，在模块顶层

##### [Tree Shaking](https://webpack.js.org/guides/tree-shaking)

利用ES2015(es6)模块语法静态解析的特性，删除没有使用的代码，减小文件大小，对代码进行优化。
webpack 2.0 加入了这一特性。

###### 源码

```js
//export.js
export function a1(){alert('a1')};
export function a2(){alert('a2')};
//import.js
import {a1} from './export.js'
a1('arguments')
```





###### 压缩后

* 关闭tree shaking

```js
!function (modules) {
    function __webpack_require__(moduleId) {
        // ...some code
    }
    var installedModules = {};
    // ...some code
}([
    function (module, exports, __webpack_require__) {
        "use strict";
        (0, __webpack_require__(1).a1)("arguements")
    },
    function (module, exports, __webpack_require__) {
        "use strict";
        Object.defineProperty(exports, "__esModule", {
            value: !0
        }),
        exports.a1 = function () {
            alert("!")
        },
        exports.a2 = function () {
            alert("!!")
        }
    }
]);
```

* 开启tree shaking

```js
!function (modules) {
    function __webpack_require__(moduleId) {
        // ...some code
    }
    var installedModules = {};
    // ...some code
}([
    function (module, __webpack_exports__, __webpack_require__) {
        "use strict";
        Object.defineProperty(__webpack_exports__, "__esModule", {
            value: !0
        });
        var __WEBPACK_IMPORTED_MODULE_0__es6_export__ = __webpack_require__(1);
        Object(__WEBPACK_IMPORTED_MODULE_0__es6_export__.a)("arguements")
    },
    function (module, __webpack_exports__, __webpack_require__) {
        "use strict";
        __webpack_exports__.a = function () {
            alert("!")
        }
    }
]);
```


###### webpack Tree Shaking 开启条件:

* 使用es2015模块语法(import 与 export)
* 使用支持无用代码移除(dead code removal)的插件，如 UglifyJSPlugin
* 去除babel-loder的模块转换插件，交给webpack来做模块转换[babel 6.0+ 配置](http://2ality.com/2015/12/webpack-tree-shaking.html)

###### 不足：

* npm公共包大多不支持es2015 module
* 与babel兼容做的不好，待babel重启modules属性

##### 结论

* 浏览器端代码使用es2015 module，模块化使用灵活，且可充分利用Tree Shaking减小代码体积
* 服务端node适合动态引入且不需要过多考虑代码体积所以使用commonjs规范，同时可以拥有更好的debug支持，提高开发效率

##### Tips

* webpack内置uglifyPlugin版本相对较低，建议不使用内置版本，单独安装
<<<<<<< HEAD
* babel6以下版本只需设置参数modules为false即可，babel6及以上只能列出除去transform-es2015-modules-commonjs所有plugin
=======
* babel6以下版本只需设置参数modules为false即可
* 建议只import需要的方法，而不是import整个模块，便于去除dead_code
>>>>>>> c767d6d57370a545bfb4b2e9911a854c6328aa4b
