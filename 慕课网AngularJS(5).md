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


