---
title: 详解用electron和react开发桌面软件
date: 2016-06-21 09:17:32
tags:
---

## 提纲

本文主要介绍`https://github.com/chentsulin/electron-react-boilerplate.git`该项目的使用方法和里面的一些配置的说明

## package.json说明

### `devDependencies`和`dependencies`的区别

1. [`dependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#dependencies) 里的包，会在下面的情况下安装
   1. 在包含`package.json`的文件夹里执行`npm install`
   2. 在任何路径下执行`npm install $package`
2. [`devDependencies`](https://github.com/npm/npm/blob/2e3776bf5676bc24fec6239a3420f377fe98acde/doc/files/package.json.md#devdependencies) 的情况：
   1. 在包含`package.json`的文件夹里执行`npm install`，如果加上`--production`参数就不安装了。
   2. 在任何路径下执行`npm install $package`都不会安装，除非加上`--dev`参数才会安装
3. 总结
   1. `devDependencies`用于开发过程中需要而运行程序不需要的包，比如单元测试、babel转码等需要的包
   2. `dependencies`用于运行程序需要的包，及上线后依赖的包
4. 补充
   1. `npm install $package --save`会把该包添加到package.json的`dependencies`字段里。`npm uninstall $package --save`将会从package.json中移除改包
   2. `npm install $package --save-dev`会把改包添加到package.json的`devDependencies`字段里。`npm unintall $package --save-dev`将会移除改包。

### `node -r babel-register server.js`里面的`-r babel-register `什么意思

1. [babel-register](https://babeljs.io/docs/usage/require/) 是什么？

   执行`require("babel-register");`后，所有的被node require的`.es6`, `js`, `jsx`, `es`文件都会先被babel转化，这样你就可以在server.js里肆无忌惮地使用es6语法了，比如node6.0不支持的`import * from XXX`语法

   前往不要忘了配置[.babelrc](https://babeljs.io/docs/usage/babelrc/)文件，

2. `node -r module`是什么意思？

   node在运行脚本之前先预加载module，类似于`require('module')`

3. 总结

   `node -r babel-register server.js`意思是，在执行server.js前先加载`babel-register`，然后这样执行`server.js`之前会先用babel转化`server.js`。