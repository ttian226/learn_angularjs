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

#### 执行期

下面的例子解释了AngularJS如何和浏览器的事件回路(event loop)交互。

1. 浏览器的事件循环等待事件的触发。所谓事件包括用户的交互操作、定时事件、或者网络事件(服务器的响应)。
2. 事件触发后，回调会被执行。此时会进入Javascript上下文。通常回调可以用来修改DOM结构。
3. 一旦回调执行完毕，浏览器就会离开Javascript上下文，并且根据DOM的修改重新渲染视图。

AngularJS通过使用自己的事件处理循环，改变了传统的Javascript工作流。这使得Javascript的执行被分成原始部分和拥有AngularJS执行上下文的部分。只有在AngularJS执行上下文中运行的操作，才能享受到AngularJS提供的数据绑定，异常处理，资源管理等功能和服务。你可以使用`$apply()`来从普通Javascript上下文进入AngularJS执行上下文。记住，大部分情况下（如在控制器，服务中），`$apply`都已经被用来处理当前事件的相应指令执行过了。只有当你使用自定义的事件回调或者是使用第三方类库的回调时，才需要自己执行`$apply`。

1. 通过调用`scope.$apply(stimulusFn)`来进入AngularJS的执行上下文，这里的`stimulusFn`是你希望在AngularJS执行上下文中执行的函数。
2. AngularJS会执行`stimulusFn()`，这个函数一般会改变应用的状态。
3. AngularJS进入`$digest`循环。这个循环是由两个小循环组成的，这两个小循环用来处理处理`$evalAsync`队列和`$watch`列表。这个`$digest`循环直到模型“稳定”前会一直迭代。这个稳定具体指的是`$evalAsync`对表为空，并且`$watch`列表中检测不到任何改变了。
4. 这个`$evalAsync`队列是用来管理那些“视图渲染前需要在当前栈框架外执行的操作的”。这通常使用`setTimeout(0)`来完成的。用`setTimeout(0)`会有速度慢的问题。并且，因为浏览器是根据事件队列按顺序渲染视图的，还会造成视图的抖动。
5. `$watch`列表是一个表达式的集合，这些表达式可能是自上次迭代后发生了改变的。如果有检测到了有改变，那么`$watch`函数就会被调用，它通常会把新的值更新到DOM中。
6. 一旦AngularJS的`$digest`循环结束，整个执行就会离开AngularJS和Javascript的上下文。这些都是在浏览器为数据改变而进行重渲染之后进行的。
下面解释了"hello world"的例子是怎么样实现“将用户输入绑定到视图上”的效果。

```html
<!DOCTYPE html>
<html lang="en" ng-app>
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
<input ng-model="name"/>
<p>hello {{name}}</p>
<script src="angular.min.js"></script>
</body>
</html>
```

* 在编译阶段：
 1. `<input>`上的`ng-model`指令给`<input>`输入框绑定了`keydown`事件；
 2. `{{name}}`这个变量替换表单式建立了一个`$watch`来接受`name`变量改变的通知。

* 在执行期阶段：
 1. 按下任何一个键(以X键为例)，都会触发一个`<input>`输入框的`keydown`事件；
 2. `<input>`上的指令捕捉到`<input>`里值得改变，然后调用`$apply("name = 'X'")`来更新处于AngularJS执行上下文中的模型；
 3. AngularJS将`name='X'`应用到模型上；
 4. `$digest`循环开始；
 5. `$watch`列表检测到了`name`值的变化，然后通知`{{name}}`变量替换的表达式，这个表达式负责将DOM进行更新；
 6. AngularJS退出执行上下文，然后退出Javascript上下文中的`keydown`事件；
 7. 浏览器以更新的文本重渲染视图。
