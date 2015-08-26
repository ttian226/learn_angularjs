#### 指令之间的交互

```html
<div class="row">
    <div class="col-md-3">
        <superman strength>动感超人---力量</superman>
    </div>
</div>
<div class="row">
    <div class="col-md-3">
        <superman strength speed>动感超人2---力量+敏捷</superman>
    </div>
</div>
<div class="row">
    <div class="col-md-3">
        <superman strength speed light>动感超人3---力量+敏捷+发光</superman>
    </div>
</div>
```

这个例子创建了4个指令：

1. 标签`superman`
2. 属性`strength`
3. 属性`speed`
4. 属性`light`

关于`directive`中的scope，是创建独立的作用域。
关于`directive`中的`controller`，和mvc中的`controller`不同，是指令内部的`controller`，给指令暴露出一组Public方法，给外部去调用的。

AngularJS内置jQueryLite(简化版的jQuery，语法和jQuery相同，link中的`addClass`,`bind`就是jQueryLite提供的)

```javascript
var myModule = angular.module('MyModule', []);

myModule.directive('superman', function() {
    return {
        scope: {},
        restrict: 'AE',
        controller: function($scope) {
            $scope.abilities = [];
            this.addStrength = function() {
                $scope.abilities.push('strength');
            };
            this.addSpeed = function() {
                $scope.abilities.push('speed');
            };
            this.addLight = function() {
                $scope.abilities.push('light');
            };
        },
        link: function(scope, element, attrs) {
            // 引入样式（bootstrap）
            element.addClass('btn btn-primary');

            // 绑定一个mouseenter事件
            element.bind('mouseenter', function() {
                console.log(scope.abilities);
            });
        }
    };
});

myModule.directive('strength', function() {
    return {
        require: '^superman',
        link: function(scope, element, attrs, supermanCtrl) {
            supermanCtrl.addStrength();
        }
    };
});

myModule.directive('speed', function() {
    return {
        require: '^superman',
        link: function(scope, element, attrs, supermanCtrl) {
            supermanCtrl.addSpeed();
        }
    };
});

myModule.directive('light', function() {
    return {
        require: '^superman',
        link: function(scope, element, attrs, supermanCtrl) {
            supermanCtrl.addLight();
        }
    };
});
```

什么时候写在`controller`里，什么时候写在`link`里？如果想让你的一些方法暴露给外部调用，就写在`controller`里。`link`是处理指令内部的一些事务的。比如给元素绑定事件，绑定数据等。

关于`require`，比如`strength`指令是依赖于`superman`这个指令的。如果设置`require`后，`link`函数就可以指定第4个参数`supermanCtrl`。AngularJS在进行处理的时候，会把`supermanCtrl`注入到link函数中，这样就可以通过`supermanCtrl`调用`myModule`里暴露在控制器的方法了。

#### 独立scope

通过指令创建了4个input输入框，在任意的input框中输入时其它3个输入框也会同步显示。

```html
<hello></hello>
<hello></hello>
<hello></hello>
<hello></hello>
```

```javascript
var module = angular.module('MyModule', []);

module.directive('hello', function() {
    return {
        restrict: 'AE',
        template: '<div><input type="text" ng-model="username"/>{{username}}</div>',
        replace: true
    };
});
```

修改js，增加`scope: {}`，这样4个输入框的输入互不影响。这样每个指令都有自己独立的scope空间。

```javascript
var module = angular.module('MyModule', []);

module.directive('hello', function() {
    return {
        restrict: 'AE',
        scope: {},
        template: '<div><input type="text" ng-model="username"/>{{username}}</div>',
        replace: true
    };
});
```

scope的绑定策略：
    
    1. @，把当前属性作为字符串传递。你还可以绑定来自外层scope的值，在属性值中插入{{}}即可。
    2. =，与父scope中的属性进行双向绑定。
    3. &，传递一个来自父scope的函数，稍后调用。

例子：

```html
<div ng-controller="MyCtrl">
    <drink flavor="{{ctrlFlavor}}"></drink>
</div>
```

```javascript
var module = angular.module('MyModule', []);

module.controller('MyCtrl', ['$scope', function($scope) {
    // 初始化数据模型ctrlFlavor
    $scope.ctrlFlavor = 'BaiWei';
}]);

module.directive('drink', function() {
    return {
        restrict: 'AE',
        template: '<div>{{flavor}}</div>',
        link: function(scope, element, attrs) {
            // 给作用域的flavor赋值，attrs.flavor是在控制器中绑定的字符串'BaiWei'
            scope.flavor = attrs.flavor;
        },
        replace: true   //覆盖<drink>节点
    };
});
```

1. 控制器中给`<drink>`的属性`flavor`的数据模型`{{ctrlFlavor}}`初始化'BaiWei'
2. 指令中给作用域（当前模板）的flavor赋值，参数attrs是`<drink>`属性集合，attrs.flavor是在控制器中绑定的字符串'BaiWei'
3. 替换后实际上没有完全覆盖`<drink>`节点，是保留了所有的属性值，即flavor属性被保留了下来

上面的例子使用@实现：

```javascript
module.directive('drink', function() {
    return {
        restrict: 'AE',
        template: '<div>{{flavor}}</div>',
        scope: {
            flavor: '@' //作用域的数据模型flavor用<drink>节点的flavor属性值初始化
        },
        replace: true
    };
});
```

这里不使用`link`属性，而是用`scope`属性配置。`scope`中的`flavor: '@'`是给当前作用域（`<div>{{flavor}}</div>`）中的`flavor`绑定一个值，这个值就是`<drink>`节点的`flavor`属性值。（要和`flavor`同名的属性值，如果`<drink>`的属性为`test`则不行）
