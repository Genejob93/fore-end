# Mint-ui

> [Mint-ui官网](http://mint-ui.github.io/#!/zh-cn) 
>
> quick-start查看案例使用 
>
> Mint-ui 主要使用于移动端的ui框架

# 1.1 Mint-ui 简单实用

> 1. **Mint-ui 的使用**
> 2. **Mint-ui 的按需引入**

> **Mint-ui的使用**

```js
1.// 安装 Mint-ui
	npm install --save mint-ui

2. 在 main.js 中引入 
	import Vue from 'vue'
	import MintUI from 'mint-ui'
	import 'mint-ui/lib/style.css'
	import App from './App.vue'
	// 声明使用插件
	Vue.use(MintUI)

```

> **Mint-ui 的按需引入**
>
> 借助 [babel-plugin-component](https://github.com/QingWei-Li/babel-plugin-component)，我们可以只引入需要的组件，以达到减小项目体积的目的。

> 1. **安装: babel-plugin-component**

```js
  //1. 首先，安装 babel-plugin-component：
  npm install babel-plugin-component -D
```

> 2. **修改 .babellrc文件配置**

```js
//2. 然后，将 .babelrc 修改为：
	{
  "presets": [
    ["es2015", { "modules": false }]
  ],
  "plugins": [["component", [
    {
      "libraryName": "mint-ui",
      "style": true
    }
  ]]]
}
```

> 3. **按需引入使用组件即可：** 

```js
//3. 然后再全局中(main.js文件) 按需引入即可使用Mint-ui

import { Button } from 'mint-ui';
// 注册使用组件
Vue.component(Button.name, Button);

// 然后在需要使用该组件的地方使用即可
	<mt-button type="default">default</mt-button>
	<mt-button type="primary">primary</mt-button>
	<mt-button type="danger">danger</mt-button>
```































