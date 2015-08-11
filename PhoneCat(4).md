### 过滤器

app.js给主模块增加一个依赖模块`phonecatFilters`（过滤器模块）

```javascript
var mainModule = angular.module('phonecatApp', [
    'ngRoute',
    'phonecatController',
    'phonecatFilters'
]);
```

在filters.js中，首先定义过滤模块。然后调用这个模块的实例方法`filter`定义过滤器，第一个参数为过滤器的名字`checkmark`，第二个参数是过滤函数它是一个匿名函数。

```javascript
// 定义过滤模块
var filterModule = angular.module('phonecatFilters', []);

// 调用模块的实例方法filter添加过滤器。过滤器的名字为checkmark
filterModule.filter('checkmark', function() {
    // 匿名函数返回一个函数，参数为html中对应的数据模型
    return function(input) {
        return input ? '\u2713' : '\u2718';
    };
});
```
