---
categories:
 - Backbone
tags:
 - Backbone
---
最近开始接触 `Backbone.js` , 这个曾经非常优秀的一款MVC前端框架，在学习的过程中，遇到下图的这样一个问题

![Error](https://jinuss.github.io/note/jinus/img/2017031801.png)

代码如下：

``` js
     searchView = Backbone.View.extend({
         initialzie: function() {
             this.render()
         },
         render: function() {
             var template = _.template($("#search_template").html(), {});
             this.el.html(template);
         }
     })
```

只是 `view` 了一个实例，然后在 `initalize` 中调用了 `render` 方法，再通过 `jquery` 获取到的内容作为模板 `template` ，将此模板填充到 `view` 中绑定 `el` 属性的容器上，然而浏览器显示 `this.el.html` 不是一个函数. 试着通过 `typeof  this.el` , 输出的是 `object` ，所以这个 `el` 是个对象，但不是一个 `jquery` 对象。

后来查阅官方文档看到有个 `el` ，它是视图元素的缓存 `jQuery` 对像，主要是简单的引用；而el属性则是所有的视图都拥有的一个 `DOM` 元素，所以二者的用法还是有区别的。 `el` 属性可以指定容器与哪个元素绑定，而绑定过后，视图就通过 `el` 来进行接下来的操作。

当将 `this.el.html` 替换成 `this.$el.html` 时，出现如期效果，无报错，如下图

![Success](/jinus/img/2017031802.png)
