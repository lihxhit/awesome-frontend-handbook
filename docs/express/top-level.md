# 顶层结构

我们从[Express官网](http://expressjs.com)提供的[Hello world example](http://expressjs.com/en/starter/hello-world.html)入手，分析express顶层结构。

```js 简单的http服务
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World!\n');
});

server.listen(3000, () => {
  console.log(`Server running at http://${hostname}:3000/`);
});
```

```js 基于express的简单http服务
1   const express = require('express')
2   const app = express()
3
4   app.use((req, res) => res.send('Hello World!'))
5
6   app.listen(3000, () => console.log('Example app listening on port 3000!'))
```

Express是一个基于 Node.js 平台，快速、开放、极简的 web 开发框架。既然是web开发框架，那么它基础的功能必然包括启动一个web服务。

从代码第1行及第2行可以看到,要想建立一个Express应用,我们需要引入被express模块导出的顶层函数,名字自定义,一般命名为express。引入express函数后执行得到express实例。

我们通过分析express模块的package.json得知express的入口文件为express > index.js。

index.js文件内容如下：

```js
// express > index.js
1   'use strict';
2  module.exports = require('./lib/express');
```

由以上代码可以得知，index.js唯一的作用是引入并导出了'express.js'中导出的模块。

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

```js
25  /**
26   * Expose `createApplication()`.
27   */
28
29  exports = module.exports = createApplication;
```

29行通过module.exports导出了函数createApplication，由此可知express实例是通过执行createApplication函数获取到的。

```js
37  function createApplication() {
38    var app = function(req, res, next) {
39      app.handle(req, res, next);
40    };
41
42    mixin(app, EventEmitter.prototype, false);
43    mixin(app, proto, false);

44    // expose the prototype that will get set on requests
45    app.request = Object.create(req, {
46    app: { configurable: true, enumerable: true, writable: true, value: app }
47    })
48
49    // expose the prototype that will get set on responses
50    app.response = Object.create(res, {
51      app: { configurable: true, enumerable: true, writable: true, value: app }
52    })
53
54    app.init();
55    return app;
56  }
```

38行用函数表达式方式创建一个函数，  

```js
59: /**
60:  * Expose the prototypes.
61:  */
62:
63: exports.application = proto;
64: exports.request = req;
65: exports.response = res;
66:
67: /**
68:  * Expose constructors.
69:  */
70:
71: exports.Route = Route;
72: exports.Router = Router;
73:
74: /**
75:  * Expose middleware
76:  */
77:
78: exports.json = bodyParser.json
79: exports.query = require('./middleware/query');
80: exports.static = require('serve-static');
81: exports.urlencoded = bodyParser.urlencoded
```

59-81行主要是导出了原型，构造函数，与常用中间件。

```js
087: ;[
088:   'bodyParser',
089:   'compress',
090:   'cookieSession',
091:   'session',
092:   'logger',
093:   'cookieParser',
094:   'favicon',
095:   'responseTime',
096:   'errorHandler',
097:   'timeout',
098:   'methodOverride',
099:   'vhost',
100:   'csrf',
101:   'directory',
102:   'limit',
103:   'multipart',
104:   'staticCache',
105: ].forEach(function (name) {
106:   Object.defineProperty(exports, name, {
107:     get: function () {
108:       throw new Error('Most middleware (like ' + name + ') is no longer bundled with Express and must be installed separately. Please see        https://github.com/senchalabs/connect#middleware.');
109:     },
110:     configurable: true
111:   });
112: });
113:
```

87到113行利用 Object.defineProperty 对exports进行属性定义，列出所有废弃和移除的中间件，当获取是会抛出异常。