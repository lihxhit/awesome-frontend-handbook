# 模块化发展阶段

##### 命名空间(NameSpace)

```javascript
var xxApp = {};
xxApp.namespace = function(name){
  var parts = name.split('.');
  var current = xxApp;
  for(var i in parts){
    if(!current[parts[i]]){
      current[parts[i]] = {};
    }
    current = current[parts[i]];
  }
}
xxApp.namespace('app.index');
xxApp.namespace('app.user');
```

##### 立即执行函数(IIFE)

```javascript
(function(window,documet){
//some code here
})(window,document);
```

##### CommonJs(Node)

```javascript
//math.js
exports.add = function() {
    var sum = 0, i = 0, args = arguments, l = args.length;
    while (i < l) {
        sum += args[i++];
    }
    return sum;
};
//increment.js
var add = require('./math').add;
exports.increment = function(val) {
    return add(val, 1);
};
//program.js
var inc = require('./increment').increment;
var a = 1;
inc(a); // 2
```

##### CMD(sea.js)

##### AMD(require.js)

##### ES6 Module

```js
//math.js
export default function() {
    var sum = 0, i = 0, args = arguments, l = args.length;
    while (i < l) {
        sum += args[i++];
    }
    return sum;
};
//increment.js
import add from './math';
exports default function(val) {
    return add(val, 1);
};
//program.js
import inc from './increment';
var a = 1;
inc(a); // 2
```

<script>
var xxApp = {};

xxApp.namespace = function(name){
  var parts = name.split('.');
  var current = xxApp;
  for(var i in parts){
    if(!current[parts[i]]){
      current[parts[i]] = {};
    }
    current = current[parts[i]];
  }
}
xxApp.namespace('app.index');
xxApp.namespace('app.user');
console.log(xxApp);
</script>