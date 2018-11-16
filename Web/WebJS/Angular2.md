# Angular2

[Angular](https://www.angular.cn/) 是由谷歌开发与维护一个开发跨平台应用程序的框架，同时适用于手机与桌面。

Angular 快速上手

安装 Angular CLI 

```shell
npm install -g @angular/cli
```

创建新应用

```shell
ng new MyAngularApp
```

启动应用服务器

```shell
cd MyAngularApp
ng serve --open
```

Angular 组件结构

- `app.component.html` - 组件模板，HTML。
- `app.component.css` - 组件私有 CSS 样式。
- `app.component.ts` - 组件类代码，TypeScript。

组件

新建组件

```shell
ng generate component componentName
```

*.component.ts：

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
```

> @Component
>
> - `selector` - 组件的选择器（CSS 元素选择器）
> - `templateUrl` - 组件模板文件的位置。
> - `styleUrls` - 组件私有 CSS 样式表文件的位置。

添加组件：在 app.component.html 插入组件

```html
......
<app-componentName></app-componentName>
......
```

创建类：

```html
export class Hero {
  id: number;
  name: string;
}
```

创建对象：

```ts
hero: Hero = {
  id: 1,
  name: 'Windstorm'
};
```

双向绑定：`[(ngModel)]` 

```html
<input [(ngModel)]="hero.name" placeholder="name">
```

事件绑定：`(click)` 

```html
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
```

循环：`*ngFor` 

```html
<li *ngFor="let hero of heroes">{{hero.name}}</li>
```

条件：`*ngIf` 

```html
<div *ngIf="selectedHero">
```



```ts
@Input() hero: Hero;
```

属性绑定：单向

```css
[hero]="selectedHero"
```

创建服务：

```html
ng generate service hero
```

注入：将服务注入到构造器中。





组件开发：主从组件。

服务：组件通信。

Angular 只会绑定到组件的*公共*属性。



## 架构

### 模块

模块由一块代码组成，可用于执行一个简单的任务。

模块属性：

- **declarations （声明）** - 视图类属于这个模块。 Angular 有三种类型的视图类： 组件 、 指令 和 管道 。
- **exports** - 声明（ declaration ）的子集，可用于其它模块中的组件模板 。
- **imports** - 本模块组件模板中需要由其它导出类的模块。
- **providers** - 服务的创建者。本模块把它们加入全局的服务表中，让它们在应用中的任何部分都可被访问到。
- **bootstrap** - 应用的主视图，称为根组件，它是所有其它应用视图的宿主。只有根模块需要设置 bootstrap 属性中。

### 组件

组件是一个模板的控制类用于处理应用和逻辑页面的视图部分。

组件是构成 Angular 应用的基础和核心，可用于整个应用程序中。

组件知道如何渲染自己及配置依赖注入。

组件通过一些由属性和方法组成的 API 与视图交互。

- HTML
- CSS
- TS

#### 元数据

@Component 装饰器：

- **selector** - 一个 css 选择器，它告诉 Angular 在 父级 HTML 中寻找一个标签，然后创建该组件，并插入此标签中。
- **templateUrl** - 组件 HTML 模板的地址。
- **directives** - 一个数组，包含 此 模板需要依赖的组件或指令。
- **providers** - 一个数组，包含组件所依赖的服务所需要的依赖注入提供者。

### 数据绑定

插值：在 HTML 标签中显示组件值。

```html
<h1>{{title}}</h1>
```

属性绑定：把HTML元素的属性设置为组件中属性的值。 

```html
<img [src]="userImageUrl">
```

事件绑定：

```html
<button (click)="onSave()">保存</button>
```

双向绑定：

```html
<input [(ngModel)]="model.name" >
```

### 指令

循环：`*ngFor = "let hero of heros"` 

选择：`*ngIf` 



### 服务

Angular2 中的服务是封装了某一特定功能，并且可以通过注入的方式供他人使用的独立模块。

服务一般用于发送请求，http获取数据。

### 依赖注入

```
constructor(private service: HeroService) { }
```

当 Angular 创建组件时，会首先为组件所需的服务找一个注入器（ Injector ） 。

注入器是一个维护服务实例的容器，存放着以前创建的实例。

如果容器中还没有所请求的服务实例，注入器就会创建一个服务实例，并且添加到容器中，然后把这个服务返回给 Angular 。

当所有的服务都被解析完并返回时， Angular 会以这些服务为参数去调用组件的构造函数。 这就是依赖注入 。



### 数据绑定

DOM属性绑定：`prop={{x}}` 与 `[prop]="x"` 相同。

`[x]`：从数据源到试图。

`(x)`：从视图到数据源。

`[(x)]`：双向绑定。

```java
[x]="xx" 
(xChange)="xx=$event"
```

`[(ngModel)]="x"` 本质：

```java
[ngModel]="x"
(ngModelChange)="x=$event"
```

模板引用变量

`#x`：表示一个 DOM 元素。

`@Input`：组件输入属性，当它通过属性绑定的形式被绑定时，值会“流入”这个属性。

`@Output：`：组件输出属性（EventEmitter），可观察对象型属性。

为属性指定别名：`@Output(alias) propertyName = ...`；
