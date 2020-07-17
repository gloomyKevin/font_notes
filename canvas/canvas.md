解决问题



vs code不能自动提示canvas代码，需要在获取canvas的dom元素代码前添加一行/**@type HTMLCanvasElement*/



要设置画布的大小，只有这两种方法(属性值单位必须为px，否则忽略)

1. 在`<canvas>`标签中设置；

```javascript
<canvas id="canvas" style="border: 1px solid #aaaaaa; display: block; margin: 50px auto;">
```

<canvas class="canvas" style="border: 1px solid #aaaaaa; display: block; margin: 50px auto;" width="800" height="600">

1. 在JS代码中设置`canvas`的属性

```javascript
canvas.width = 800;
canvas.height = 600;
```

tips

- 不要用 CSS 控制它的宽和高,会走出图片拉伸，
- 重新设置 canvas 标签的宽高属性会让画布擦除所有的内容。
- 可以给 canvas 画布设置背景色



兼容性

- ie9 以上才支持 canvas, 其他 chrome、ff、苹果浏览器等都支持
- 只要浏览器兼容 canvas，那么就会支持绝大部分 api(个别最新 api 除外)
- 移动端的兼容情况非常理想，基本上随便使用
- 2d 的支持的都非常好，3d（webgl）ie11 才支持，其他都支持
- 如果浏览器不兼容，最好进行友好提示



复用lineTo以绘制折线

```javascript
context.moveTo(100,100);
        context.lineTo(300,300);
        context.lineTo(100,500);
        context.lineWidth = 5;
        context.strokeStyle = "#AA394C";
        context.stroke();
```

复用moveTo以绘制多条折线

```javascript
context.moveTo(100,100);
        context.lineTo(300,300);
        context.lineTo(100,500);
        context.lineWidth = 5;
        context.strokeStyle = "red";
        context.stroke();
        context.moveTo(300,100);
        context.lineTo(500,300);
        context.lineTo(300,500);
        context.lineWidth = 5;
        context.strokeStyle = "blue";
        context.stroke();
        context.moveTo(500,100);
        context.lineTo(700,300);
        context.lineTo(500,500);
        context.lineWidth = 5;
        context.strokeStyle = "black";
        context.stroke();
```



创建矩形

- 语法：ctx.rect(x, y, width, height);
- 解释：x, y是矩形左上角坐标， width和height都是以像素计
- rect方法只是规划了矩形的路径，并没有填充和描边。
- 改造案例：04填充矩形.html
- rect: abbr. 矩形（rectangular）；收据（receipt）



canvas封装常用的绘制函数

以绘制矩形为例，绘制矩形中要用到的数据

1. 矩形的 x、y坐标
2. 矩形的宽高
3. 矩形的边框的线条样式、线条宽度
4. 矩形填充的样式
5. 矩形的旋转角度
6. 矩形的缩小放大