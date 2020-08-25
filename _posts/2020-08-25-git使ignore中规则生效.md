---
categories:
 - git
tags:
 - git
---
*.gitignore*文件中可以定义哪些文件或者文件夹不进行版本迭代，会被git忽略，常见的如前端中模块文件夹*node_modules*，在git项目开始之前时可以先定义忽略规则，当一个项目已经和代码仓库关联，被追踪时，此时修改 *.gitignore*不会生效,这是因为git在本地有缓存，。需要执行如下几个命令，才会生效

``` bash
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
git push -u origin master
```
