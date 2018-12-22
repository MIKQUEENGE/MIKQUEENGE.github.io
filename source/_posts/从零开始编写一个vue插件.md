---
title: 从零开始编写一个vue插件
toc: true
date: 2018-12-17 10:54:29
categories:
- Web
tags:
- vue
- mathjax
---

写毕设的时候需要一个mathjax编辑器，因此直接写一个插件试一下。
<!-- more -->

## 准备账号
进入[npm](https://www.npmjs.com/signup)注册账号

## 初始化项目
```shell
vue init webpack-simple mathjax-toolbar
cd mathjax-toolbar
npm install
```

得到的项目内的`/src`结构如下：
```
src/
├── assets
│   └── logo.png
├── App.vue
└── main.js
```

## 创建插件相关文件
- 在`src/`下创建插件文件夹`plugin/`
- 进入`plugin/`目录
- 创建插件的Vue组件文件`mathjaxToolbar.vue`
- 创建插件的入口文件`index.js`

创建后`src/`目录为：
```
src/
├── assets
│   └── logo.png
├── main.js
├── App.vue
└── plugin
    ├── index.js
    └── mathjaxToolbar.vue
```

## 编写插件入口文件`index.js`
```js
'use strict';

import mathjaxToolbar from './mathjaxToolbar.vue'

const VueMathjaxToolbar = {
  install (Vue) {
    Vue.component('math-toolbar', mathjaxToolbar)
  }
}

export default VueMathjaxToolbar
```
## 在`src/main.js`中注册插件组件并使用
添加如下代码：
```js
import mathjaxToolbar from './plugin/index.js'
Vue.use(mathjaxToolbar)
```

## 修改`src/App.vue`
```vue
<template>
  <div id="app">
    <mathjax-toolbar></mathjax-toolbar>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
    }
  }
}
</script>

<style>
</style>
```

## 编写插件组件`mathjaxToolbar.vue`
这里不再列出，有兴趣的可以在github查看代码：
[mathjaxToolbar.vue]()

## 关于在Vue组件中跨域引入第三方js或cdn
如果想要引入第三方js，假设为`https://xxx.js`
在`mounted`中添加：
```js
const mScript = document.createElement('script')
mScript.type = 'text/javascript'
mScript.src = 'https://xxx.js'
document.body.appendChild(mScript)
```

## 更新package.json
删除"private": true
添加：
```json
"main": "dist/mathjaxEditor.js",
"repository": {
  "type": "git",
  "url": "https://github.com/zmj97/mathjax-toolbar"
},
"keywords": [
  "javascript",
  "vue",
  "mathjax",
  "toolbar",
  "html"
]
```

## 更新webpack.config.json
```js
// 修改entry
entry: './src/plugin/index.js',
output: {
  path: path.resolve(__dirname, './dist'),
  publicPath: '/dist/',
  // 修改
  filename: 'mathjaxEditor.js',
  // 添加
  library: 'mathjax-toolbar',
  libraryTarget: 'umd',
  umdNamedDefine: true
}
```

## build与发布
```bash
npm run build
# 登录npm账号
npm login
# 发布
npm publish
```

## 更新包
```bash
# 更新版本号，如1.0.1
npm version 1.0.1
# 发布
npm publish
```