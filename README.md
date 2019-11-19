# Canvas 学习笔记

---

### 1. 兼容性以及获取上下文

```html
<script>
  var canvas = document.getElementById('plot');
  let ctx = null;
  canvas.getContext &&
    (() => {
      console.log(canvas.getContext('2d'));
      ctx = canvas.getContext('2d');
    })();
</script>
```

- 创建一个长宽固定的 canvas 标签
- 通过 id 选择器选择其标签
- 然后通过检测其 getContext 是否存在，来判断 canvas 标签是否支持 canvas 标签
- 这里得到的 ctx 对象即为我该 canvas 标签的上下文, 之后对其进行操作即可

### 2. 画一个矩形

```html
<canvas id="plot" width="300" height="300"></canvas>
<script>
  var canvas = document.getElementById('plot');
  canvas.getContext &&
    (() => {
      const ctx = canvas.getContext('2d');
      /* 先设置一个描边的样式stroke */
      ctx.strokeStyle = '#f44';
      ctx.lineWidth = 3;
      /*画出描边的图形*/
      ctx.strokeRect(10, 10, 55, 50);
      ctx.fillStyle = '#eee';
      ctx.fillRect(10, 10, 55, 50);
    })();
</script>
```

- 使用 canvas 画图形: 1.使用设置对应的画笔样式 2. 用对应的画笔样式画出图形
- stoke\*描边的图形
- fill\*实心的图形
- 这里的初始的坐标点是将绘制 canvas 画布的左上角作为原点的，向右和下分别为 x, y 的正坐标
- 这里画矩形的 strokeRect 参数为 矩形左上角点的的 x, y 离原点的距离以及长和宽

### 3. 路径的绘制

- 新建一条路径 (beginPath)
- 确定路径的起点 (moveTo(x, y))
- 调用绘制方法绘制路径(直线曲线) (arc, lineTo)
- 封闭路径 (closePath) -> 这一步如果需要画出闭合的图像可用，如果是一个非封闭图形，可以不用
- 用描边 stroke() 或者 填充 fill()方法来进行渲染

> Notes: 如果使用的是 fill 来进行渲染，那么一定是自动生成闭合图形后进行填充的

#### 1. 直线绘制

```html
<canvas id="plot" width="300" height="300"></canvas>
<script>
  var canvas = document.getElementById('plot');
  canvas.getContext &&
    (() => {
      const context = canvas.getContext('2d');
      context.beginPath();
      context.moveTo(30, 30);
      context.lineTo(50, 40);
      context.lineTo(40, 50);
      // 非封闭图形
      // context.closePath();

      context.strokeStyle = '#f44';
      context.stroke();
    })();
</script>
```

#### 2. 弧形绘制

- arc(x, y, r, startAngle, endAngle, anticlockwise)
  - x: 圆心的 x 轴坐标
  - y: 圆心的 y 轴坐标
  - startAngle: 开始的弧形弧度 (角度 / 180 \* Math.PI)
  - endAngle: 结束的弧形弧度 (角度 / 180 \* Math.PI)
  - anticlosewise: 顺逆时针(true: 逆时针, false: 顺时针)

startAngle 为 0 的位置在以圆心为 x 轴的 x 轴上

```javascript
context.beginPath(0);
context.moveTo(100, 100);
context.arc(100, 100, 50, (Math.PI / 180) * 30, (Math.PI / 180) * 120, false);
context.closePath();
context.strokeStyle = '#f44';
context.stroke();
```

- arcTo(x1, y1, x2, y2, radius)
  - x1: 起始点的横坐标
  - y1: 起始点的纵坐标
  - x2: 结束点的横坐标
  - y2: 结束点的纵坐标
  - radius: 指定的半径
