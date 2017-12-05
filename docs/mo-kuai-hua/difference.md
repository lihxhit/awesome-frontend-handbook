# 区别(CommonJs 与 ESModule)

##### Node

* 模块运行时动态加载


##### ESModule


* 默认开启严格模式('use strict')
* 静态解析



webpack Tree Shaking

Conclusion
So, what we've learned is that in order to take advantage of tree shaking, you must...

Use ES2015 module syntax (i.e. import and export).
Include a minifier that supports dead code removal (e.g. the UglifyJSPlugin).