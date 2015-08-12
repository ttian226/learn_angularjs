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

