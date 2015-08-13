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

#### Html编辑器

##### 概览

AngularJS的HTML编译器能让浏览器识别新的HTML语法。它能让你将行为关联到HTML元素或者属性上，甚至能让你创造具有自定义行为的新元素。AngularJS称这种行为扩展为“指令”

HTML在编写静态页面时，有很多声明式的结构来控制格式。比如你要把某个内容居中，你不必告诉浏览器“去找到窗口的中点位置，然后跟内容的中间结合”。你只需要添加一个 align="center" 的属性给需要内容居中的元素就行了。这就是声明式语言的强大之处。

但是声明式语言也有力所不能及的地方，原因之一在于你不能用它来让浏览器识别新的语法。比如说，你不要内容居中，而是居左到1/3,这时它就做不到了。所以我们需要一个办法让浏览器能学会新的HTML语法。

AngularJS生来自带一些对创建APP非常有用的指令。我们也希望你能自己创造一些对你自己的应用有用的指令。这些扩展的指令就是你创建APP的 **特定领域语言（Domain Specific Language**

编译的过程都会在浏览器端发生；服务器端不会参与到其中的任何步骤，也不会做预编译。

##### 编译器

编译器是AngularJS提供的一项服务，它通过遍历DOM来查找和它相关的属性。整个编译的过程分为两个阶段。
* 编译： 遍历DOM并且收集所有的相关指令，生成一个链接函数。
* 链接： 给指令绑定一个作用域，生成一个动态的视图。作用域模型的任何改变都会反映到视图上，并且视图上的任何用户操作也都会反映到作用域模型。这使得作用域模型成为你的业务逻辑里唯一要关心的东西。

有一些指令，比如ng-repeat会为数据集合里的每一项DOM元素都克隆一次。将整个编译过程分为编译和链接两个阶段的作法改善了整体的性能，因为克隆出来的模板总共只需要被编译一次，然后链接到各自的模型实例上就行了

##### 指令

指令指示的是“当关联的HTML结构进入编译阶段时应该执行的操作”。指令可以写在元素的名称里，属性里，css类名里，注释里。下面有几个功能相同的使用ng-bind指令的例子。

```html
<span ng-bind="exp"></span>
<span class="ng-bind: exp;"></span>
<ng-bind></ng-bind>
<!-- directive: ng-bind exp -->
```

指令本质上只是一个当编译器编译到相关DOM时需要执行的函数。

一个例子：给`<span>`加上`draggable`指令

```html
<!DOCTYPE html>
<html lang="en" ng-app="drag">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
<span draggable>Drag Me</span>
<script src="angular.min.js"></script>
<script src="drag.js"></script>
</body>
</html>
```

drag.js中定义`draggable`指令:

```javascript
var module = angular.module('drag', []);
module.directive('draggable', function ($document) {
    var startX = 0, startY = 0, x = 0, y = 0;

    return function (scope, element, attr) {
        // 这里element是span元素，给span元素初始化样式
        element.css({
            position: 'relative',
            border: '1px solid red',
            backgroundColor: 'lightgrey',
            cursor: 'pointer'
        });

        // 给span元素绑定mousedown事件，当鼠标点击span元素时触发
        element.bind('mousedown', function (event) {
            startX = event.screenX - x;
            startY = event.screenY - y;
            
            // 当mousedown事件触发时，给document绑定mousemove和mouseup事件
            $document.bind('mousemove', mousemove);
            $document.bind('mouseup', mouseup);
        });

        // 根据移动的偏移实时设置span元素的top，left
        function mousemove(event) {
            y = event.screenY - startY;
            x = event.screenX - startX;
            element.css({
                top: y + 'px',
                left: x + 'px'
            });
        }

        // 鼠标抬起时移除事件
        function mouseup() {
            $document.unbind('mousemove', mousemove);
            $document.unbind('mouseup', mouseup);
        }
    };
});
```
