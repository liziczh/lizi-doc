# Angular

**Angular 是什么**：

[Angular](https://www.angular.cn/) 是由谷歌开发与维护一个开发跨平台应用程序的框架，同时适用于手机与桌面。

**Angular 有什么特点**：

- 基于 Angular 我们可以构建适用于所有平台的应用。比如：Web 应用、移动 Web 应用、移动应用和桌面应用等。
- 通过 Web Worker和服务端渲染 (SSR)，达到在如今Web平台上所能达到的最高渲染速度。
- Angular 让你能够有效掌控可伸缩性。基于 RxJS、Immutable.js 和其它推送模型，能适应海量数据需求。

**Angular 提供了哪些功能**：

- 动态HTML
- 强大的表单系统 (模板驱动和模型驱动)
- 强大的视图引擎
- 事件处理
- 快速的页面渲染
- 灵活的路由
- HTTP 服务
- 视图封装
- AOT、Tree Shaking

**Angular 与 AngularJS 有什么区别**：

- 不再有`Controller`和 `Scope`
- 更好的组件化及代码复用
- 更好的移动端支持
- 引入了 `RxJS` 与 `Observable`
- 引入了 `Zone.js`，提供更加智能的变化检测

### Angular 配置环境

**基于 Angular Quickstart**：

使用 Git 克隆：

```
git clone https://github.com/angular/quickstart ng4-quickstart
```

安装依赖：

```
npm install
```

**基于 Angular CLI**：

安装 Angular CLI：

```
npm install -g @angular/cli
```

检测是否安装成功：

```
ng --version
```

创建新项目：

```
ng new PROJECT-NAME
```

启动本地服务器：

```
cd PROJECT-NAME
ng serve
```

### 数据绑定

插值表达式：`{{expression}}` ，`{{expression | pipe}}` ；

### 自定义组件

创建新组件：

```
ng generate component COMPONENT-NAME
```

定义组件元信息：

```
@Component({
   selector: 'app-component-name',  // 用于定义组件在HTML代码中匹配的标签
   templateUrl: './component-name.component.html', // 定义组件的模板视图文件路径
   styleUrls: ['./component-name.component.css']  // 定义组件的样式文件路径
})
```

定义组件类：

```
export class Student  {
  name = 'zhangsan'; 
}
```

### 路由

导入路由模块：

```typescript
import { RouterModule } from '@angular/router';

@NgModule({
  imports: [RouterModule,...],
  ...
})
export class AppModule { }
```

配置路由信息：

```typescript
import { Routes, RouterModule } from '@angular/router';
import { UserComponent } from './user.component';

export const ROUTES: Routes = [
  { path: 'user', component: UserComponent }
];

@NgModule({
  imports: [
    BrowserModule,
    RouterModule.forRoot(ROUTES)
  ],
  // ...
})
export class AppModule {}
```

routerLink 指令：链接到相应组件。

```html
<a routerLink="/">首页</a>
<a routerLink="/user">我的</a>
```

router-outlet 指令：此处加载组件

```
<router-outlet></router-outlet>
```

## 架构

一个 Angualr 应用的基本构造块是 `NgModule` ，它为组件提供了编译的上下文。一个 Angular 应用是由一组 `NgModule` 定义出来的。

- 组件定义**视图**。
- 组件使用**服务**（服务作为**依赖**被**注入**到组件中）。

组件与服务都是简单类，使用装饰器标注它们的类型，并提供元数据告知 Angular 如何使用它们。

- 组件类(@Component)元数据：关联组件类与视图模板 (template) 。

- 服务类(@Injectable)元数据：提供了一些信息，Angular 要用这些信息来让组件可以通过*依赖注入（DI）*使用该服务。
- 路由服务：定义视图间的导航路径。

**模块**：

NgModule 可以将其组件和一组相关代码（如服务）关联起来，形成功能单元。

每个应用都有一个根模块 (AppModule) 。根模块提供了用来启动应用的引导机制。 一个应用通常会包含很多功能模块。

NgModule 也可以从其它 NgModule 中导入功能，并允许导出它们自己的功能供其它 NgModule 使用。

**组件**：模板、指令和数据绑定。

使用 `@Component()` 装饰器标注组件，每个应有至少有一个根组件。

**模板、指令和数据绑定**：

- 属性绑定：组件输入数据，将应用数据中的值插入视图。
- 事件绑定：组件输出数据，应用接受模板中的数据。

**服务与依赖注入**：

服务：与视图无关，跨组件共享数据与逻辑。

依赖注入：服务作为依赖被注入到组件中。

**路由**：路由器会将URL路径映射到视图。

### 模块简介（NgModule）

Angular 应用是模块化的。

一个 NgModule 就是一个容器，用于存放一些内聚的代码块，这些代码块专注于某个应用领域、某个工作流或一组紧密相关的功能。 它可以包含一些组件、服务提供商或其它代码文件，其作用域由包含它们的 NgModule 定义。 它还可以**导入**一些由其它模块中导出的功能，并**导出**一些指定的功能供其它 NgModule 使用。

每个Angular应用至少包含一个根模块 (AppModule) ，位于 `app.module.ts` 文件中，引导根模块即可启动应用。

**@NgModule 元数据**：

`@NgModule()`：NgModule 装饰器；

- declaration (可声明对象) - 属于本 NgModule 的组件、指令、管道。
- import (导入模块) - 本 NgModule 导入的其他模块。
- export (导出模块) - 本 NgModule 导出的可在其他模块的组件模板中使用的可声明对象的子集。
- providers (服务提供商) - 模块级别的服务提供商，全局服务提供商。
- bootstrap (根组件) - 只有根模块设置 bootstrap 属性。

**NgModule 与组件**：

NgModule 为其中的组件提供了一个*编译上下文环境*。

组件与模板共同定义了视图，组件还可以包含视图层次结构。

**NgModule 与 JavaScript**：

NgModule 系统与 JavaScript（ES2015）用来管理 JavaScript 对象的模块系统无关。

### 组件简介（Component）

组件控制屏幕上称为视图的一小片区域。

组件通过一些由属性和方法组成的 API 与视图交互。

**@Component 元数据**：

`@Component()`：组件装饰器；

- selector - 一个 CSS 选择器，在HTML中选择器对应的标签中插入该组件。
- templateUrl - 组件 HTML 模板文件的相对路径。
- providers：当前组件的服务提供商。

**模板语法**：

- 数据绑定 - 协调应用与 DOM 中的数据，（`{{hero.name}}`，`(click)`，`[hero]`）。

- 管道：数据转换。

- 指令：程序逻辑应用到视图上，（`*ngIf`，`*ngFor`）。

**数据绑定**：

- 插值表达式：`{{value}}`；（组件 --> DOM）
- 属性绑定：`[prop] = "value"`；（组件 --> DOM）
- 事件绑定：`(event) = "handler"`；（组件 <-- DOM）
- 双向绑定：`[(ngModel)] = "property"`；（组件 <--> DOM）

> - 父组件 --> 子组件：属性绑定。
> - 子组件 --> 父组件：事件绑定。

**管道**：

管道 (Pipe) ：值的转换逻辑。

管道使用：`{{ value | pipe }}`；

管道声明：`@Pipe()` 管道装饰器；

**指令**：

Angular 模板是动态的，Angular 渲染时，根据指令对 DOM 进行转换。

指令分为结构型指令和属性型指令。

- 结构型指令：`*ngIf`，`*ngFor`。
- 属性型指令：`ngModel`。

### 服务（Service）

理想情况下，组件的工作只管用户体验，把诸如从服务器获取数据、验证用户输入或直接往控制台中写日志等工作委托给各种服务。

Angular 通过依赖注入将服务注入到组件中。

`@Injectable()` ：服务装饰器。

### 后续

响应式编程：

- 生命周期钩子：通过实现生命周期钩子接口，可以监听组件生命周期中的一些关键时刻，从创建到销毁。
- 可观察对象：在组件和服务中使用可观察对象来发布和订阅任意类型的消息。

客户端与服务器交互：

- HTTP：用 HTTP 客户端与服务器通讯，以获取数据、保存数据或执行服务端动作。
- 服务端渲染：Angular Universal 会通过服务端渲染（SSR）技术在服务器上生成静态的应用页面。
- Service Worker：借助 Service Worker 来减轻对网络的依赖，你可以显著提升用户体验。

特定库：

- 动画：
- Forms：









