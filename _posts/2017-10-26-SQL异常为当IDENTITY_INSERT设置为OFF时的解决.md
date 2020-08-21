---
categories:
- 数据库
tags:
- SQL
---
误删数据库时，可以利用`insert`插入删除的数据，但是有时表可能有自增字段如id。这是插入数据如果包含自增字段就会出现错误，提示"IDENTITY_INSERT设置为OFF,插入失败"。

所以我们将其设置为on即可，sql语句：
```SQL
 set IDENTITY_INSERT 表名 on
```
完美地解决了问题，当插入数据后，记得重置数据库设置
>IDENTITY_INSERT为off
