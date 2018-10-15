# AngularJS

AngularJS 一个 JavaScript MVC 框架，AngularJS 通过**指令**扩展了 HTML，通过**表达式**绑定数据到 HTML 页面上。

**AngularJS 特点**：

- AngularJS 把应用程序数据绑定到 HTML 元素。
- AngularJS 可以克隆和重复 HTML 元素。
- AngularJS 可以隐藏和显示 HTML 元素。
- AngularJS 可以在 HTML 元素"背后"添加代码。
- AngularJS 支持输入验证。

**引入 AngularJS**：

```html
<script src="https://cdn.staticfile.org/angular.js/1.4.6/angular.min.js"></script>
```

**AngularJS 指令**：

- ng-app：定义一个 AngularJS 应用。

- ng-model：将元素域值绑定到应用程序变量中。

- ng-bind：将应用程序数据绑定到 HTML 视图。

**AngularJS 表达式**：

AngularJS 表达式：`{{expression}}` ，将应用程序数据绑定到 HTML 页面。

**AngularJS 应用**：

AngularJS 模块：`ng-app` 

```js
var app = angular.module('myApp', []);
```

AngularJS 控制器：`ng-controller` 

```js
app.controller('myCtrl', function($scope) {
    $scope.firstName= "John";
    $scope.lastName= "Doe";
});
```

**AngularJS 实例**：

```html
<div ng-app="myApp" ng-controller="myCtrl">
 
名: <input type="text" ng-model="firstName"><br>
姓: <input type="text" ng-model="lastName"><br>
<br>
姓名: {{firstName + " " + lastName}}
 
</div>
 
<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.firstName= "John";
    $scope.lastName= "Doe";
});
</script>
```

## AngularJS 表达式

AngularJS 表达式：`{{expression}}` ，将应用程序数据绑定到 HTML 页面。

表达式内容：数字，字符串，数组，对象。

**AngularJS 表达式 与 JavaScript 表达式**：

- 类似于 JavaScript 表达式，AngularJS 表达式可以包含字母，操作符，变量。

- 与 JavaScript 表达式不同，AngularJS 表达式可以写在 HTML 中。

- 与 JavaScript 表达式不同，AngularJS 表达式不支持条件判断，循环及异常。

- 与 JavaScript 表达式不同，AngularJS 表达式支持过滤器。

## AngularJS 指令

- ng-app：定义一个 AngularJS 应用。
- ng-controller：定义一个控制器。
- ng-init：定义变量初始值。
- ng-model：将元素域值绑定到应用程序变量中。
- ng-bind：将应用程序数据绑定到 HTML 视图。

```html
<div ng-app="myApp" ng-controller="myCtroller">
    名: <input type="text" ng-model="firstName"><br>
    姓: <input type="text" ng-model="lastName"><br>
    <br>
    姓名: {{firstName + " " + lastName}}
</div>
```

- ng-repeat：重复一个 HTML 元素。

```html
<div ng-app="myApp" ng-init="names=[
{name:'Jani',country:'Norway'},
{name:'Hege',country:'Sweden'},
{name:'Kai',country:'Denmark'}]">
 
<p>循环对象：</p>
<ul>
  <li ng-repeat="x in names">
    {{ x.firstName + " " + x.lastName }}
  </li>
</ul>
</div>
```

## AngularJS 模型

ng-model：将输入域的值与 AngularJS 创建的变量绑定。

双向绑定：`ng-model` 指令在修改输入域的值时， AngularJS 属性的值也将修改。

应用状态：`ng-model` 指令可以为应用数据提供状态值(invalid, dirty, touched, error)。

- ng-invalid:未通过验证的表单

- ng-valid:布尔型属性，它指示表单是否通过验证。如果表单当前通过验证，他将为true

- ng-dirty:布尔值属性，表示用户是否修改了表单。如果为 ture，表示有修改过；false 表示修没有修改过

- ng-touched:布尔值属性，表示用户是否和控件进行过交互

- ng-pristine:布尔值属性，表示用户是否修改了表单。如果为ture，表示没有修改过；false表示修改过

`ng-model` 指令根据表单域的状态添加/移除以下类：

- `ng-valid`: 验证通过
- `ng-invalid`: 验证失败
- `ng-valid-[key]`: 由$setValidity添加的所有验证通过的值
- `ng-invalid-[key]`: 由$setValidity添加的所有验证失败的值
- `ng-pristine`: 控件为初始状态
- `ng-dirty`: 控件输入值已变更
- `ng-touched`: 控件已失去焦点
- `ng-untouched`: 控件未失去焦点
- `ng-pending`: 任何为满足$asyncValidators的情况

## AngularJS 作用域

`$scope`：作用域，作为控制器函数的参数传递。

`$rootScope`：根作用域（共享数据），作用于整个应用，可在各个 controller 中使用。

## AngularJS 控制器

自定义控制器（controller）：

```html
<div ng-app="myApp" ng-controller="myCtrl">
名: <input type="text" ng-model="firstName"><br>
姓: <input type="text" ng-model="lastName"><br>
<br>
姓名: {{firstName + " " + lastName}}<br>
姓名: {{fullName()}}
</div>

<script>
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope) {
        $scope.firstName = "John";
        $scope.lastName = "Doe";
        $scope.fullName = function() {
        return $scope.firstName + " " + $scope.lastName;
    }
    });
</script>
```

## AngularJS 过滤器

AngularJS 过滤器：使用 `|` 添加过滤器到表达式或指令中。

| 过滤器          | 描述                     |
| --------------- | ------------------------ |
| uppercase       | 格式化字符串为大写。     |
| lowercase       | 格式化字符串为小写。     |
| currency        | 格式化数字为货币格式。   |
| orderBy: 表达式 | 根据某个表达式排列数组。 |
| filter: 模型    | 从数组项中选择一个子集。 |

**表达式中添加过滤器**：

```html
{{ 表达式 | 过滤器 }}
```

**指令中添加过滤器**：

```css
ng-repeat="x in names | filter:test | orderBy:'age'"
```

**自定义过滤器**：

```js
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.msg = "Runoob";
});
app.filter('reverse', function() { //可以注入依赖
    return function(text) {
        return text.split("").reverse().join("");
    }
});
```

## AngularJS 服务

AngularJS 服务就是一个函数或对象。AngularJS 内建了30 多个服务。

- $location：返回当前页面 URL 地址。

- $http：服务向服务器发送请求，应用响应服务器传送过来的数据。

- $timeout：定时调用。

- $interval：周期调用。

### 自定义服务

创建自定义服务：

```js
app.service('hexafy', function() {
    this.myFunc = function (x) {
        return x.toString(16);
    }
});
```

使用自定义服务：

```js
app.controller('myCtrl', function($scope, hexafy) {
    $scope.hex = hexafy.myFunc(255);
});
```

```js
app.filter('myFormat',['hexafy', function(hexafy) {
    return function(x) {
        return hexafy.myFunc(x);
    };
}]);
```

### $http 服务

```js
// 简单的 GET 请求，可以改为 POST
$http({
    method: 'GET',
    url: '/someUrl'
}).then(function successCallback(response) {
        // 请求成功执行代码
    }, function errorCallback(response) {
        // 请求失败执行代码
});
```

GET 与 POST 简写：

```jsw
$http.get('/someUrl', config).then(successCallback, errorCallback);
```

```js
$http.post('/someUrl', data, config).then(successCallback, errorCallback);
```

## AngularJS 选择框

AngularJwS 可以使用 `ng-option` 指令创建一个下拉列表选项，列表项通过数组或对象循环输出。

```html
<select ng-model="selectedSite" ng-options="x.site for x in sites"></select>
```

使用对象 x 为键(key)，y 为值(value)：

```html
<select ng-model="selectedSite" ng-options="x for (x, y) in sites"></select>
```

## AngularJS 表格

使用 `ng-repeat` 指令绘制表格：

```html
<tr ng-repeat="x in names">
  <td>{{ x.Name }}</td>
  <td>{{ x.Country }}</td>
</tr>
```

使用过滤器：`ng-repeat="x in names | orderBy:'Country' "` 。

显示序号：`$index` （从0开始）。

判断序号奇偶：`$odd` 和 `$even` 。

## AngularJS DOM

ng-disabled：直接绑定应用程序数据到 HTML 的 disabled 属性。

## AngularJS 事件

ng-click：点击事件。

ng-show：显示 ( true ) /隐藏 ( false ) 一个 HTML 元素。

ng-hide：隐藏 ( true ) /显示 ( false ) 一个 HTML 元素。

## AngularJS 模块

创建模块：

```js
var app = angular.module("myApp", []); 
app.controller("myCtrl", function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
});
```

为模块添加控制器：

```html
<div ng-app="myApp" ng-controller="myCtrl">
{{ firstName + " " + lastName }}
</div>
```

## AngularJS 表单

- input 元素
- select 元素
- button 元素
- textarea 元素

## AngularJS 输入验证

HTML 表单属性 `novalidate` 用于禁用浏览器默认的验证。

| 属性      | 描述             |
| --------- | ---------------- |
| $dirty    | 表单有填写记录   |
| $valid    | 字段内容合法的   |
| $invalid  | 字段内容是非法的 |
| $pristine | 表单没有填写记录 |

## AngularJS 全局API

| API                                                          | 描述                                          |
| ------------------------------------------------------------ | --------------------------------------------- |
| angular.lowercase (<angular1.7） angular.$$lowercase()（angular1.7+） | 转换字符串为小写                              |
| angular.uppercase() (<angular1.7） angular.$$uppercase()（angular1.7+） | 转换字符串为大写                              |
| angular.isString()                                           | 判断给定的对象是否为字符串，如果是返回 true。 |
| angular.isNumber()                                           | 判断给定的对象是否为数字，如果是返回 true。   |

## AngularJS 包含

ng-include：包含 HTML 内容，包含 AngularJS 代码。

## AngularJS 依赖注入

