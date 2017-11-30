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