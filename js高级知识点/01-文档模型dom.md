# dom文档模型操作

## 1. 什么是dom

定义：dom(`document object model`)文档对象模型，标记语言文档的各个部分都封装成了对象。
DOM 将文档解析为一个由节点和对象（包括元素、文本等）组成的结构树。
可以通过调用这些对象来获取对应节点的属性。对标记语言进行增删改查的操作。

备注：`document`对象是属于`window`对象的,可以使用window.document访问

## 2.获取dom元素有哪些方法？

| 方法                                     | 描述               | 返回值                |
|----------------------------------------|------------------|--------------------|
| document.getElementById(id)            | 根据元素id值获取dom节点   | dom对象              |
| document.getElementsByTagName(tagName) | 根据元素标签名获取dom节点   | 符合条件的dom类数组        | 
| document.getElementsByClassName(class) | 通过class获取dom     | 符合条件的所有dom对象组成的类数组 |
| document.getElementsByName(name)       | 通过标签的属性name获取dom | 符合条件的所有dom对象组成的类数组 |
| document.querySelector(选择器)            | 通过选择器获取dom       | 获取第一个符合条件的dom对象    |
| document.querySelectorAll(选择器)         | 通过选择器获取dom       | 符合条件的dom类数组        |

备注：获取`html`标签可以直接用`document.documentELement`获取dom对象。
`body`标签可以用`document.body`属性获得。

## 3. 操作dom元素的方法/属性

### 3.1 创建一个dom元素

1. `document.createElement(标签名)`创建一个dom节点
2. `document.createTextNode(文本内容）`生成文本节点

```js
// 创建dom节点。返回一个dom节点
const div = document.createElement('div');
// 创建文本节点
const text = document.createTextNode("创建文本节点");
```

### 3.2 添加一个dom元素

1. `fatherEle.appendChild(ele)`向父节点的末尾添加一个子节点
2. `fatherEle.innerHtml = html文本`修改父节点内部的html内容
   备注：`fatherEle.innerHtml = ''`会清空父元素的html内容
3. `fatherEle.insertBefore(ele,oldEle)`向`oldEle`开头插入`ele`节点

```js
const ul = document.querySelector('ul');
// 创建dom节点。返回一个dom节点
const div = document.createElement('div');
const second = document.querySelector('ul > li:nth-of-type(2)');
// 创建文本节点
const text = document.createTextNode("创建文本节点");
// console.log(div)
ul.appendChild(text)
ul.appendChild(div)
div.style.height = '100px'
div.innerText = 'haru'
ul.insertBefore(div, second)
```

### 3.3 删除一个dom元素

1. fatherDom.removeChild(dom)

### 3.4 克隆一个dom元素

1. eleDom.cloneNode(boolean)
   内部的布尔值来判断是否要克隆子代节点，false不用克隆只要本体，true克隆后代。

html代码

```html

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
```

js代码

```js
const ul = document.querySelector('ul');
const node = ul.cloneNode(false);  // <ul></ul>
const node = ul.cloneNode(true);  // <ul>...</ul>
```

### 3.5 获取dom元素的属性

`eleDom.getAttribute`(属性名)
// 根据属性名获取对应属性值

```html

<body>
<ul>
    <li class="haru" num="1">1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
<script>
    const ul = document.querySelector('ul');
    const li = ul.children[0]
    console.log(li.getAttribute('class'))  // haru
    console.log(li.getAttribute('num'))  // 1
</script>
```

### 3.6 设置dom元素的属性

1. eleDom.setAttribute(属性名，属性值)
   为当前的eleDom元素添加`属性名：属性值`且属性值只能是字符串

### 3.7 移除dom元素的属性

1. eleDom.removeAttribute(属性名)

### 3.8 获取dom元素的所有属性名

1. `eleDom.attributes`属性会返回一个关于该dom元素所有属性的`NamedNodeMap`对象。
   内部有所有该元素的属性名。

### 3.9 获取dom元素的首个子元素`element.firstElementChild`

```html

<ul>
    <li class="haru" num="1">1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
<script>
    const ul = document.querySelector('ul');
    console.log(ul.firstChild)  // #text
    console.log(ul.firstElementChild)  // li
</script>
```

### 3.10 获取/设置dom元素的class属性

1. `eleDom.className`可以获取当前dom元素设置的`class属性`, 返回一个空格分隔的字符串
2. `eleDom.classList`返回一个由类名组成的数组
   1. `classList.add(类名)`添加一个类名
   2. `classList.remove(类名）`移除一个类名
   3. `classList.toggle(类名）`若该类名存在则移除，不存在则添加
   4. `classList.contains(类名)`判断类名是否存在