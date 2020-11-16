---
categories:
 - TypeScript
---

**类型系统**和**ES的支持**

### 基础类型

 - 布尔值 `true` / `false`

 - 数字 `number`

 - 字符串 `string`

 - 数组 （形如 `number[]` 或者 `Array<number>` ）
 - 元组 : 这种类型允许表示一个已知元素数量和类型的数组
 - 枚举
 - `Any` ：表示允许赋值为任意类型
 - `Void` ：表示没有任何返回值的函数
 - `Null` 和 `undefined`

 - `never`

 - `object` ：非原始类型
 - 联合类型 : `string | number`

 #### 类型断言 `type assertion`

    

``` ts
      值 as 类型
```

 #### 类型推论 `type inference`

### 接口

   接口(interfaces)是对行为的抽象，或常用于对对象的形状进行描述

  + 属性可选 `?`
  + 只读属性 `readonly`: 变量使用`const`，属性使用`readonly`
  + 额外的属性检查
  + 函数类型

### 重载

  重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。

### 函数表达式

  在typescript中 `=>` 用来表示函数的定义，左边时输入类型，需要用括号括起来，右边时输出类型
  
### 声明文件

  声明文件必须以 `.d.ts` 为后缀。声明语句中只能定义类型。 
  
