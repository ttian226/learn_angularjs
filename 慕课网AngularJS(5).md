#### 路由

* Ajax请求不会留下History记录
* 用户无法直接通过URL进入应用中的指定页面（保存书签，链接分享等）
* Ajax无法实现SEO


以BookStore代码为例：$routeProvider是AngularJS提供的一个路由机制，通过它来实现路由的配置。前提需要引入`angular-route.js`

```javascript
bookStoreApp.config(function($routeProvider) {
    $routeProvider.when('/hello', {
        templateUrl: 'tpls/hello.html',
        controller: 'HelloCtrl'
    }).when('/list', {
        templateUrl: 'tpls/bookList.html',
        controller: 'BookListCtrl'
    }).otherwise({
        redirectTo: '/hello'
    });
});
```

这种路由机制的缺点，无法实现深层次嵌套的路由，AngularUI Router可以解决这个问题


前端路由的基本原理：
* 哈希`#`，以前是锚点，不用刷新页面跳转，即页内导航。
* HTML5中提供的新的history API。通过js代码修改浏览器地址栏里的地址，浏览器会留下历史记录，但是页面不会跳转
* 路由的核心是给应用定义**状态**
。和传统的开发方式会有些思路上的不同。要预先把状态定义好，要通过哪个地址进入哪种状态，会影响整个页面的编码的方式

#### 指令

```html
<!doctype html>
<html ng-app="MyModule">
<head>
    <meta charset="utf-8">
</head>
<body>
<hello></hello>
</body>
<script src="angular.min.js"></script>
<script src="controller.js"></script>
</html>
```

```javascript
var myModule = angular.module('MyModule', []);

myModule.directive('hello', function() {
    return {
        restrict: 'E',
        template: '<div>Hi everyone!</div>',
        replace: true
    };
});
```

##### restrict

* `restrict`为匹配模式，一共有4个选项分别为A（属性）,E（元素）,M（注释）,C（样式类）

*作为属性匹配*

```html
<div hello></div>
```

```javascript
myModule.directive('hello', function() {
    return {
        restrict: 'A',
        template: '<div>Hi everyone!</div>',
        replace: true
    };
});
```

*作为元素匹配*

```html
<div></div>
```

```javascript
myModule.directive('hello', function() {
    return {
        restrict: 'E',
        template: '<div>Hi everyone!</div>',
        replace: true
    };
});
```

*作为样式类匹配*

```html
<div class='hello'></div>
```

```javascript
myModule.directive('hello', function() {
    return {
        restrict: 'C',
        template: '<div>Hi everyone!</div>',
        replace: true
    };
});
```

*作为注释匹配*

```html
<!-- directive:hello -->
```

```javascript
myModule.directive('hello', function() {
    return {
        restrict: 'M',
        template: '<div>Hi everyone!</div>',
        replace: true
    };
});
```

以上四种匹配在页面上都显示'Hi everyone!'

AngularJS默认是使用A(属性)匹配。A(属性)和E(元素)两种模式是最常用的。
* 推荐使用元素和属性的方式使用指令。
* 当需要创建带有自己的模板的指令时，使用元素名称的方式创建指令。
* 当需要为已有的HTML标签增加功能时，使用属性的方式创建指令。

##### templateUrl

配置项`template`为最终要显示出来的html标签。显然使用模板的方式编写出的html会比较少，如果需要编写大量的html时，写在`template`会很不方便而且不利于维护。这时需要写在`templateUrl`这个配置项里。

例如：把模板切成一个独立的html来编写，即不需要在js里进行拼写，并且运行效率也不高。而是直接写在html中，如hello.html

```javascript
myModule.directive('hello', function() {
    return {
        restrict: 'E',
        templateUrl: 'hello.html',
        replace: true
    };
});
```

##### $templateCache

当我们的模板不仅仅在'hello'这个指令中使用，也想在其它指令中使用。我们希望AngularJS帮它给缓存起来。

```html
<hello></hello>
```

run方法：当注射器加载完成所有模块时，此方法执行一次。用AngularJS内置的`$templateCache`的`put`方法把模板给缓存起来。在使用模板时时，使用get方法。

```javascript
var myModule = angular.module('MyModule', []);

// 当注射器加载完成时执行，此方法执行一次
myModule.run(function($templateCache) {
    // 缓存模板
    $templateCache.put('hello.html', '<div>Hello everyone!!!!</div>')
});

myModule.directive('hello', function($templateCache) {
    return {
        restrict: 'E',
        // 使用模板
        template: $templateCache.get('hello.html'),
        replace: true
    };
});
```

##### replace和transclude

在`<hello>`标签中添加一个`<div>`

```html
<hello>
    <div>这里是指令的内部内容</div>
</hello>
```

配置项中，如果使用replace这种方式，会把里面div的内容给替换掉

```javascript
myModule.directive('hello', function($templateCache) {
    return {
        restrict: 'E',
        template: '<div>hello everyone!</div>',
        replace: true
    };
});
```

配置项中，使用transclude这种方式可以保存被嵌套的元素，在模板中配置`<div ng-transclude></div>`，这里会被替换成`<hello>`内部嵌套的内容。

```javascript
myModule.directive('hello', function($templateCache) {
    return {
        restrict: 'E',
        template: '<div>hello everyone!<div ng-transclude></div></div>',
        transclude: true
    };
});
```

*transclude*是一个非常重要的配置项。通过`transclude`可以让指令嵌套的使用。


#### 指令执行的大概机制

3个阶段：

    1. 加载阶段。加载angularjs，找到ng-app指令，确定应用的边界。
    2. 编译阶段。遍历Dom，查找所有的指令，缓存到内部；根据指令代码中的template,replace,transclude转换Dom结构；如果存在compile函数则调用
    3. 链接阶段。每条指令的link函数被调用

AngularJS在指令里操作Dom，在指令的link方法中操作Dom
