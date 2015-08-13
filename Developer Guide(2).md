#### 启动

下面解释了我们是如何把这一切运转起来的：

1. 浏览器载入HTML，然后把它解析成DOM。
2. 浏览器载入angular.js脚本。
3. AngularJS等到`DOMContentLoaded`事件触发。
4. AngularJS寻找`ng-app`指令，这个指令指示了应用的边界。
5. 使用`ng-app`中指定的模块来配置注入器(`$injector`)。
6. 注入器(`$injector`)是用来创建“编译服务(`$compile service`)”和“根作用域(`$rootScope`)”的。
7. 编译服务(`$compile service`)是用来编译DOM并把它链接到根作用域(`$rootScope`)的。
8. `ng-init`指令将`World`赋给作用域里的`name`这个变量。
9. 通过`{{name}}`的替换，整个表达式变成了`Hello World`

```html
<!DOCTYPE html>
<html lang="en" ng-app>
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
<p ng-init="name='world'">hello {{name}}</p>
<script src="angular.min.js"></script>
</body>
</html>
```

