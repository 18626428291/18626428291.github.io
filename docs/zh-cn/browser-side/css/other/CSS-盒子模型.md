# CSS盒模型<!-- {docsify-ignore} -->

**文档更新日期: {docsify-updated}**

> [CSS教程:盒子模型](zh-cn/browser-side/css/README#CSS-盒子模型)

## 前言

盒子模型，英文即box model。无论是div、span、还是a都是盒子。

但是，图片、表单元素一律看作是文本，它们并不是盒子。这个很好理解，比如说，一张图片里并不能放东西，它自己就是自己的内容。

## 盒子中的区域

一个盒子中主要的属性就5个：width、height、padding、border、margin。如下：

- width和height：内容的宽度、高度（不是盒子的宽度、高度）。
- padding：内边距。
- border：边框。
- margin：外边距。

盒子模型的示意图：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/ekI0T5000202102261014429781.png)

代码演示：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/OPPw8o000202102261014458194.png)

上面这个盒子，width:200px; height:200px; 但是真实占有的宽高是302*302。 这是因为还要加上padding、border。

!> 注意：宽度和真实占有宽度，不是一个概念！来看下面这例子。

## 标准盒模型和IE盒模型

> 我们目前所学习的知识中，以标准盒子模型为准。

### 标准盒子模型

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/MSJb5r000202102261014455876.jpg)

### IE盒子模型

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/VyLEFN000202102261014464371.jpg)

上图显示：

在 CSS 盒子模型 (Box Model) 规定了元素处理元素的几种方式：

- width和height：内容的宽度、高度（不是盒子的宽度、高度）。
- padding：内边距。
- border：边框。
- margin：外边距。

CSS盒模型和IE盒模型的区别：

- 在 标准盒子模型中，width 和 height 指的是内容区域的宽度和高度。增加内边距、边框和外边距不会影响内容区域的尺寸，但是会增加元素框的总尺寸。
- IE盒子模型中，width 和 height 指的是内容区域+border+padding的宽度和高度。

?> 注：Android 中也有 margin 和 padding 的概念，意思是差不多的，如果你会一点 Android，应该比较好理解吧。区别在于，Android 中没有 border

## \<body\>标签也有margin

<body>标签有必要强调一下。很多人以为<body>标签占据的是整个页面的全部区域，其实是错误的，正确的理解是这样的：整个网页最大的盒子是<document>，即浏览器。

而<body>是<document>的儿子

浏览器给<body>默认的margin大小是**8个像素**，此时<body>占据了整个页面的一大部分区域，而不是全部区域。来看一段代码。

## 认识width、height

一定要知道，在前端开发工程师眼中，世界中的一切都是不同的。

比如说，丈量稿纸，前端开发工程师只会丈量内容宽度：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/RBQVr0000202102261014466765.png)

下面这两个盒子，真实占有宽高，都是302*302：

盒子1：

```
    .box1{
        width: 100px;
        height: 100px;
        padding: 100px;
        border: 1px solid red;
    }
```

盒子2：

```
    .box2{
        width: 250px;
        height: 250px;
        padding: 25px;
        border: 1px solid red;
    }
```

真实占有宽度 = 左border + 左padding + width + 右padding + 右border

上面这两个盒子的盒模型图如下：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/RhAcNP000202102261014467684.png)

如果想保持一个盒子的真实占有宽度不变，那么加width的时候就要减padding。加padding的时候就要减width。因为盒子变胖了是灾难性的，这会把别的盒子挤下去。

## 认识padding

!> padding区域也有颜色

padding就是内边距。padding的区域有背景颜色，css2.1前提下，并且背景颜色一定和内容区域的相同。也就是说，background-color将填充所有border以内的区域。

效果如下：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/QxWNYJ000202102261014476634.png)

!> padding有四个方向

padding是4个方向的，所以我们能够分别描述4个方向的padding。

方法有两种，第一种写小属性；第二种写综合属性，用空格隔开。

小属性的写法：

```css
    padding-top: 30px;
    padding-right: 20px;
    padding-bottom: 40px;
    padding-left: 100px;
```

?> 综合属性的写法：(上、右、下、左)（顺时针方向，用空格隔开。margin的道理也是一样的）

```
padding:30px 20px 40px 100px;
```

如果写了四个值，则顺序为：上、右、下、左。

如果只写了三个值，则顺序为：上、右、下。??和右一样。

如果只写了两个值，比如说：

```css
padding: 30px 40px;
```

则顺序等价于：30px 40px 30px 40px;

要懂得，**用小属性层叠大属性**。比如：

```css
padding: 20px;
padding-left: 30px;
```

上面的padding对应盒子模型为：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/XQ7jD0000202102261014473678.png)

下面的写法：

```
padding-left: 30px;
padding: 20px;
```

!> 第一行的小属性无效，因为被第二行的大属性层叠掉了。

### 一些元素，默认带有padding

一些元素，默认带有padding，比如ul标签。如下：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/2mw62M000202102261014481658.png)

上图显示，不加任何样式的ul，也是有40px的padding-left。

!> 所以，我们做站的时候，为了便于控制，总是喜欢清除这个默认的padding。

可以使用*进行清除：

```css
        *{
            margin: 0;
            padding: 0;
        }
```

但是，*的效率不高，所以我们使用并集选择器，罗列所有的标签（不用背，有专业的清除默认样式的样式表，今后学习）：

```css
body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td{
    margin:0;
    padding:0;
```

## 认识border

border就是边框。

?> 边框有三个要素：像素（粗细）、线型、颜色。

颜色如果不写，默认是黑色。另外两个属性不写，要命了，显示不出来边框。

### border-style

border的所有的线型如下：（我们可以通过查看[CSS 边框](zh-cn/browser-side/css/README#CSS-边框)得到）

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/OeHdKg000202102261014485936.png)

比如border:10px ridge red;这个属性，在chrome和firefox、IE中有细微差别：（因为可以显示出效果，因此并不是兼容性问题，只是有细微差别而已）

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/XpcJoL000202102261014485746.png)

如果公司里面的设计师是处女座的，追求极高的页面还原度，那么不能使用css来制作边框。就要用到图片，就要切图了。

所以，比较稳定的border-style就几个：solid、dashed、dotted

### border拆分

border是一个大综合属性。比如说：

```
border:1px solid red;
```

就是把4个边框，都设置为1px宽度、线型实线、red颜色。

border属性是能够被拆开的，有两大种拆开的方式：

- （1）按三要素拆开：border-width、border-style、border-color。（一个border属性是由三个小属性综合而成的）
- （2）按方向拆开：border-top、border-right、border-bottom、border-left。

现在我们明白了：一个border属性，是由三个小属性综合而成的。

如果某一个小属性后面是空格隔开的多个值，那么就是上右下左的顺序。举例如下：

```css
border-width:10px 20px;
border-style:solid dashed dotted;
border-color:red green blue yellow;
```

效果如下：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/J4MkPC000202102261014483640.png)

#### 按三要素拆

```css
border-width:10px;    //边框宽度
border-style:solid;   //线型
border-color:red;     //颜色。
```

等价于：

```css
border:10px solid red;
```

#### 按方向来拆

```css
border-top:10px solid red;
border-right:10px solid red;
border-bottom:10px solid red;
border-left:10px solid red;
```

等价于：

```css
border:10px solid red;
```

#### 按三要素和方向来拆

(就是把每个方向的，每个要素拆开。3*4 = 12)

```css
    border-top-width:10px;
    border-top-style:solid;
    border-top-color:red;
    border-right-width:10px;
    border-right-style:solid;
    border-right-color:red;
    border-bottom-width:10px;
    border-bottom-style:solid;
    border-bottom-color:red;
    border-left-width:10px;
    border-left-style:solid;
    border-left-color:red;
```

等价于：

```css
border:10px solid red;
```

工作中到底用什么？很简答：什么简单用什么。

!> 但要懂得，用小属性层叠大属性。

举例如下：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/rd2gRV000202102261014485257.png)

为了实现上方效果，写法如下：

```css
border:10px solid red;
border-right-color:blue;
```

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/WiTe0w000202102261014492342.png)

为了实现上方效果，写法如下：

```css
border:10px solid red;
border-style:solid dashed;
```

**border可以没有：**

```css
border:none;
```

也可以调整左边边框的宽度为0：

```css
border-left-width: 0;
```

#### 举例：利用border属性画一个三角形（小技巧）

步骤如下：

1. 当我们设置盒子的width和height为0时，此时效果如下：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/NWei4b000202102261014495562.png)

2. 然后将border的底部取消：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/W1YARA000202102261014497227.png)

3. 最后设置border的左边和右边为白色：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/jLmeGy000202102261014492087.png)

这样，一个三角形就画好了。
