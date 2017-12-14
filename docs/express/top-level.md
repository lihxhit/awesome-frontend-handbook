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

从代码第1行可以看到,要想建立一个Express应用,我们需要引入被express模块导出的顶层函数,名字自定义,一般命名为express。