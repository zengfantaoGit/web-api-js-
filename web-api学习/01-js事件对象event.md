## JS 之 事件Event对象详解（属性、方法、自定义事件）

### 一. Event对象是什么？

定义：事件`event`对象是在浏览器中触发事件时。
浏览器会自动创建的`event`对象。其中`event`对象包含此次
事件操作的信息，包括事件类型，事件目标，触发事件。还有
触发事件时鼠标的坐标等。

当触发事件。浏览器会将此次生成的`event`对象作为参数设置在
用户的绑定回调函数上。我们可以通过`event`对象来获取此次操作的
具体信息。

* 事件可以由用户触发，例如：**鼠标事件**，**键盘事件**等。  
* 事件也可以由js脚本触发，例如`dom.click()`会触发绑定在dom元素上的
点击事件。  
* 还可以由API生成，例如：动画完成后触发对应事件、视频播放被暂停时触发对应事件；  
* 最后还可以通过自定义事件来进行触发。

下面是一次触发mouseup事件后，浏览器生成的event对象
`
MouseEvent = {isTrusted: true, screenX: 312, screenY: 533, clientX: 469, clientY: 646,
altKey: false
bubbles: true // 判断事件是否会事件冒泡时触发事件
button: 0
buttons: 0
cancelBubble: false
cancelable: true
clientX: 469
clientY: 646
composed: true
ctrlKey: false
currentTarget: null
defaultPrevented: false
detail: 1
eventPhase: 0
fromElement: null
isTrusted: true
layerX: 469
layerY: 796
metaKey: false
movementX: 0
movementY: 0
offsetX: 461
offsetY: 211
pageX: 469
pageY: 796
path: (6) [div.content, div#app, body, html, document, Window]
relatedTarget: null
returnValue: true
screenX: 312
screenY: 533
shiftKey: false
sourceCapabilities: InputDeviceCapabilities {firesTouchEvents: false}
srcElement: div.content
target: div.content
timeStamp: 11837.065000087023
toElement: div.content
type: "mouseup"
view: Window {window: Window, self: Window, document: document, name: "", location: Location, …}
which: 1
x: 469
y: 646
__proto__: MouseEvent
`

### 二，event事件对象常用属性

1. event.bubbles（只读）
该属性值为boolean值，表示当前事件是否在冒泡时触发。


2. event.cancelBubble
该属性值为boolean值，表示是否阻止当前事件的继续冒泡/捕获。
默认为false，设置为true则会阻止当前事件冒泡/捕获。当前该属性已经被
`event.stopPropagetion()`取代。
```js
const body = document.body
  const div = document.querySelector('div');
  body.addEventListener('click', (e) => {
    console.log('body')
    e.cancelBubble = true
  },true)
  div.addEventListener('click',(e) => {
    console.log(e)
  },true)
```
上述代码中，给`body`与`div`元素设置了click事件。
且均在事件捕获阶段触发。由于在`body`元素中设置`e.cancelBubble = true`
会阻止事件继续捕获导致div事件不触发。

3. event.cancelable (只读)
该属性值为`boolean`值，表示当前事件的默认行为是否可以被取消。
例如点击**超链接的页面跳转**，**拖拽图片**。即是是否能用`event.preventDefault()`
取消默认事件

4. event.currentTarget (只读)
该属性是一个dom元素。表示当前事件所绑定的dom元素。
要注意的是。该属性值只能在事件触发时被`event`对象调用。
如果在`console.log(event)`输出后，此时的currentTarget会是null。
可以设置`debugger`断点进行观察

5. event.target (只读)
该属性是一个dom元素。表示触发当前事件的那个DOM元素。
但`target`的值为触发事件的DOM，`currentTarget`的值为绑定事件的DOM。

```js
const body = document.body
    body.addEventListener('click', function (e) {
        console.log('currentTarget',e.currentTarget)  // body
        console.log('target',e.target)  // div
    }, true)
    const div = document.querySelector('div');
```

上述代码中，若点击`div`元素，`currentTarget`会显示绑定dom(body)。
而`target`会显示触发dom的元素(div)

6. **event.defaultPrevented（只读）**
该属性值为布尔值，表示当前事件是否调用了event.preventDefault()方法，从而阻止了浏览器的默认行为，true表示已经调用过，false表示还未调用。


7. **event.returnValue**
该属性为boolean值，通过控制它的值来判断是否该触发默认事件。
类似 event.preventDefault() + event.defaultPrevented 组合

8. **event.eventPhase**
该属性为整数值（0，1，2，3），每个取值表示当前事件处理的阶段。
共分成四个阶段。如下表所示：

| 常量 | 值 | 描述 |
| :---: | :---: | :---: |
| `Event.NONE` | 0 | 这个阶段，没有事件正在被处理 |
| `Event.CAPTURING_PHASE` | 1 | 这个阶段是指事件捕获的过程，事件正在被目标元素的祖先对象处理，是从最外层的祖先元素到目标元素的过程，从`Window`、`Document`、…、目标元素的过程。|
| `Event.AT_TARGET` | 2 | 这个阶段是指到达目标元素的过程。如果 `Event.bubbles` 的值为 `false`，即事件不会冒泡，则对事件对象的处理在这个阶段后就会结束。|
| `Event.BUBBLING_PHASE` | 3 | 这个阶段是指事件冒泡的过程，从从目标元素到最外层的祖先元素的过程。 |

```js
const body = document.body
    body.addEventListener('click', function (e) {
        console.log(e.eventPhase,'body')  // 1
    }, true)  // body点击事件在捕获阶段触发
    const div = document.querySelector('div');
    div.addEventListener('click', (e) => {
        console.log(e.eventPhase,'div') // 触发的目标元素，2
    }, false)  // 冒泡时触发
```

事件流执行过程：
![图片](https://i-blog.csdnimg.cn/blog_migrate/ee74a595096785a871bd4d0a86231d70.png)

9. event.type (只读)
该属性值为string，用来判断当前事件触发的类型，不区分大小写。

测试代码：
```js
<div style="width: 200px;height: 30px;" id="div3">
  点击获取事件的type属性
</div>

<script>
  // 获取事件的type属性
    document.querySelector('#div3').addEventListener('click', function (e) {
      console.log('事件的type属性---', e.type)  //click
    })

    window.addEventListener('mousedown',(e) => {
    console.log(e.type,'window')  // mousedown
})
</script>
```

### 四. 常用方法
1. **event.composedPath()**
该方法用来获取当前事件的事件传播路径，从触发元素到最外层Window，而且阻止冒泡的方法不会影响到事件的传播路径。如果Shadow DOM根节点触发事件，  
并且ShadowRoot.mode是关闭的，则获取的路径中将不包括Shadow DOM节点。

2. **event.preventDefault()**
该方法执行后会阻塞当前元素触发后的默认事件。例如`超链接的跳转事件`。
但不会对事件传播造成影响。与`event.returnValue`作用相同

3. **event.stopPropagation()**
该方法用来阻止当前事件在捕获阶段和冒泡阶段中的传播，如果点击了子元素，
但是在子元素中阻止了事件的冒泡，那么父元素对应的事件不会被触发；
如果点击了子元素，但是在父元素中阻止了事件的捕获传播，那么子元素对应的事件将不会被触发。
与`event.cancelBubble`作用相同。


### 五。总结

#### 1. 阻止事件冒泡的方式
* e.stopPropagation()
* e.cancelBubble = true （未来可能废弃）
* stopImmediatePropagation方法：阻止事件冒泡并且阻止该元素上同事件类型的监听器被触发

### 2. 阻止默认事件触发
1. e.preventDefault()
2. e.returnValue = false
3. return false, 直接结束对应操作






