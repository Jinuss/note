# import和require的区别

### require

只能在运行时确定模块

- `require`是AMD规范引入方式
       
- 运行时调用

- 赋值过程  

- 写法
    
    ```js
       const fs=require("fs")
       exports.fs=fs;
       module.exports=fs;  
    ```

### import

- `import`是ES6的一个语法标准
- 编译时调用
- 解构过程
- 写法
  
  ```js
  import fs from 'fs'
  import {default as fs} from 'fs'
  import * as fs from 'fs'
  import {readFile} from 'fs'
  import {readFile as read} from 'fs'
  import fs, {readFile} from 'fs'

  export default fs
  export const fs
  export function readFile
  export {readFile, read}=fs
  export * from 'fs'
  
  ```