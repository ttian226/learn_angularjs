### 过滤器

app.js给主模块增加一个依赖模块`phonecatFilters`（过滤器模块）

```javascript
var mainModule = angular.module('phonecatApp', [
    'ngRoute',
    'phonecatController',
    'phonecatFilters'
]);
```

在filters.js中，首先定义过滤模块。然后调用这个模块的实例方法`filter`定义过滤器，第一个参数为过滤器的名字`checkmark`，第二个参数是过滤函数它是一个匿名函数。

```javascript
// 定义过滤模块
var filterModule = angular.module('phonecatFilters', []);

// 调用模块的实例方法filter添加过滤器。过滤器的名字为checkmark
filterModule.filter('checkmark', function() {
    // 匿名函数返回一个函数，参数为html中对应的数据模型
    return function(input) {
        // 当input为ture时返回'\u2713'（√），为false时返回'\u2718'（×）
        return input ? '\u2713' : '\u2718';
    };
});
```

在AngularJS模板中使用的过滤器语法为:
`{{ expression | filter }}`

在模板phone-detail.html中添加了过滤器`checkmark`

```html
<img ng-src="{{phone.images[0]}}" class="phone">

<h1>{{phone.name}}</h1>

<p>{{phone.description}}</p>

<ul class="phone-thumbs">
    <li ng-repeat="img in phone.images">
        <img ng-src="{{img}}">
    </li>
</ul>

<ul class="specs">
    <li>
        <span>Availability and Networks</span>
        <dl>
            <dt>Availability</dt>
            <dd ng-repeat="availability in phone.availability">{{availability}}</dd>
        </dl>
    </li>
    <li>
        <span>Battery</span>
        <dl>
            <dt>Type</dt>
            <dd>{{phone.battery.type}}</dd>
            <dt>Talk Time</dt>
            <dd>{{phone.battery.talkTime}}</dd>
            <dt>Standby time (max)</dt>
            <dd>{{phone.battery.standbyTime}}</dd>
        </dl>
    </li>
    <li>
        <span>Storage and Memory</span>
        <dl>
            <dt>RAM</dt>
            <dd>{{phone.storage.ram}}</dd>
            <dt>Internal Storage</dt>
            <dd>{{phone.storage.flash}}</dd>
        </dl>
    </li>
    <li>
        <span>Connectivity</span>
        <dl>
            <dt>Network Support</dt>
            <dd>{{phone.connectivity.cell}}</dd>
            <dt>WiFi</dt>
            <dd>{{phone.connectivity.wifi}}</dd>
            <dt>Bluetooth</dt>
            <dd>{{phone.connectivity.bluetooth}}</dd>
            <dt>Infrared</dt>
            <dd>{{phone.connectivity.infrared | checkmark}}</dd>
            <dt>GPS</dt>
            <dd>{{phone.connectivity.gps | checkmark}}</dd>
        </dl>
    </li>
    <li>
        <span>Android</span>
        <dl>
            <dt>OS Version</dt>
            <dd>{{phone.android.os}}</dd>
            <dt>UI</dt>
            <dd>{{phone.android.ui}}</dd>
        </dl>
    </li>
    <li>
        <span>Size and Weight</span>
        <dl>
            <dt>Dimensions</dt>
            <dd ng-repeat="dim in phone.sizeAndWeight.dimensions">{{dim}}</dd>
            <dt>Weight</dt>
            <dd>{{phone.sizeAndWeight.weight}}</dd>
        </dl>
    </li>
    <li>
        <span>Display</span>
        <dl>
            <dt>Screen size</dt>
            <dd>{{phone.display.screenSize}}</dd>
            <dt>Screen resolution</dt>
            <dd>{{phone.display.screenResolution}}</dd>
            <dt>Touch screen</dt>
            <dd>{{phone.display.touchScreen | checkmark}}</dd>
        </dl>
    </li>
    <li>
        <span>Hardware</span>
        <dl>
            <dt>CPU</dt>
            <dd>{{phone.hardware.cpu}}</dd>
            <dt>USB</dt>
            <dd>{{phone.hardware.usb}}</dd>
            <dt>Audio / headphone jack</dt>
            <dd>{{phone.hardware.audioJack}}</dd>
            <dt>FM Radio</dt>
            <dd>{{phone.hardware.fmRadio | checkmark}}</dd>
            <dt>Accelerometer</dt>
            <dd>{{phone.hardware.accelerometer | checkmark}}</dd>
        </dl>
    </li>
    <li>
        <span>Camera</span>
        <dl>
            <dt>Primary</dt>
            <dd>{{phone.camera.primary}}</dd>
            <dt>Features</dt>
            <dd>{{phone.camera.features.join(', ')}}</dd>
        </dl>
    </li>
    <li>
        <span>Additional Features</span>
        <dd>{{phone.additionalFeatures}}</dd>
    </li>
</ul>
```
