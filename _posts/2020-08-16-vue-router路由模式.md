vue-router的model有两种模式：**hash模式**和**history模式**

- 前端路由的核心就在于改变视图的同时不会向后端发出请求

#### 两种模式的区别

- hash模式
  - 即地址栏URL中的#符号。hash虽然出现在URL中，但不会被包括在HTTP请求中，对后端完全没有影响，因此改变hash不会重新加载页面。
  - 只可修改#后面的部分，因此只能设置与当前URL同文档的URL
  - hash设置的新值必须与原来不一样才会触发动作将记录添加到栈中
  - 只可添加短字符串
- history模式
  - 利用`HTML5 History Interface`中新增的`pushState()`和`replaceState()`方法。这两个方法应用于浏览器的历史记录栈，在当前已有的back、forward、go的基础上，它们提供了对历史记录进行修改的功能。只是当它们执行修改时，虽然改变了当前的URL，但浏览器不会立即向后端发送请求。需搭配前端路由的404页面支持。
  - `pushState()`设置的新URL可以与当前URL同源的任意URL
  - `pushState()`设置的新URL与当前URL一模一样也会将记录添加到栈中
  - `pushState()`通过`stateObject`参数可以添加任意类型的数据到记录中
  - `pushState()`可额外设置title属性供后续使用

