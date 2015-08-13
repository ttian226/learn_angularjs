#### 加载Angular.js

通常将加载`angular.js`的script标签放在页面的底部，这样可以缩短应用加载的时间，因为这样HTML的加载不会被`angular.js`脚本的加载阻塞。

请将`ng-app`指令放到你的应用的标签根节点中, 如果你想要AngularJS自动执行整个`<html>`程序就把它放在`<html>`标签中。

```html
<html ng-app>
```

#### 自动初始化

AngularJS会在DOMContentLoaded事件触发时执行，并通过`ng-app`指令 寻找你的应用根作用域。如果`ng-app`指令找到了，那么AngularJS将会：
1. 载入和指令内容相关的模块。
2. 创建一个应用的注入器(injector)。

#### 手动初始化

如果你需要主动控制一下初始化的过程，你可以使用手动执行引导程序的方法。比如当你使用“脚本加载器(script loader)”，或者需要在AngularJS编译页面之前做一些操作，你就会用到它了。

下面代码演示了如何使用手动初始化，它的效果等同于使用`ng-app`指令 。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
    Hello {{'World!'}}
<script src="angular.min.js"></script>
<script>
    angular.element(document).ready(function() {
        angular.bootstrap(document);
    });
</script>
</body>
</html>
```
1. 等页面和所有的脚本加载完之后，找到HTML模板的根节点——通常就是文档的根节点。
2. 调用`api/angular.bootstrap`将模板编译成可执行的、数据双向绑定的应用程序。
