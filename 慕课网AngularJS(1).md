

#### 例子1


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

#### 例子2


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
