#### 使用AngularJS改变样式

使用jQuery
*直接拿到dom节点，给节点改变class属性*

```css
.text-red {
    background-color: #ff0000;
}
.text-green {
    background-color: #00ff00;
}
```

```html
<div>
    <p class="text-red" id="text">测试CSS样式</p>
    <button class="btn btn-default">绿色</button>
</div>
```

```javascript
var $btn = $('.btn-default'),
    $text = $('#text');

$btn.on('click', function(e) {
    $text.removeClass('text-red').addClass('text-green');
});
```

使用AngularJS
*通过变量的方式来控制css。通过数据模型的变化来引导视图的变化，而不是直接操作视图。*

```html
<div ng-controller="CSSCtrl">
    <p class="text-{{color}}">测试CSS样式</p>
    <button class="btn btn-default" ng-click="setGreen()">绿色</button>
</div>
```

```javascript
var module = angular.module('MyCSSModule', []);

module.controller('CSSCtrl', ['$scope', function($scope) {
    $scope.color = 'red';
    $scope.setGreen = function() {
        $scope.color = 'green';
    };
}]);
```

#### ngClass

使用`ngClass`指令动态改变样式。`ng-class`接收一个表达式`{error: isError, warning: isWarning}`。当`isError`为true时`class='error'`。当`isWarning`为true时`class='warning'`

```css
.error {
    background-color: red;
}
.warning {
    background-color: yellow;
}
```

```html
<div ng-controller="HeaderController">
    <div ng-class="{error: isError, warning: isWarning}">{{messageText}}</div>
    <button ng-click="showError()">Simulate Error</button>
    <button ng-click="showWarning()">Simulate Warning</button>
</div>
```

```javascript
var module = angular.module('MyCSSModule', []);

module.controller('HeaderController', ['$scope', function($scope) {
    // 默认两个样式都不显示
    $scope.isError = false;
    $scope.isWarning = false;

    // 当点击按钮时，动态改变文本和样式
    $scope.showError = function() {
        $scope.messageText = 'This is an error!';
        $scope.isError = true;
        $scope.isWarning = false;
    };
    $scope.showWarning = function() {
        $scope.messageText = 'Just a warning. Please carry on.';
        $scope.isWarning = true;
        $scope.isError = false;
    };
}]);
```


