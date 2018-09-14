## package.json常用项

### 基础信息
* name(string), version(string)
> 名称与版本，是必填项。

* description(string), keyword(array)
> 描述与关键字，可以帮助人们在使用npm search时找到这个包。

### 配置项
* files(array)
> 被项目包含的文件名数组。控制上传到npm库所包含的文件，与.npmignore 同时起作用，作用相反。
> 某些文件总是被包含的，不论是否在规则中指定了它们。
```
package.json
README (and its variants)
CHANGELOG (and its variants)
LICENSE / LICENCE
```
> 相反地，一些文件总是被忽略
```
.git
CVS
.svn
.hg
.lock-wscript
.wafpickle-N
*.swp
.DS_Store
._*
npm-debug.log
```
* main(string)
指定模块的入口程序文件

### 参考
https://www.cnblogs.com/nullcc/p/5829218.html

