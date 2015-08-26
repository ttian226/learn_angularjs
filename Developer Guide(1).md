#### controller

controller只能做两件事：

    1. 初始化$scope对象
    2. 给$scope对象添加行为

不要在controller中做下面这些事：

    1. 操作Dom，基本都可以通过数据绑定和指令系统来代替
    2. 格式化input，用angular form controls代替
    3. 过滤output，用angular filter代替
    4. 跨控制器传递数据，用angular service代替

#### 初始化$scope对象

```html
<div ng-controller="GreetingController">
  {{ greeting }}
</div>
```

```javascript
var myApp = angular.module('myApp',[]);

myApp.controller('GreetingController', ['$scope', function($scope) {
  $scope.greeting = 'Hola!';
}]);
```

#### 给$scope对象添加行为

为了在view中添加事件或者做些计算，我们必须给scope添加行为。我们通过给`$scope`对象添加方法来实现。这些方法在template/view中是可调用的。

下面的例子在controller中添加一个方法`double`

```javascript
var myApp = angular.module('myApp',[]);

myApp.controller('DoubleController', ['$scope', function($scope) {
  $scope.double = function(value) { return value * 2; };
}]);
```

一旦controller被绑定到dom上，`double`方法就可以在模板上的表达式里使用

```html
<div ng-controller="DoubleController">
  Two times <input ng-model="num"> equals {{ double(num) }}
</div>
```

#### 正确的使用控制器

通常来说控制器里不要做太多的事，它应仅仅包括视图中所需的业务逻辑

#### 一个简单的controller的例子

我们创建了下面3个组件：

    1. 一个template带两个button和一个简单的消息
    2. 一个model，名为spice的字符串
    3. 一个controller，包含两个方法分别设置了spice的值

```html
<div ng-controller="SpicyController">
 <button ng-click="chiliSpicy()">Chili</button>
 <button ng-click="jalapenoSpicy()">Jalapeño</button>
 <p>The food is {{spice}} spicy!</p>
</div>
```

```javascript
var myApp = angular.module('spicyApp1', []);

myApp.controller('SpicyController', ['$scope', function($scope) {
    $scope.spice = 'very';

    $scope.chiliSpicy = function() {
        $scope.spice = 'chili';
    };

    $scope.jalapenoSpicy = function() {
        $scope.spice = 'jalapeño';
    };
}]);
```

通过上面的例子可以看到：

    1. ng-controller指令隐式的为模板创建了一个作用域，这个作用域通过SpicyController来管理。
    2. SpicyController是一个纯粹的javascript函数
    3. 给$scope赋予属性值来创建或更新数据模型
    4. 控制器里的方法可以被通过直接赋予$scope属性来创建，如上面的chiliSpicy方法
    5. controller里的属性和方法在模板中是可以被直接访问的
