# 顶层结构

我们从[Express官网](http://expressjs.com)提供的[Hello world example](http://expressjs.com/en/starter/hello-world.html)入手，分析express顶层结构。

```js
1   const express = require('express')
2   const app = express()
3
4   app.get('/', (req, res) => res.send('Hello World!'))
5
6   app.listen(3000, () => console.log('Example app listening on port 3000!'))
```

Express是一个基于 Node.js 平台，快速、开放、极简的 web 开发框架。既然是web开发框架，那么它基础的功能必然包括启动一个web服务。

从代码第1行及第2行可以看到,要想建立一个Express应用,我们需要引入被express模块导出的顶层函数,名字自定义,一般命名为express。引入express函数后执行得到express实例。

我们通过分析express模块的package.json得知express的入口文件为express > index.js。

index.js文件内容如下：

```js
// express > index.js
9   'use strict';
10  module.exports = require('./lib/express');
```

通过分析第10行代码可以得知，index.js唯一的作用是引入并导出了'express.js'中导出的模块。

因此我们继续分析express > lib > express.js文件。

express.js文件内容如下:

```js
// express > lib > express.js
15  var bodyParser = require('body-parser')
16  var EventEmitter = require('events').EventEmitter;
17  var mixin = require('merge-descriptors');
18  var proto = require('./application');
19  var Route = require('./router/route');
20  var Router = require('./router');
21  var req = require('./request');
22  var res = require('./response');
```

第15-22行代码为express.js中依赖的模块。其中包括三个全局模块以及5个非全局模块。其中三个全局模块作用分别如下：

* body-parser：Nodejs request body解析中间件。在用户自定义的请求处理之前解析request body，解析后的内容可以通过req.body获取。
* events：Node.js核心模块之一，提供时间监听器与触发器。
* merge-descriptors: 合并对象。

