# js鼠标事件 以及clientX、clientY、offsetX、offsetY、layerX、layerY、pageX、 pageY、screenX、screenY属性的区别。

### MouseEvent有以下事件：
1. mousedown 鼠标按下
2. mouseup 鼠标抬起
3. click 鼠标左键单击
4. dblclick 鼠标左键双击
5. mousemove 鼠标移动
6. mouseover 鼠标进入
7. mouseout 鼠标离开
8. mouseenter 鼠标进入
9. mouseleave 鼠标离开
10. contextmenu 鼠标右键菜单
11. wheel：滚动鼠标的滚轮时触发，该事件继承的是WheelEvent接口。

注意：
* 在鼠标点击时顺序：
mousedown --> mouseup --> click --> dblclick
* mouseover和mouseout子元素也会触发，可以冒泡触发
* mouseenter和mouseleave是针对侦听的对象触发，阻止了冒泡

### 阻止默认事件
1. `e.preventDefault()`
2. `e.returnValue = false`
3. 直接在回调函数 `return false`

去除右键菜单默认事件
```js
document.body.addEventListener('contextmenu', (e) => {  // 监听contextmenu右键菜单事件
        e.preventDefault()
    })
```

去除图像默认拖拽事件
```js
document.querySelector('img').addEventListener('mousemove', (e) => {
    e.returnValue = false
})
```
## 二，mouseEvent常见属性

### clientX与clientY属性
clientX与x属性一样。都是指从当前点击位置到浏览器左上顶点的x距离.  
且这个距离与滚动条距离无关。
![图片](https://i-blog.csdnimg.cn/blog_migrate/a0a71b8e69e99832502456c3c32de5b9.png)

### offsetX与offsetY属性
offsetX属性只会描述点击点到目标元素`(e.target)`的左上顶点的x距离。  
且目标元素的左顶点只会包含width与padding不包含border。  
且这个距离与滚动条距离无关。

### layerX与layerY属性
layerX,layerY 往上找有`定位属性的父元素`的左上角（自身有定位属性的话就是相对于自身），都没有的话，就是相对于`body`的左上角。
且这个左上顶点是包含`border`的

### button属性 （只读）
`mouseEvent.button`返回一个整数值（0，1，2）
* 0 代表左键
* 1 代表滚轮键
* 2 代表右键

### buttons属性 （只读）
属性返回一个三个比特位的值，表示同时按下了哪些键。它用来处理同时按下多个鼠标键的情况。该属性只读。
* 001 表示左键
* 010 表示右建
* 100 表示中键


## 三，mouseEvent事件详解

### wheel事件
wheel事件触发时机：鼠标滚轮滚动时触发。（直接拖动滚动条不会触发）     
且window对象的wheel事件自身的默认事件不能阻止`e.cancelable = false`

### wheel事件属性
1. e.deltaX：表示触发此次事件中x方向的滚动量。
2. e.deltaY：表示触发此次事件中y方向的滚动量。（正值表示向下滚动，负值表示向上滚动）
3. e.deltaZ：表示触发此次事件中z方向的滚动量。

