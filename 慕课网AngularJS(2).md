#### 双向数据绑定

##### 使用ng-bind指令

之前的例子，使用`{{}}`取值表达式时，当页面刷新快的时候页面上会显示出`{greeting.text}}`这样的文字。

```html
<div ng-controller="helloAngular">
    <p>{{greeting.text}},AngularJS</p>
</div>
```

使用`ng-bind`指令则不会出现这样的问题，其它代码都不变，只改变`<p>`节点

```html
<div ng-controller="helloAngular">
    <p><span ng-bind="greeting.text"></span>,AngularJS</p>
</div>
```

