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
