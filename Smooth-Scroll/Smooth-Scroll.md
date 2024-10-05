# Smooth-Scroll

>使用js完成**平滑滚动**
>
>当我们单击它时，他会平滑的向下滚动到另一个部分，在您的页面上或者另一个锚链接上

## 初始准备阶段

html的代码

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="./style.css">
    <title>Document</title>
</head>

<body>
    <div class="container">
        <a class="first" href="#">Bring me down</a>
    </div>
    <div class="container2">
        <a class="last" href="#">Thanks</a>
        <img src="./cd.jpg" alt="">
    </div>

    <script src="./app.js"></script>
</body>

</html>
```

css代码

```css
.container {
  height: 100vh;
  background-color: lightblue;
  display: flex;
  justify-content: center;
  align-items: center;
}

.container2 {
  height: 100vh;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

img {
  height: 600px;
  width: 1200px;
}
```

## Js代码的编写

```js
function smoothScroll(target, duration) {
  var target = document.querySelector(target);
  var targetPosition = target.getBoundingClientRect().top;
  var startPosition = window.pageYOffset || window.scrollY;
  console.log(startPosition)
  var distance = targetPosition - startPosition;
  var startTime = null;

  function loop(currentTime) {
    if (startTime === null) startTime = currentTime;
    var timeElapsed = currentTime - startTime;
    var run = ease(timeElapsed, startPosition, distance, duration);
    window.scrollTo(0, run);
    if (timeElapsed < duration) requestAnimationFrame(loop);
  }
  function ease(t, b, c, d) {
    t /= d / 2;
    if (t < 1) return c / 2 * t * t + b;
    t--;
    return -c / 2 * (t * (t - 2) - 1) + b;
  }
  requestAnimationFrame(loop);
}

var first = document.querySelector('.first')
var last = document.querySelector('.last')

first.addEventListener('click',function (){
  smoothScroll('.last',1000)
})
last.addEventListener('click',function (){
  smoothScroll('.first',1000)
})
```

> **解读**
>
> - 参数
>   - target : 移动到的目标位置
>   - duration : 动画的持续时间
> - `var target = document.querySelector(target);`
>   - 使用选择器找到目标位置
>   - ![image-20241005173209996](https://cdn.jsdelivr.net/gh/Killer-89757/PicBed/images/2024%2F10%2Fimage-20241005173209996-3feb78.png)
> - `var targetPosition = target.getBoundingClientRect().top;`
>   - 得到元素的边界矩形的位置信息
>   - ![image-20241005173735522](https://cdn.jsdelivr.net/gh/Killer-89757/PicBed/images/2024%2F10%2Fimage-20241005173735522-bec8ca.png)
>   - 这里我们是要移动到这个元素的顶部，所以选择使用top
> - `var startPosition = window.pageYOffset || window.scrollY;`
>   - 返回文档在垂直方向已滚动的像素值
> - `var distance = targetPosition - startPosition;`
>   - 计算滚动的距离
> - requestAnimationFrame(loop)
>   - ![image-20241005185947155](https://cdn.jsdelivr.net/gh/Killer-89757/PicBed/images/2024%2F10%2Fimage-20241005185947155-940c91.png)
> - ![image-20241005183124599](https://cdn.jsdelivr.net/gh/Killer-89757/PicBed/images/2024%2F10%2Fimage-20241005183124599-dece87.png)
> - ![image-20241005184800948](https://cdn.jsdelivr.net/gh/Killer-89757/PicBed/images/2024%2F10%2Fimage-20241005184800948-1f5eeb.png)
> - ![image-20241005183819515](https://cdn.jsdelivr.net/gh/Killer-89757/PicBed/images/2024%2F10%2Fimage-20241005183819515-3f7aec.png)

不会的信息直接取[MDN](https://developer.mozilla.org/zh-CN/)中查找API学习

1. [getBoundingClientRect](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect) 的学习

   - 参数

     - 无。

   - 返回值

     - 返回值是一个 [`DOMRect`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMRect) 对象，是包含整个元素的最小矩形（包括 `padding` 和 `border-width`）。该对象使用 `left`、`top`、`right`、`bottom`、`x`、`y`、`width` 和 `height` 这几个以像素为单位的只读属性描述整个矩形的位置和大小。除了 `width` 和 `height` 以外的属性是相对于视图窗口的左上角来计算的。

     - ![image-20241005190509630](https://cdn.jsdelivr.net/gh/Killer-89757/PicBed/images/2024%2F10%2Fimage-20241005190509630-a3322c.png)

2. `window.pageYOffset || window.scrollY` 直接写 `window.scrollY `即可

   - ![image-20241005174544399](https://cdn.jsdelivr.net/gh/Killer-89757/PicBed/images/2024%2F10%2Fimage-20241005174544399-85dd1d.png)
   - 返回文档在垂直方向已滚动的像素值。

3. [window.requestAnimationFrame()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)
   - `window.requestAnimationFrame()` 方法会告诉浏览器你希望**执行一个动画**。它要求浏览器在下一次重绘之前，调用用户提供的回调函数。
   - 对回调函数的调用频率通常与显示器的刷新率相匹配。虽然 75hz、120hz 和 144hz 也被广泛使用，但是最常见的刷新率还是 60hz（每秒 60 个周期/帧）。
   - **备注：**若你想在浏览器下次重绘之前继续更新下一帧动画，那么回调函数自身必须再次调用 `requestAnimationFrame()`。`requestAnimationFrame()` 是**一次性**的。

4. easing 动画的缓动方式

   - https://nicmulvaney.com/easing

   - ![image-20241005180743724](https://cdn.jsdelivr.net/gh/Killer-89757/PicBed/images/2024%2F10%2Fimage-20241005180743724-a2bf5c.png)

5. [Window：scrollTo()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/scrollTo)

   - **`Window.scrollTo()`** 会滚动到文档中的一组特定坐标

   - ```
     scrollTo(x-coord, y-coord)
     scrollTo(options)
     ```

   - 参数
     - `x-coord` 是你希望显示在左上角的文档水平轴像素。
     - `y-coord` 是你希望显示在左上角的文档垂直轴像素。

   - `options`
     - 包含以下参数的字典：
     - `top`指定沿 Y 轴滚动窗口或元素的像素数量。
     - `left`指定沿 X 轴滚动窗口或元素的像素数量。
     - `behavior`确定滚动是即时完成还是以平滑动画进行。该选项是一个字符串，必须取以下值之一：
       - `smooth`：滚动应该平滑地进行动画展示
       - `instant`：滚动应在一次跳转中即时完成
       - `auto`：滚动行为由 `scroll-behavior` 的计算值来决定