# AngularJS

AngularJS 一个 JavaScript 框架，AngularJS 通过**指令**扩展了 HTML，通过**表达式**绑定数据到 HTML 页面上。

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

