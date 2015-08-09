#### Hello World

*使用的版本是：AngularJS v1.4.3*

```html
<!DOCTYPE html>
<html ng-app>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="angular.min.js"></script>
</head>
<body>
    Hello {{'World'}}
</body>
</html>
```

* 使用`ng-app`，告诉浏览器使用angular处理整个html页面
* 载入angular.min.js文件
* 双大括号中`{{}}`是绑定的表达式`'World'`。
* *发现`{{'World'}}`里面的字符串要用引号引起来，如果不加引号会把`{{}}`也输出来*
* `{{}}`其实是一个取值表达式。


```html
<body>
    Your name: <input type="text" ng-model="yourname" placeholder="World"/>
    <br/>
    Hello {{yourname || 'World'}}
</body>
```

* `<input ng-model="yourname"/>`绑定一个模型变量`yourname`。（暂时理解为变量）
* 将`yourname`模型变量加入到`{{}}`中。
* *发现一定要用`||`设置默认的字符串，使用`|`或去掉`||`都不行，替换成`+`可以，但是它是一个连接符*

双向数据绑定：
1. 输入框中的任何修改都会立即反应到模型变量`yourname`上。一个方向。
2. 模型变量`yourname`的任何更改都会反应到html上的文本上。另一个方向。
*这两个方向不是相互的*

#### `ng-app`指令

```html
<html lang="en" ng-app>
```
`ng-app`指令标记了AngularJS脚本的作用域，在`<html>`中添加`ng-app`属性即说明整个`<html>`都是AngularJS脚本作用域。开发者也可以在局部使用`ng-app`指令，如`<div ng-app>`，则AngularJS脚本仅在该`<div>`中运行。


#### AngularJS脚本标签

```html
<script src="angular.min.js"></script>
```
这行代码载入angular.js脚本，当浏览器将整个HTML页面载入完毕后将会执行该angular.js脚本，angular.js脚本运行后将会寻找含有`ng-app`指令的HTML标签，该标签即定义了AngularJS应用的作用域。


#### 双大括号绑定的表达式

```html
<p>Nothing here {{'yet' + '!'}}</p>
```
这行代码演示了AngularJS模板的核心功能——绑定，这个绑定由双大括号`{{}}`和表达式`'yet' + '!'`组成。
这个绑定告诉AngularJS需要运算其中的表达式并将结果插入DOM中，接下来的步骤我们将看到，DOM可以随着表达式运算结果的改变而实时更新。
AngularJS表达式是一种类似于JavaScript的代码片段，AngularJS表达式仅在AngularJS的作用域中运行，而不是在整个DOM中运行。

#### 引导AngularJS应用

通过ngApp指令来自动引导AngularJS应用是一种简洁的方式，适合大多数情况
