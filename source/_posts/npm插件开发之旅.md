---
title: 'npm插件开发之旅'
date: 2017-01-31 15:28:07
categories:
- node
tags:
- npm
- node
- JavaScript
---
### npm插件

刚开始学习node，年前试着写了一个插件，主要是熟悉AMD，require模块化，node的http模块及npm插件的开发流程。

插件本身用了http模块请求数据，并且引入一个转码模块对输入进行转码，然后对res进行简单处理。


然后复盘一下用到的知识点和遇到的坑。
<!-- more -->

### `require`、`exports`、`module`
#### `require`的路径写法

`require`函数用于在当前模块中加载和使用别的模块，传入一个模块名，返回一个模块导出对象。模块名可使用相对路径（以./开头），或者是绝对路径（以/或C:之类的盘符开头）。另外，模块名中的.js扩展名可以省略。以下是一个例子。
```js
var foo1 = require('./foo');
var foo2 = require('./foo.js');
var foo3 = require('/home/user/foo');
var foo4 = require('/home/user/foo.js');

// foo1至foo4中保存的是同一个模块的导出对象。
```

#### exports
exports对象是当前模块的导出对象，用于导出模块公有方法和属性。别的模块通过require函数使用当前模块时得到的就是当前模块的exports对象。以下例子中导出了一个公有方法。
```js
exports.hello = function () {
    console.log('Hello World!');
};
```
#### module

通过module对象可以访问到当前模块的一些相关信息，但最多的用途是替换当前模块的导出对象。例如模块导出对象默认是一个普通对象，如果想改成一个函数的话，可以使用以下方式。
```js
module.exports = function () {
    console.log('Hello World!');
};
```

以上代码中，模块默认导出对象被替换为一个函数。


### 插件的工程目录结构
```
- /home/user/workspace/node-echo/   # 工程目录
    - bin/                          # 存放命令行相关代码
        node-echo
    + doc/                          # 存放文档
    - lib/                          # 存放API相关代码
        echo.js
    - node_modules/                 # 存放三方包
        + argv/
    + tests/                        # 存放测试用例
    package.json                    # 元数据文件
    README.md                       # 说明文件
```
### 添加package.json的bin字段
```
 "bin": {
    "hi": "./bin/hi.js"
  },
```
这样`npm install -g`安装后会自动生成`.cmd`文件

### URLencode编码

测试接口的时候一直报下面的错误
```
{
	"reason":"info参数错误",
	"result":null,
	"error_code":211201
}
```
仔细看了API文档以后，发现接口不能接受中文字符，需要进行转码，然而并不知道要使用哪种转码方式，经过吐血的测试以后，终于确定是URLencode转码。封装成一个`urlencode`模块
```js
var urlencode = require('urlencode');
```


### 在npm发布

如果报错是这样
```
no_perms Private mode enable, only admin can publish this module
```
使用下面代码重置npm镜像
```
npm config set registry http://registry.npmjs.org
```

### 最后一个坑

本地测试正常，npm发布后再`npm install -g`安装执行就报错
```
 command not found
```
着实纠结好久，最后试着是在入口文件中加
`#!/usr/bin/env node`

虽然可以了，但是原理不太理解，`#!/usr/bin/env node`是Linux系统下，在shell脚本中，通过#!注释来指定当前脚本使用的解析器。


### 参考文章

> [npm插件开发流程](https://my.oschina.net/ubuntuvim/blog/540494)

> [七天学会NodeJS](https://nqdeng.github.io/7-days-nodejs/)