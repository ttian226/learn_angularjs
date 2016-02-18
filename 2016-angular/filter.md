#### 过滤ngRepeat的列表

```html
<body ng-app="app" ng-controller="TestCtrl as test">
  <input type="text" ng-model="search">
  <p ng-repeat="person in test.people | filter:search">
    {{person.name}}
  </p>
</body>
```

```javascript
angular.module('app', [])
    .controller('TestCtrl', function () {
      var self = this;
      self.people = [
        {
          name: "Eric Simons",
          born: "Chicago"
        },
        {
          name: "Albert Pai",
          born: "Taiwan"
        },
        {
          name: "Matthew Greenster",
          born: "Virginia"
        }
      ];
    });
```

[完整代码](http://plnkr.co/edit/SLJIvKxek9fvZiHdTGtx)

只过滤name:

```html
<input type="text" ng-model="search.name">
```

过滤所有字段（name和born）:

```html
<input type="text" ng-model="search.$" />
```

#### 自定义过滤器

```html
<body ng-app="app" ng-controller="TestCtrl as test">
  <input type="text" ng-model="test.myString">
  <h2>
    {{test.myString | capitalize}}
  </h2>
</body>
```

```javascript
angular.module('app', [])
    .controller('TestCtrl', function () {
      var self = this;
      self.myString = 'hello world';
      
    })
    .filter('capitalize', function () {
      return function (text) {
        return text.toUpperCase();
      };
    });
```

[完整代码](http://plnkr.co/edit/YSTV6CtSkn8xYEq8LwP2)

