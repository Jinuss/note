## 前言

 产品需求需要实现表格在分页跳转后能够保存前一页的操作状态（选中或取消选中）

## 实现

  + 第一步：实现当前页选中/取消选择

``` js
   toggleSelection() {
       //已经存在数据库
       this.tableDatauser.forEach((row) => {
           if (this.formerUserIdList.indexOf(row.userId) >= 0) {
               this.$refs.multipleTable.toggleRowSelection(row, true);
           }
       });
       let arr = this.tableDatauser;
       this.formerUserIdList.forEach((item) => {
           let tmp = arr.filter((i) => i.userId == item);
           if (!tmp.length) {
               this.hasSelectList.push(item);
           }
       });
       ...
   }
```

`multipleTable` 是表格的 `ref` 属性值, `tableDatauser` 是获取的表格数据， `formerUserIdList` 是上次选中的数据，已经存在于数据中的数据。上面这个代码块是实现了默认选中的情况。

  + 第二步：实现点击选中

  

``` js
  handleSelectionChanges(val) {
      let tmp = [];
      val.forEach((item) => {
          tmp.push(item.userId);
      });
      this.pageSelectedList = tmp;
  },
```

`pageSelectedList` 这个数组变量用于保存当前页的选中项，每次表格项有变动都会触发ElementUI内置的 `selection-change` 事件
  至此，已经满足当前页的选中（全选），我们需要实现的是表格翻页仍能保持前一次的操作，即缓存用户之前的操作（选中或未选中）状态。
  ElementUI还内置了一个事件 `select` , 这个事件是在表格项的复选框发生改变时会触发，因此我们可以从这里入手，获取用户操作后（不包含与数据库打交道，只是界面操作）。

  + 第三步、获取用户操作后的当前页状态

``` js
 selectRow(select, row) {
     let flg = true;
     let arr = select.filter((i) => i.userId == row.userId);
     if (arr.length) {
         this.preSelectedList.push(arr[0].userId);
         this.preCancelSelectdList = this.preCancelSelectdList.filter(
             (i) => i != row.userId
         );
     } else {
         this.preSelectedList = this.preSelectedList.filter(
             (i) => i != row.userId
         );
         this.preCancelSelectdList.push(row.userId);
     }
 },
```

`preSelectedList` 用于保存之前未选中现在选中项， `preCancelSelectedList` 用于保存之前选中现在取消项。这两个数组都是用来缓存用户在当前页的操作，便于在翻页后去渲染界面，渲染Demo如下：

``` js
 toggleSelection() {
     ...
     //前一页已经勾选但未保存
     this.tableDatauser.forEach((row) => {
         if (this.preSelectedList.indexOf(row.userId) >= 0) {
             this.$refs.multipleTable.toggleRowSelection(row, true);
         }
     });
     //前一页取消勾选但未保存
     this.tableDatauser.forEach((row) => {
         if (this.preCancelSelectdList.indexOf(row.userId) >= 0) {
             this.$refs.multipleTable.toggleRowSelection(row, false);
         }
     });
 },
```

* 最后：拼接需要保存到数据库的数据项

``` js
 this.hasSelectList = Array.from(
     new Set(
         this.hasSelectList.concat(
             this.pageSelectedList.concat(this.preSelectedList)
         )
     )
 );
 if (this.preCancelSelectdList.length) {
     this.hasSelectList = this.hasSelectList.filter(
         (i) => !this.preCancelSelectdList.includes(i)
     );
 }
```
