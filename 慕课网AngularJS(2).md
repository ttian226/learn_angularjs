#### 模块定义

AngularJS实现模块化步骤：
* 用`angular`这个全局对象的`module`方法去定义一个模块
* 在这个`module`的实例上去调用`controller`方法来创建`helloAngular`的控制器
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

* framework存放angularjs以及第三方库如bootstrap等。
* tpls目录存放模板文件（html片段）
* js目录存放js文件，app.js作为启动点的js
* index.html应用的主html文件(包含`<div ng-view></div>`,根容器)

#### 路由

本质上来说路由就是根据地址栏里的url的不同，去展现不同的视图。这个视图是由控制器去负责生成的。不同的视图有不同的控制器去处理

#### ng官方推荐的模块切分方式

![Image](https://github.com/ttian226/learn_angularjs/blob/master/imgs/ng-modules.png)

* 任何一个ng应用都是由控制器，指令，服务，路由，过滤器等有限的模块类型构成的
* 控制器，指令，服务，路由，过滤器分别放在一个模块里（可借助grunt合并）
* 用一个总的app模块作为入口点，它依赖其它所有模块

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

