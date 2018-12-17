---
title: Hexo | （五）Yilia主题优化
comments: true
date: 2018-06-17 19:13:57
id: hexo-yilia-optimization
tags:
- Hexo博客
categories: Hexo博客
toc: true
reward: true
copyright: true
---

<!--# Hexo | （五）Yilia主题优化-->

yilia主题简单优雅，但是缺少一些我想要的功能，所以我自己稍微扩展了一些功能，比如添加了之前使用的LiveRe评论系统，新增了百度自动推送功能，在文章底部追加了版权声明，勉强实现了相册功能。
优化后的yilia主题：[https://github.com/liziczh/hexo-theme-yilia](https://github.com/liziczh/hexo-theme-yilia)

<!--more-->

### add:LiveRe评论系统 

1.在`yilia/layout/_partial/post`下添加`livere.ejs`文件

```ejs
<!-- 来必力City版安装代码 -->
<div id="lv-container" data-id="city" data-uid="<%=theme.livere_uid%>">
<script type="text/javascript">
   (function(d, s) {
       var j, e = d.getElementsByTagName(s)[0];
       if (typeof LivereTower === 'function') { return; }
       j = d.createElement(s);
       j.src = 'https://cdn-city.livere.com/js/embed.dist.js';
       j.async = true;
       e.parentNode.insertBefore(j, e);
   })(document, 'script');
</script>
</div>
<!-- City版安装代码已完成 -->
```

2.编辑`yilia/layout/_partial/article.ejs`，在评论代码中引用`livere.ejs`

```ejs
<% if (theme.livere_uid){ %>
   <%- partial('post/livere', {
     key: post.slug,
     title: post.title,
     url: config.url+url_for(post.path)
   }) %>
<% } %>
```

3.编辑`yilia/_config.yml`，添加`livere_uid`属性

```yaml
#0、liveRe评论
livere_uid: false
```

### add:百度推送 

1.在`yilia/layout/_partial`下添加`baidu-push.ejs`文件

```ejs
<% if (theme.baidu_push){ %>
<script>
(function(){
    var bp = document.createElement('script');
    var curProtocol = window.location.protocol.split(':')[0];
    if (curProtocol === 'https') {
        bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';
    }
    else {
        bp.src = 'http://push.zhanzhang.baidu.com/push.js';
    }
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(bp, s);
})();
</script>
<% } %> 
```

2.编辑`layout/_partial/head.ejs`，引用`baidu-push.ejs`

```ejs
<%- partial('baidu-push') %>
```

3.编辑`yilia/_config.yml`，添加`baidu_push`属性

```yaml
# 百度推送
baidu_push: true
```

### add:版权声明

1.编辑`layout/_partial/head.ejs`，添加`post-copyright`代码

```ejs
<% if (((theme.copyright_type === 2 && !post.copyright) || (theme.copyright_type === 1 && post.copyright)) && !index){ %>
<div> 
    <ul class="post-copyright">
        <li>
            <strong>本文作者：</strong><%= config.author%>
        </li>
        <li>
            <strong>本文链接：</strong>
            <a href="<%= config.url %><%- url_for(post.path) %>" title="<%= config.title %>"><%= config.url %><%- url_for(post.path) %></a>
        </li>
        <li>
            <strong>版权声明： </strong>
            本博客所有文章除特别声明外，均采用 <a href="https://creativecommons.org/licenses/by-nc-sa/3.0/" rel="external nofollow" target="_blank">CC BY-NC-SA 3.0</a> 许可协议。转载请注明出处!
        </li>
    </ul>
</div>
<% } %>
```

2.添加`.post-copyright`的样式

```css
.post-copyright{
    margin: 0em 0em 0em 0em;
    padding: 0.5em 1em;
    border-left:3px solid #ff1700;
    background-color: #f9f9f9;
    list-style: none;
    font-size: 14px;
}
```

3.编辑`yilia/_config.yml`，添加`copyright_type`属性

```yaml
# 版权声明
# type：0-关闭版权声明； 1-存在copyright:true属性的文章，显示版权声明； 2-所有文章均有版权声明
copyright_type: 0
```

### new:相册页面（未完成）

暂时是直接将以下代码嵌入index.md文档中，勉强实现相册功能。但这样写我自己看着都难受，之后有时间再改。

1.相册图片CSS

```css
<style>
.img{width:240px;display:inline-block;margin:0 10px 10px 0;}
.img-last{width:240px;display:inline-block;margin:0 0 10px 0;}
</style>
```

2.原生JS实现jQuery入口函数，实现手机图片自适应。

```html
<script>
/*原生JS实现jQuery入口函数*/
function ready(fn){
    if(document.addEventListener{
       document.addEventListener('DOMContentLoaded',function(){
        document.removeEventListener('DOMContentLoaded',arguments.callee,false);
        fn();
    },false);
}else if(document.attachEvent){
    document.attachEvent('onreadystatechange',function(){
        if(document.readyState=='complete'){
            document.detachEvent('onreadystatechange',arguments.callee);
            fn();
        }
    });
}}; 
/*手机图片自适应*/
ready(function(){
    var img = document.getElementsByTagName("img");
    if(window.screen.width < 500){
        for(var i = 0 ; i < img.length;i++){
            var len = (window.screen.width-40) / 2;
            img[i].style.width = len.toString()+"px";
        }
    }
}); 
</script>
```

### new:Ones页面（未完成）

单独写一个Ones的静态界面，暂未完成，之后再说。



