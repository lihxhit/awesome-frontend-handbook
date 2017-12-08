# 顶层结构

我们从[Express官网](http://expressjs.com)提供的[Hello world example](http://expressjs.com/en/starter/hello-world.html)入手，分析express顶层结构。

```js
const express = require('express')
const app = express()

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(3000, () => console.log('Example app listening on port 3000!'))
```

