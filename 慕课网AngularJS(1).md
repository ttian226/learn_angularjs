

#### 例子1

* 除了引入的`angular.js`没有任何js代码。
* 通过`ng-model`给`<input>`绑定一个模型变量`greeting.text`
* 这个模型变量的$scope是$rootScope（全局的），也就是在html任何位置都可以引入这个模型变量
* 通过`{{}}`引入模型变量

```html
<!DOCTYPE html>
<html lang="en" ng-app>
<head>
    <meta charset="UTF-8">
</head>
<body>
<div>
    <input ng-model="greeting.text"/>
    <p>{{greeting.text}},AngularJS</p>
</div>
</body>
<script src="angular.min.js"></script>
</html>
```

**双向绑定**
* 视图`<input>`输入框里文字的变化会引起数据模型`greeting.text`的变化（用户输入行为：视图->数据模型）
* 同样数据模型`greeting.text`的变化也会引起视图的变化（p标签文字变化：数据模型->视图）

#### 例子2

* 定义模块`hellapp`，并给`<html>`绑定这个模块。
* 定义`helloAngular`，给`$scope`增加属性`greeting`（数据模型）
* 由于`ng-controller`绑定了`<div>`，所以`$scope`作用域为这个`<div>`，即数据模型只在这个`<div>`中可以被引用。

```html
<!DOCTYPE html>
<html lang="en" ng-app="hellapp">
<head>
    <meta charset="UTF-8">
</head>
<body>
<div ng-controller="helloAngular">
    <p>{{greeting.text}},AngularJS</p>
</div>
</body>
<script src="angular.min.js"></script>
<script src="controller.js"></script>
</html>
```

```javascript
// 定义了模块hellapp
var myModule = angular.module("hellapp", []);

// 定义helloAngular
myModule.controller('helloAngular', ['$scope', function($scope) {
    $scope.greeting = {
        text: 'Hello'
    };
}]);
```


