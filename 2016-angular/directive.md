#### directive的四种类型

```html
<my-dir></my-dir>   //作为标签名
<span my-dir="exp"></span>  //作为属性名
<!-- directive: my-dir exp -->  //注释
<span class="my-dir: exp;"></span>  //类名
```

**推荐使用标签名和属性名的指令**


#### 模板directive

例子一：使用template定义模板

```html
<body ng-app="docsSimpleDirective">
    <div ng-controller="Controller">
      <div my-customer></div>
    </div>
  </body>
```

```javascript
angular.module('docsSimpleDirective', [])
    .controller('Controller', ['$scope', function ($scope) {
      $scope.customer = {
        name: 'Naomi',
        address: '1600 Amphitheatre'
      };
    }])
    .directive('myCustomer', function () {
      return {
        template: 'Name: {{customer.name}} Address: {{customer.address}}'
      };
    });
```

**通常来说如果模板内容比较少的话使用`template`，如果模板内容多则使用`templateUrl`**

[完整代码](http://plnkr.co/edit/CbPuc8oXKufFKN4Zi9KA)


例子二：使用templateUrl定义模板

html同例子一相同。

```javascript
angular.module('docsTemplateUrlDirective', [])
    .controller('Controller', ['$scope', function ($scope) {
      $scope.customer = {
        name: 'Naomi',
        address: '1600 Amphitheatre'
      };
    }])
    .directive('myCustomer', function () {
      return {
        templateUrl: 'my-customer.html'
      };
    });
```

[完整代码](http://plnkr.co/edit/ETxYybPXEL3LhFbKplx9)
