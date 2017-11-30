# CommonJs(Node)

##### 特点

- 每个文件被视为一个独立的模块

##### 关键字

**exports** , **module** , **require**

##### exports

exports 变量是在模块的文件级别作用域内有效的，它在模块被执行前被赋予 module.exports 的值。

它有一个快捷方式，以便 module.exports.f = ... 可以被更简洁地写成 exports.f = ...
注意，就像任何变量，如果一个新的值被赋值给 exports，它就不再绑定到 module.exports：

```javascript
module.exports.hello = true; // 从对模块的引用中导出
exports = { hello: false };  // 不导出，只在模块内有效
```

##### Node导出模块常用方式

###### 到处命名空间

```javascript
const fs = require('fs');
fs.unlink('/tmp/hello', (err) => {
  if (err) throw err;
  console.log('成功删除 /tmp/hello');
});
```

###### 导出函数

```javascript
//express.js
module.exports = function(){
    //some code here
};
//app.js
const express = require('./express');
const app = express();
```

###### 导出高阶函数

```javascript
//serve-favicon.js
module.exports = function(path){
    //some code here
    return function(req,res,next){
        //some code here
    }
}
const path = require('path');
const express = require('express');
const favicon = require('./serve-favicon');
const app = express();
app.use(favicon(path.join(__dirname, '/public/favicon.ico')));
```