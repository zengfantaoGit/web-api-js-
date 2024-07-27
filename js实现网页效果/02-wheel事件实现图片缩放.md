## wheel事件实现图片缩放。

### 1. 实现原理
通过wheel事件的wheelEvent事件对象获取绑定在容器上面的鼠标滚动量。
即是`event.deltaY`。
再将容器的wheel事件触发的默认事件阻止`e.returnValue = false`。再设置img对象上面的transform
属性来实现图片缩放。

css对应样式
```css
/*容器*/
.c1 {
      display: flex;
      margin: 300px auto;
      justify-content: center;
      align-items: center;
      width: 600px;
      height: 600px;
      border: 1px solid red;
    }
    /*容器内部图片*/
    .c1 > img {
      width: 500px;
      transition: all .3s;
      transform: scale(100%);
    }
```

html代码
```html
<body>
<div class="c1">
  <img src="../img/1.png" alt="">
</div>
</body>
```

js逻辑代码
```js
<script>
  const c1 = document.querySelector('.c1');
  const img = document.querySelector('img');
  let scale = 1
    //监听c1容器的wheel事件
  c1.addEventListener('wheel',function (e) {
    console.log(e.cancelable,e.deltaY)
    // 判断当前是否能阻止滚动默认事件
    e.cancelable && (e.returnValue = false)
    // 调整缩放比例
    scale += 0.01 * e.deltaY
    scale = Math.min(Math.max(0.125,scale), 3)
    // 调整对应样式
    img.style.transform = `scale(${scale})`
  })
</script>
```

其中scale变量来维护图片的缩放尺度。每次滚动都让scale变量变化
`0.01 * e.deltaY`大小。`scale = Math.min(Math.max(0.125,scale), 3)`
来控制scale大小位于0.125到3之间。

