[TOC]

# React 项目

## 1.1 创建 React项目

> 1. **下载脚手架**:  **`create-react-app  项目名`**   
>2.  **cd 进入项目目录:** `npm run start`  启动项目 
> 3.  **react 项目打包命令:**  `npm run build` 会在 项目下创建一个 build 目录
> 4.  **运行打包后的react项目:**  `serve build` 

## 1.2 AntD 组件的安装使用

> [AntD 官网](https://ant.design/docs/react/introduce-cn)   
>
> ```js
> // 1. 安装 antd
> 	yarn add antd
> // 2. 修改 src/App.js，引入 antd 的按钮组件。
>   import Button from 'antd/es/button';
> 	或者: import {Button} from 'antd';
> 
> // 3. 修改 src/App.css，在文件顶部引入 antd/dist/antd.css。
> 	@import '~antd/dist/antd.css';
>    或者:  import 'antd/dist/antd.css';
> ```
>
> **AntD 样式的按需引入**  
>
> > 1. AntD 的高级设置里详细介绍
>
> ```js
> // 下载如下包	
> 	yarn add react-app-rewired customize-cra
> ```
>
> > 2. 在项目点的根目录下创建一个 `config-overrides.js`  用于修改默认配置
>
> ```js
> module.exports = function override(config, env) {
>   // do stuff with the webpack config...
>   return config;
> };
> ```
>
> >3. 使用 `babel-plugin-import`  插件
> >
> >​       安装:  `yarn add babel-plugin-import`
>
> > 4. 修改 config-overrides.js
> >
> > ```js
> > const { override, fixBabelImports } = require('customize-cra');
> > 
> >  module.exports = override(
> >    // 使用 import的babel插件, 针对 antd 进行按需打包
> >    fixBabelImports('import', {
> >      libraryName: 'antd',
> >      libraryDirectory: 'es',
> >      style: 'css',  // 自动打包相关的 css 文件
> >    }),
> >  );
> > ```
> >
> > 