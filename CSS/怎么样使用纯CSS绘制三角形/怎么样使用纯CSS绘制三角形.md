# 怎么样使用纯CSS绘制三角形

## 1.1 实现步骤

:question::question: ​**rgba() 透明 和 transparent 透明有什么区别?** 

> ```js
> rgba(0,0,0,0);  该透明是黑色的透明
> transparent;    透明是 关键字透明, 纯透明的.
> ```

> 1. 保证元素的宽高为 0;
> 2. 元素的边框设置的要足够大
> 3. 需要显示三角形的部分, 写真实的颜色, 不需要写三角形的部分给透明色.

## 1.2 CODE(代码部分)

```js
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        #box {
            width: 0;
            height: 0;
            border-left: 100px solid cyan;
            border-top: 100px solid transparent;
            border-right: 100px solid transparent;
            border-bottom: 100px solid transparent;
        }
    </style>
</head>
<body>
<div id="box"></div>
</body>
```

## 1.3 图示如下:

![1561905225368](assets/1561905225368.png)



