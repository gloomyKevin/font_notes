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



