---
title: Web | jQuery
id: web-jquery
comments: true
tags:
  - Web前端
categories: Web前端
date: 2018-04-09 16:34:00
toc: true
reward: true
copyright: true
---

<!--# jQuery-->

jQuery 是一个 JavaScript 函数库，将 JS 的一些功能实现封装成了函数，极大地简化了 JavaScript 编程。
jQuery是 John Resig 于2006年创建的一个开源项目，jQuery集成了 JavaScript、CSS、 DOM 和 Ajax 于一体的强大功能。它可以用最少的代码， 完成更多复杂而困难的功能。

<!--more-->

# 1. 引入jQuery

## 1.1 jQuery版本

jQuery版本：
- jQuery-1.x.x：兼容IE6/7/8低级浏览器。
- jQuery-2.x.x：不兼容IE6/7/8。
- jQuery-3.x.x：全面支持HTML5和CSS3。

jQuery版本分类：
- Development version：[jquery.js] 开发版，未压缩，用于测试和开发。
- Production version：[jquery.min.js] 精简版，已被压缩。

## 1.2 引入jQuery

```html
<head>
	<script type="text/javascript" src="js//jquery-1.12.4.js"></script>
</head>
```

# 2. jQuery入门

## 2.1 jQuery对象与DOM对象

jQuery简化了JS编程，多数JS功能实现都被封装成了函数，而调用这些jQuery函数必须使用jQuery对象。

jQuery对象本质是一个DOM对象数组。

```js
// DOM对象转jQuery对象
var $jQueryObj = $(DOMObj);
// jQuery对象转DOM对象
var DOMObj = $jQueryObj[0];
var DOMObj = $jQueryObj.get(0);
```

> jQuery对象命名一般以$为前缀

## 2.2 jQuery入口函数

第一种写法：

```js
$(document).ready(function(){
    jQuery代码
});
```

第二种写法：

```js
$(function(){
    jQuery代码
});
```

**jQuery入口函数与JS入口函数的区别**：
①jQuery的入口函数只等待DOM树加载完即执行；
②JS入口函数需要等待所有资源加载完成再执行；

## 2.3 jQuery基础语法

```js
$("选择器").操作函数()
```

`$()`：即`jQuery()`，本质是一个函数。

# 3. jQuery选择器

jQuery选择器：获取元素

## 3.1 元素选择器

| 元素选择器 | 描述         |
| ---------- | ------------ |
| `*`        | 通配符选择器 |
| `#id`      | id选择器     |
| `.class`   | 类选择器     |
| `element`  | 元素选择器   |
| `s1s2`     | 交集选择器   |
| `s1,s2`    | 并集选择器   |
| `s1 s2`    | 后代选择器   |
| `s1 > s2`  | 子元素选择器 |

## 3.2 属性选择器

| 属性选择器      | 描述                    |
| --------------- | ----------------------- |
| `[attr]`        | 属性选择器              |
| `[attr=value]`  | 属性=值的元素           |
| `[attr!=value]` | 属性!=值的元素          |
| `[attr$=value]` | 属性值以value结尾的元素 |

## 3.3 过滤选择器

| 位置     | 描述         |
| -------- | ------------ |
| `:first` | 第一个元素   |
| `:last`  | 第二个元素   |
| `:odd`   | 所有奇数元素 |
| `:even`  | 所有偶数元素 |

| 索引位置     | 描述                               |
| ------------ | ---------------------------------- |
| `:eq(index)` | 指定索引的元素<br>（index从0开始） |
| `:gt(num)`   | 所有索引>num的元素                 |
| `:lt(num)`   | 所有索引<num的元素                 |

| 标签类型    | 描述         |
| ----------- | ------------ |
| `:header`   | 所有标题元素 |
| `:animated` | 所有动画元素 |

| 元素状态          | 描述               |
| ----------------- | ------------------ |
| `:contains(text)` | 包含指定文本的元素 |
| `:empty`          | 无子节点的元素     |
| `:hidden`         | 所有隐藏的元素     |
| `:visible`        | 所有可见的元素     |

## 3.4 表单选择器

| 表单元素    | 描述                                 |
| ----------- | ------------------------------------ |
| `:input`    | 所有`<input>`元素                    |
| `:text`     | 所有`type="text"`的 `<input>` 元素   |
| `:password` | 所有`type="password"`的`<input>`元素 |
| `:radio`    | 所有`type="radio"`的`<input>`元素    |
| `:checkbox` | 所有`type="checkbox"`的`<input>`元素 |
| `:submit`   | 所有`type="submit"`的`<input>`元素   |
| `:reset`    | 所有`type="reset"`的`<input>`元素    |
| `:button`   | 所有`type="button"`的`<input>`元素   |
| `:image`    | 所有`type="image"`的`<input>`元素    |
| `:file`     | 所有`type="file"`的`<input>`元素     |
| `:enable`   | 所有激活的`<input>`元素              |
| `:disabled` | 所有禁用的`<input>`元素              |
| `:selected` | 所有被选取的`<input>`元素            |
| `:checked`  | 所有被选中的`<input>`元素            |

# 4. jQueryDOM★

## 4.1 DOM 操作★

### 4.1.1 DOM HTML内容

**1.text()**：设置/获取**所选元素的文本内容**

```js
$("selector").text();  // 获取文本内容
$("selector").text("文本内容");  // 设置文本内容
```

**2.html()**：设置/获取**所选元素的内容**

```js
$("selector").html();  // 获取HTML内容
$("selector").html("HTML代码");  //设置 HTML内容
```

**3.val()**：设置/获取**表单字段的值**

```js
$("selector").val();  // 获取表单字段的值
$("selector").val("表单字段值");  // 设置表单字段的值
```

### 4.1.2 DOM HTML属性

**1.attr()**：**HTML属性**，只能返回string的结果

```js
$("selector").attr("属性名");  // 获取HTML属性
$("selector").attr("属性名", "值");  // 设置HTML属性
$("selector").attr({"属性名":"值", "属性名":"值"});  // 设置多个HTML属性
```

**2.prop()**：**DOM属性**，如selected，disabled，checked等属性。

```js
$("selector").prop("属性名");  // 获取属性
$("selector").prop("属性名", "值");  // 设置属性
$("selector").prop({"属性名":"值", "属性名":"值"});  // 设置多个属性
```

### 4.1.3 DOM 插入元素

**1.append()**：在被选元素的结尾追加内容

```js
$("selector").append("插入内容");
```

**2.prepend()**：在被选元素的开头插入内容

```js
$("selector").prepend("插入内容");
```

**3.before()**：在被选元素之前插入内容

```js
$("selector").before("插入内容");
```

**4.after()**：在被选元素之后插入内容

```js
$("selector").after("插入内容");
```

### 4.1.4 DOM 删除元素

**1.remove()**：删除被选元素及其子元素

```js
$("selector").remove();
```

**2.empty()**：删除被选元素的所有子元素

```js
$("selector").empty();
```

### 4.1.5 DOM CSS类

**1.addClass()**：向被选元素添加一个或多个样式类

```js
$("selector").addClass("类名");
```

**2.removeClass()**：从被选元素移除一个或多个类

```js
$("selector").removeClass("类名");
```

**3.toggleClass()**：对被选元素进行类切换（本质是类的添加/删除）

```js
$("selector").toggleClass("类名");
```

**4.hasClass()**：判断被选元素是否存在类

```js
$("selector").hasClass("类名");
```

### 4.1.6 DOM CSS属性

**1.css()**：设置或返回样式属性

```js
$("selector").css("样式属性");       // 获取样式属性值
$("selector").css("样式属性","值");  // 设置样式属性
$("selector").css({"样式属性":"值","样式属性":"值",...});  // 设置多个样式属性
```

### 4.1.7 DOM 元素尺寸

**1.width()**：设置或返回元素的宽度（不包括内边距、边框、外边距）

```js
$("selector").width();
```

**2.height()**：设置或返回元素的高度（不包括内边距、边框、外边距）

```js
$("selector").height();
```

**3.innerWidth()**：返回元素的宽度（包括内边距）

```js
$("selector").innerWidth(); 
```

**4.innerHeight()**：返回元素的高度（包括内边距）

```js
$("selector").innerHeight();
```

**5.outerWidth()**：返回元素的宽度（包括内边距、边框）

```js
$("selector").outerWidth();
```

**6.outerHeight()**：返回元素的高度（包括内边距、边框）

```js
$("selector").outerHeight();
```

**7.outerWidth(true)**：返回元素的宽度（包括内边距、边框和外边距）

```js
$("selector").outerWidth(true);
```

**8.outerHeight(true)**：返回元素的高度（包括内边距、边框和外边距）

```js
$("selector").outerHeight(true);
```

### 4.1.8 DOM 位置

**1.scrollTop()**：滚动条顶部偏移量

```js
$("selector").scrollTop();
```

**2.scrollLeft()**：滚动条左边偏移量

```js
$("selector").scrollLeft();
```

## 4.2 DOM 遍历★

### 4.2.1 向上遍历-祖先

**1.parent()**：返回被选元素的直接父元素

```js
$("selector").parent("筛选选择器");  // 直接父元素，可筛选
```

**2.parents()**：返回被选元素的所有祖先元素，向上遍历直到根`<html>`

```js
$("selector").parents("筛选选择器");  // 所有祖先元素，可筛选
```

**3.parentsUntil()**：返回介于两个给定元素之间的所有祖先元素

```js
$("selector1").parentsUntil("selector2"); 
```

### 4.2.2 向下遍历-后代

**1.children()**：返回被选元素的所有直接子元素

```js
$("selector").children("筛选选择器");  // 返回直接子元素，可筛选
```

**2.find()**：返回被选元素的所有后代元素

```js
$("selector").find("筛选选择器"); // 返回后代元素，可筛选
```

### 4.2.3 水平遍历-兄弟

**1.siblings()**：返回被选元素的所有兄弟元素

```js
$("selector").siblings("筛选选择器");  // 返回所有兄弟元素，可筛选
```

**2.next()**：返回被选元素的下一个兄弟元素

```js
$("selector").next("筛选选择器");  // 返回下一个兄弟元素，可筛选
```

**3.nextAll()**：返回被选元素之后的兄弟元素

```js
$("selector").nextAll("筛选选择器");  // 返回元素之后的兄弟元素，可筛选
```

**4.nextUntil()**：返回介于两个给定元素之间的所有兄弟元素

```js
$("selector1").nextUntil("selector2");  // 从selector1水平向后遍历直到selector2
```

**5.prev()**：返回被选元素的上一个兄弟元素

```js
$("selector").prev("筛选选择器");  // 返回上一个兄弟元素，可筛选
```

**6.prevAll()**：返回被选元素之前的同胞元素

```js
$("selector").prevAll("筛选选择器");  // 返回元素之前的兄弟元素，可筛选
```

**7.prevUntil()**：返回介于两个给定元素之间的所有同胞元素

```js
$("selector1").prevUntil("selector2");  // 从selector1水平向前遍历直到selector2
```

### 4.2.3 元素筛选

**1.eq()**：返回被选元素中带有指定索引的元素

```js
$("selector").eq(index);  // 返回指定索引的元素
```

**2.filter()**：返回匹配筛选标准的元素

```js
$("selector").filter("筛选选择器");  // 返回匹配筛选选择器的元素
```

**3.not()**：返回不匹配筛选标准的元素

```js
$("selector").not("筛选选择器");  // 返回不匹配筛选选择器的元素
```

**4.first()**：获取第一个元素

```js
$("selector").first();
```

**5.last()**：获取最后一个元素

```js
$("selector").last();
```

# 5. jQuery效果

## 5.1 隐藏/显示

**1.show()**：显示

```js
$("selector").show(speed,callback);
```

**2.hide()**：隐藏

```js
$("selector").hide(speed,callback);
```

**3.toggle()**：切换hide()/show()

```js
$("selector").toggle(speed,callback)
```

> speed：速度(ms)，[可选参数]
> callback：当前动画 100% 完成之后执行的函数，[可选参数]

##  5.2 淡入/淡出-fade

**1.fadeIn()**：淡入已隐藏的元素

```js
$("selector").fadeIn(speed,callback);
```

**2.fadeOut()**：淡出已显示的元素

```js
$("selector").fadeIn(speed,callback);
```

**3.fadeToggle()**：切换fadeIn()和fadeOut()

```js
$("selector").fadeToggle(speed,callback);
```

**4.fadeTo()**：允许渐变为给定的不透明度

```js
$("selector").fadeTo(speed,opacity,callback);
```

> speed：速度(ms)，[可选参数]
> opacity：不透明度(0~1)，[可选参数]
> callback：当前动画 100% 完成之后执行的函数，[可选参数]

## 5.3 滑动-slide

**1.slideDown()**：向下滑动元素

```js
$("selector").slideDown(speed,callback);
```

**2.slideUp()**：向上滑动元素

```js
$("selector").slideUp(speed,callback);
```

**3.slideToggle()**：切换slideDown()和slideUp()

```js
$("selector").slideToggle(speed,callback);
```

## 5.4 自定义动画-animate

**animate()**：自定义动画

```js
$("selector").animate({params},speed,callback);
```

> params：定义形成动画的CSS属性，[必要参数]
> speed：速度(ms)，[可选参数]
> callback：当前动画 100% 完成之后执行的函数，[可选参数]
>

## 5.5 停止效果-stop

**stop()**：在动画或效果完成前对它们进行停止

```js
$("selector").stop(stopAll,goToEnd);
```

> stopAll：是否清除动画队列；默认false-仅停止活动的动画，允许队列中后面的动画执行。
> goToEnd：是否立即完成当前动画 ；默认false。

# 6. jQuery事件机制

## 6.1 事件类型

| 事件句柄    | 描述         |
| ----------- | ------------ |
| ready    | DOM载入      |
| click     | 鼠标单击     |
| focus     | 元素获得焦点 |
| blur      | 元素失去焦点 |
| mouseover | 鼠标覆盖     |
| mouseout  | 鼠标移开     |
| mouseup   | 鼠标点击     |
| mousedown | 鼠标松开     |
| scroll    | 窗口滚动     |
| change    | 发生改变     |
| unload    | 退出页面     |
| submit    | 点击提交     |
| keydown	  | 某个键盘的键被按下 |
| keypress  | 某个键盘的键被按下或按住 |
| keyup	  | 某个键盘的键被松开|

## 6.2 事件绑定方式

**1.简单事件绑定**：

```js
$("selector").事件(handler);
```

> 一次只能绑定一个事件

**2.bind()**：事件绑定

```js
$("selector").bind("events"[,data],handler);
```

> bind()不支持动态绑定。

**3.delegate()**：事件委托

```js
$("selector").delegate("childSelector","events"[,data],handler);
```

> 通过委托父元素可以动态为当前或未来子元素绑定事件；
>

**4.on()**：统一事件绑定方式，推荐使用

```js
$("selector").on("events"[,"childSelector"][,data],handler)
```

> events：事件，多个事件以空格分隔
> childSelector：后代元素，[可选]
> data：传递给handler的数据，事件触发后通过event.data调用，[可选]
> handler：事件处理函数

## 6.3 事件移除方式

**1.unbind()**：移除被选元素的事件 

```js
$("selector").unbind("events",handler);
```

**2.undelegate()**：移除由delegate()添加的事件

```js
$("selector").undelegate("selector","events",handler); 
```

> events：规定需要删除处理函数的一个或多个事件类型 ，[可选]
> selector：规定需要删除事件的选择器 ，[可选]
> handler：规定要删除的具体事件处理函数 ，[可选]

**3.off()**：解除事件绑定，推荐使用

```js
$("selector").off("events"[,"selector"][,handler],map);
```

> events：规定需要删除处理函数的一个或多个事件类型 ，[可选]
> selector：规定需要删除事件的选择器 ，[可选]
> handler：规定要删除的具体事件处理函数 ，[可选]
> map：规定事件映射 (*{event:function, event:function, ...})* ，包含要添加到元素的一个或多个事件，以及当事件发生时运行的函数。 

# 7. jQuery杂项（难点）

## 7.1 链式编程

链式编程：使用一个jQuery对象不断地调用(点调用)函数。栗子如下：

```js
$("div").addClass("highlight").children("a").show().end().siblings().removeClass("highlight").children("a").hide();
```

> 非筛选函数：函数返回当前jQuery对象，jQuery对象不发生改变，如addClass()，hide()...
> 筛选函数：函数返回新的jQuery对象，如find()，parent()...
>

**链式编程原理**：jQuery的**非筛选函数**都返回其本身

```js
return this
```

**end()**：结束当前链条中的最近的筛选操作，并将匹配元素集还原为之前的状态

```js
jQueryObj.end()
```

**链式编程本质★**：
jQuery对象(即包装后的DOM对象)
①调用**筛选/遍历函数**后返回新的jQuery对象，将新的jQuery对象压入栈内；
②调用**非筛选/遍历函数**后返回本身(return this)。
③调用**end()**将栈顶元素 (当前jQuery对象) 弹出栈，指向新的栈顶元素 (最近上一次的jQuery对象)。

## 7.2 隐式迭代

**隐式迭代**：jQuery对象本质是DOM对象数组，即`$("selector")`返回一个对象数组。jQuery会自动对匹配到的DOM数组进行循环遍历，执行所调用的函数。

**设置操作**：隐式迭代，循环遍历DOM对象数组执行设置函数。

**获取操作**：大部分情况下返回第一个元素的值。

## 7.3 each方法

**each()**：为每个匹配元素规定运行的函数

```js
$("selector").each(function(index,element));
```

> function()：为每个匹配元素规定运行的函数，[必要参数]
> index：选择器的index位置
> element：当前的元素

## 7.4 多库共存

jQuery使用`$`标识符作为`jQuery`的简写符号 ，如果页面上同时存在其他JS库正在使用相同的简写符号`$`怎么办呢？

**noConflict()**：释放`$`标识符的控制

```js
var a = $.noConflict();  // 释放$的控制权，将$的能力赋予a。
```




