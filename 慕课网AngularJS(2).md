#### 模块定义

AngularJS实现模块化步骤：
* 用angular这个全局对象的module方法去定义一个模块
* 在这个module的实例上去调用controller方法来创建helloAngular的控制器
* 根据官方的定义，AngularJS中的模块是一个集合，是由模型，视图，控制器，过滤器，服务等组合到一起，实现某一个功能。

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

#### 目录结构

![Image](https://github.com/ttian226/learn_angularjs/blob/master/imgs/ng-file-list.png)

#### 双向数据绑定

##### 使用ng-bind指令

之前的例子，使用`{{}}`取值表达式时，当页面刷新快的时候页面上会显示出`{greeting.text}}`这样的文字。

```html
<div ng-controller="helloAngular">
    <p>{{greeting.text}},AngularJS</p>
</div>
```

使用`ng-bind`指令则不会出现这样的问题，其它代码都不变，只改变`<p>`节点

```html
<div ng-controller="helloAngular">
    <p><span ng-bind="greeting.text"></span>,AngularJS</p>
</div>
```

