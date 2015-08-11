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
        // 当input为ture时返回'\u2713'（√），为false时返回'\u2718'（×）
        return input ? '\u2713' : '\u2718';
    };
});
```

在AngularJS模板中使用的过滤器语法为:
`{{ expression | filter }}`

在模板phone-detail.html中添加了过滤器`checkmark`，例如`{{phone.connectivity.infrared | checkmark}}`,用`checkmark`替换数据模型`phone.connectivity.infrared`的值。

```html
<li>
    <span>Connectivity</span>
    <dl>
        <dt>Infrared</dt>
        <dd>{{phone.connectivity.infrared | checkmark}}</dd>
        <dt>GPS</dt>
        <dd>{{phone.connectivity.gps | checkmark}}</dd>
    </dl>
</li>
```

#### 事件处理器

在controller.js中修改控制器`PhoneDetailCtrl`，创建了`mainImageUrl`模型属性，并把它的默认值设置为第一张图片

```javascript
phoneModule.controller('PhoneDetailCtrl', ['$scope', '$routeParams', '$http', function($scope, $routeParams, $http) {
    $http.get('phones/' + $routeParams.phoneId + '.json').success(function(data) {
        $scope.phone = data;
        $scope.mainImageUrl = data.images[0];
    });

    $scope.setImage = function(imageUrl) {
        $scope.mainImageUrl = imageUrl;
    };
}]);
```

phone-detail.html中，把大图的`ngSrc`指令绑定到`mainImageUrl`属性上。同时注册一个`ngClick`指令到缩略图上。当一个用户点击缩略图的任意一个时，这个处理器会使用`setImage`事件处理函数来把`mainImageUrl`属性设置成选定缩略图的URL

```html
<img ng-src="{{mainImageUrl}}" class="phone">

<ul class="phone-thumbs">
    <li ng-repeat="img in phone.images">
        <img ng-src="{{img}}" ng-click="setImage(img)">
    </li>
</ul>
```
