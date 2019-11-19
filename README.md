# Canvas 学习笔记

---

### 0. 参考文档

[菜鸟教程](https://www.runoob.com/w3cnote/html5-canvas-intro.html)

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

**通过 moveTo 来指定绘图的起始点，之后若是封闭路径，最后的结束点会和这个起始点连接**

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
  - x1: 参考点的横坐标
  - y1: 参考点的纵坐标
  - x2: 结束点的横坐标
  - y2: 结束点的纵坐标
  - radius: 指定的半径

```javascript
context.beginPath();
context.moveTo(100, 100); // 初始点的坐标
context.arcTo(300, 100, 300, 300, 200);
context.closePath();
context.strokeStyle = '#f44';
context.stroke();
```

![Arc参考点起始点，结束点的关系](https://www.runoob.com/wp-content/uploads/2018/12/3556678928-5b74dd8f1bd2a_articlex.png)

#### 3. 画贝塞尔曲线

- 二次贝塞尔曲线 quadraticCurveTo(cp1x, cp1y, x, y)
  - cp1x: 控制点 1 的横坐标
  * cp1y: 控制点 1 的纵坐标
  * x: 结束点 的横坐标
  - y: 结束点 的纵坐标

```javascript
const canvas = document.getElementById('bezier');
const context = canvas.getContext('2d');
context.beginPath();
context.moveTo(10, 200);
context.quadraticCurveTo(40, 100, 200, 200);
context.stroke();
```

- 三次贝塞尔曲线 bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
  - cp1x: 控制点 1 的横坐标
  - cp1y: 控制点 1 的纵坐标
  - cp2x: 控制点 2 的横坐标
  - cp2y: 控制点 2 的纵坐标
  - x: 结束点的横坐标
  - y: 结束点的纵坐标

```javascript
context.beginPath();
context.moveTo(10, 50);
context.bezierCurveTo(20, 0, 60, 20, 100, 50);
context.strokeStyle = '#f44';
context.stroke();

context.beginPath();
context.moveTo(10, 50);
context.bezierCurveTo(20, 100, 60, 80, 100, 50);
context.strokeStyle = '#f44';
context.stroke();
```

### 4. 样式的修改

#### 1. 上色

1. fillStyle = color; 设置图形的填充颜色
2. strokeStyle = color; 设置图形轮廓的颜色
   **这里的 color 可以是 16 进制纯色例如: '#f44', 或者自带的颜色'red', 也可以是渐变色**
3. 渐变色设置

```javascript
var gradient = ctx.createLinearGradient(0, 0, 170, 150);
gradient.addColorStop('0', 'magenta');
gradient.addColorStop('0.3', 'blue');
gradient.addColorStop('0.6', 'red');
gradient.addColorStop('1.0', 'yellow');

ctx.strokeStyle = gradient;
ctx.lineWidth = 5;
ctx.strokeRect(20, 20, 150, 100);
```

- 通过 createLinearGradient 创建一个渐变对象，从左上角到右下角的
- addColorStop(present, color)
  - present: 颜色停止的百分比
  - color: 停止时的颜色

* 得到的 gradient 即是渐变的颜色

4. 色谱图的实现

```html
<canvas id="canvas" width="500" height="500"></canvas>
<script>
  const canvas = document.getElementById('canvas');
  const context = canvas.getContext('2d');

  context.lineWidth = 5;
  context.strokeStyle = '#333';
  context.strokeRect(100, 100, 300, 300);

  for (i = 0; i < 10; i++) {
    for (j = 0; j < 10; j++) {
      const x = 100 + (i * 300) / 10;
      const y = 100 + (j * 300) / 10;

      context.fillStyle = `rgba(${255 - (255 / 10) * i}, ${255 -
        (255 / 10) * j}, 255)`;
      context.fillRect(x, y, 300 / 10, 300 / 10);
    }
  }
</script>
```

#### 2. 线形变化

- 线宽(lineWidth)
  - ctx.lineWidth = 10; (设置线性的宽度)

* 线形(lineCap = 'butt' | 'round' | 'square')
  - butt: 末端以方形结束
  - round: 末端以圆形结束
  - square: 末端多出一个矩形区域

```javascript
const canvas = document.querySelector('#lineStyle');
const context = canvas.getContext('2d');

const capStyle = ['butt', 'round', 'square'];

capStyle.forEach((elem, i) => {
  context.beginPath();
  context.moveTo(100, 100 + i * 20);
  context.lineTo(300, 100 + i * 20);
  context.lineWidth = 10;
  context.lineCap = elem;
  const gradient = context.createLinearGradient(100, 0, 400, 0);
  gradient.addColorStop('0', '#ff4444');
  gradient.addColorStop('1', 'yellow');
  context.strokeStyle = gradient;
  context.stroke();
});
```
