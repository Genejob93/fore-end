# Stylus 学习

## 1.1 Stylus 安装使用

```js
1.// 通过 cmd 命令安装 stylus
npm install -g stylus (查看 npm 全局安装文件夹位置: npm config get prefix)

2.// 查看 stylus 安装的版本
	stylus -V     // 大写的 V

3.// vscode中 编译 stylus 文件的命令 
	 stylus -w index.styl -o css // -w 监视  -o  css是 stylus输出目录, 该目录可以为任意名字
   
   /**
   	* 注意: 要编译的 styl 文件, 编译后输出的目录是 同一级目录
   	*    如果把 styl 文件写到 css 编译输出目录里面,那么 -o 后面的输出目录需要 是绝对路径
   	*  stylus -w index.styl -o C:\WorkSpace\VSCode\03.study\day07\04-vue-app\css
    */
```

## 1.2 Stylus的使用

```js
1. // stylus 中定义变量 时一般为了区分, 变量名前要加  $ 符
	$font = 30px
  
2.// 引入 css 	
		@import "./reset.css"
	// 引入混合
		@import "./mixins.styl"


```



