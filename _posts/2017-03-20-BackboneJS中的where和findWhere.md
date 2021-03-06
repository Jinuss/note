---
categories:
 - Backbone
tags:
 - Backbone
---
`BackboneJS` 算是前端MVC框架的鼻祖，虽已过时，但其思想还是值得研究学习。

`where` 和 `findWhere` 是该框架中经常用到的两个方法，下面做下笔记予以说明：

* `where` 返回的是一个数组，取值时需要指定 `model` (模型)的索引号，适用于查找具有相同属性值的多个 `model`
* `findWhere` 返回的是匹配到的第一个 `model` ，适用于查找 `collection` 中某一属性值唯一的集合

示例：

``` js
    (function($) {
        var stuModel = Backbone.Model.extend({
            defaults: {
                name: "",
                age: ""
            }
        });
        var collection = Backbone.Collection.extend({
            model: stuModel
        });
        var stu = [{
            name: "lihua",
            age: 18
        }, {
            name: "wangming",
            age: 19
        }, {
            name: "sunwei",
            age: 18
        }]
        var s1 = Stu.where({
            age: 18
        });
        var s2 = Stu.findWhere({
            age: 18
        });
        conosle.log(s1);
        console.log(s2)
    })(jQuery);
```

 输出 

![result](/jinus/img/2017032001.png)
