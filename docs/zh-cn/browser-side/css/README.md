# CSS<!-- {docsify-ignore} -->

**文档更新日期: {docsify-updated}**

## 1.CSS 简介

### 1.1.什么是 CSS?

---

- CSS 指层叠样式表 ( **C**ascading **S**tyle **S**heets )
- 样式定义**如何显示** HTML 元素
- 样式通常存储在**样式表**中
- 把样式添加到 HTML 4.0 中，是为了**解决内容与表现分离的问题**
- **外部样式表**可以极大提高工作效率
- 外部样式表通常存储在 **CSS 文件**中
- 多个样式定义可**层叠**为一
- 样式对网页中元素位置的排版进行像素级精确控制

### 1.2.样式层叠

---

样式层叠就是对一个元素多次设置同一个样式，这将使用最后一次设置的属性值。

#### 样式层叠次序

当同一个 HTML 元素定义了多个样式时，应该使用哪个样式？

一般而言，所有的样式会根据下面的规则层叠于一个新的虚拟样式表中，其中数字 4 拥有最高的优先权。

1. 浏览器缺省设置
2. 外部样式表
3. 内部样式表（位于 `<head>` 标签内部）
4. 内联样式（在 HTML 元素内部）

因此，内联样式（在 HTML 元素内部）拥有最高的优先权，这意味着它将优先于以下的样式声明： 标签中的样式声明，外部样式表中的样式声明，或者浏览器中的样式声明（缺省值）。

### 1.3.语法

---

CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明:

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/5OSHLb000selector.gif)

选择器通常是您需要改变样式的 HTML 元素。

每条声明由一个属性和一个值组成。

属性（property）是您希望设置的样式属性（style attribute）。每个属性有一个值。属性和值被冒号分开。

#### 1.3.1.CSS 颜色值的写法

在描述颜色的时候，除了使用英文单词 red，我们还可以使用十六进制的颜色值 #ff0000：

```css
p {
  color: #ff0000;
}
或者 p {
  color: #f00;
}
```

还可以通过两种方法使用 RGB 值：

```css
p {
  color: rgb(255, 0, 0);
}
p {
  color: rgb(100%, 0%, 0%);
}
```

!> **提示：**当使用 RGB 百分比时，即使当值为 0 时也要写百分比符号。但是在其他的情况下就不需要这么做了。比如说，当尺寸为 0 像素时，0 之后不需要使用 px 单位。

### 1.4.CSS 注释

---

注释是用来解释你的代码，并且可以随意编辑它，浏览器会忽略它。

CSS 注释以 "`/*`" 开始, 以 "`*/`" 结束

## 2.CSS 选择器

参考[CSS3 选择器](zh-cn/browser-side/css/css3/css3-选择器)

## 3.CSS 创建

### 3.1.如何插入样式表

---

插入样式表的方法有三种:

- 外部样式表
- 内部样式表
- 内联样式

#### 3.1.1.外部样式表

标签在（文档的）头部

```html
<head>
  <link rel="stylesheet" type="text/css" href="mystyle.css" />
</head>
```

浏览器会从文件 mystyle.css 中读到样式声明，并根据它来格式文档

#### 3.1.2.内部样式表

当单个文档需要特殊的样式时，就应该使用内部样式表。你可以使用 `<style>`标签在文档头部定义内部样式表，就像这样:

```html
<head>
  <style>
    hr {
      color: sienna;
    }

    p {
      margin-left: 20px;
    }

    body {
      background-image: url('images/back40.gif');
    }
  </style>
</head>
```

3.1.3.内联样式

由于要将表现和内容混杂在一起，内联样式会损失掉样式表的许多优势。请慎用这种方法，例如当样式仅需要在一个元素上应用一次时。

要使用内联样式，你需要在相关的标签内使用样式（style）属性。Style 属性可以包含任何 CSS 属性。本例展示如何改变段落的颜色和左外边距：

```html
<p style="color:sienna;margin-left:20px">这是一个段落。</p>
```

### 3.2.多重样式

---

如果某些属性在不同的样式表中被同样的选择器定义，那么属性值将从更具体的样式表中被继承过来。

#### 3.2.1.多重样式将层叠为一个

样式表允许以多种方式规定样式信息。样式可以规定在单个的 HTML 元素中，在 HTML 页的头元素中，或在一个外部的 CSS 文件中。甚至可以在同一个 HTML 文档内部引用多个外部样式表。

##### 3.2.1.1.层叠次序

当同一个 HTML 元素被不止一个样式定义时，会使用哪个样式呢？

一般而言，所有的样式会根据下面的规则层叠于一个新的虚拟样式表中，其中数字 4 拥有最高的优先权。

1. 浏览器缺省设置
2. 外部样式表
3. 内部样式表（位于 head 标签内部）
4. 内联样式（在 HTML 元素内部）

因此，内联样式（在 HTML 元素内部）拥有最高的优先权，这意味着它将优先于以下的样式声明： 标签中的样式声明，外部样式表中的样式声明，或者浏览器中的样式声明（缺省值）。

##### 3.2.1.2.多重样式优先级顺序

下列是一份优先级逐级增加的选择器列表，其中数字 7 拥有最高的优先权：

1. 通用选择器（\*）
2. 元素(类型)选择器
3. 类选择器
4. 属性选择器
5. 伪类
6. ID 选择器
7. 内联样式

##### 3.2.1.3.!important 规则例外

当 !important 规则被应用在一个样式声明中时，该样式声明会覆盖 CSS 中任何其他的声明，无论它处在声明列表中的哪里。尽管如此，!important 规则还是与优先级毫无关系。使用 !important 不是一个好习惯，因为它改变了你样式表本来的级联规则，从而使其难以调试。

一些经验法则：

- Always 要优化考虑使用样式规则的优先级来解决问题而不是 !important
- Only 只在需要覆盖全站或外部 css（例如引用的 ExtJs 或者 YUI ）的特定页面中使用 !important
- Never 永远不要在全站范围的 CSS 上使用 !important
- Never 永远不要在你的插件中使用 !important

### 3.3.权重计算

---

![201712181559548677](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/FXIF3C0001513584051697589.png)

以下是对于上图的解释：

- 内联样式表的权值最高 1000
- ID 选择器的权值为 100
- Class 类选择器的权值为 10
- HTML 标签选择器的权值为 1

### 3.4.CSS 优先级法则

---

- A 选择器都有一个权值，权值越大越优先；
- B 当权值相等时，后出现的样式表设置要优于先出现的样式表设置；
- C 创作者的规则高于浏览者：即网页编写者设置的 CSS 样式的优先权高于浏览器所设置的样式；
- D 继承的 CSS 样式不如后来指定的 CSS 样式；
- E 在同一组属性设置中标有 "!important" 规则的优先级最大；

## 4.CSS 背景

CSS 背景属性用于定义 HTML 元素的背景。

CSS 属性定义背景效果：

- [`background`](zh-cn/browser-side/css/css3-属性#background)
- [`background-color`](zh-cn/browser-side/css/css3-属性#background-color)
- [`background-image`](zh-cn/browser-side/css/css3-属性#background-image)
- [`background-repeat`](zh-cn/browser-side/css/css3-属性#background-repeat)
- [`background-attachment`](zh-cn/browser-side/css/css3-属性#background-attachment)
- [`background-position`](zh-cn/browser-side/css/css3-属性#background-position)

### 4.1.背景颜色

---

`background-color` 属性定义了元素的背景颜色。

```css
body {
  background-color: #b0c4de;
}
```

?> **提示：**`background-color` 不能继承，其默认值是`transparent`。如果一个元素没有指定背景色，那么背景就是透明的，这样其父元素的背景才可见。

### 4.2.背景图像

#### 4.2.1.平铺

---

`background-image` 属性描述了元素的背景图像.

默认情况下，背景图像进行平铺重复显示，以覆盖整个元素实体。

#### 4.2.2. 水平或垂直平铺

---

如果需要在 HTML 页面上对背景图像进行平铺，可以使用 [`background-repeat`](zh-cn/browser-side/css/css3-属性#background-repeat) 属性。

默认情况下 `background-image` 属性会在页面的水平或者垂直方向平铺。

一些图像如果在水平方向与垂直方向平铺，这样看起来很不协调，如下所示:

```css
body {
  background-image: url('gradient2.png');
}
```

如果图像只在水平方向平铺`(repeat-x)`, 页面背景会更好些:

```css
body {
  background-image: url('gradient2.png');
  background-repeat: repeat-x;
}
```

#### 4.2.3.设置定位与不平铺

---

?> 让背景图像不影响文本的排版

如果你不想让图像平铺，你可以使用 `background-repeat` 属性:

```css
body {
  background-image: url('img_tree.png');
  background-repeat: no-repeat;
}
```

以上实例中，背景图像与文本显示在同一个位置，为了让页面排版更加合理，不影响文本的阅读，我们可以改变图像的位置。

可以利用 `background-position` 属性改变图像在背景中的位置:

```css
body {
  background-image: url('img_tree.png');
  background-repeat: no-repeat;
  background-position: right top;
}
```

?> **提示：**为 `background-position` 属性提供值有很多方法。首先，可以使用一些关键字：`top`、`bottom`、`left`、`right` 和 `center`；<br>其次，可以使用长度值，如 100px 或 5cm；<br>最后也可以使用百分数值。<br>不同类型的值对于背景图像的放置稍有差异。

##### 关键字

图像放置关键字最容易理解的作用就像其名字的意义。例如，`top` `left` 使图像放置在元素内边距区的左上角。

!> 只要保证不超过两个关键字：一个对应水平方向，另一个对应垂直方向，那么你可以设置位置关键字以任何顺序出现。

如果只有一个关键字，则会默认另一个关键字为 `center`。

下面是等价的位置关键字：

| 单一关键字 | 等价的关键字                   |
| :--------- | :----------------------------- |
| center     | center center                  |
| top        | top center 或 center top       |
| bottom     | bottom center 或 center bottom |
| right      | right center 或 center right   |
| left       | left center 或 center left     |

##### 百分数值

百分数值的表现方式更为复杂。假设你希望用百分数值将图像在其元素中居中，你可以按照下面的代码进行设置：

```css
body {
  background-image: url('img_tree.png');

  background-repeat: no-repeat;

  background-position: 50% 50%;
}
```

##### 长度值

长度值解释的是元素内边距区左上角的偏移，偏移点是图像的左上角。

比如，如果设置值为 50px 100px，图像的左上角将在元素内边距区左上角向右 50 像素、向下 100 像素的位置上：

```css
body {
  background-image: url('img_tree.png');

  background-repeat: no-repeat;

  background-position: 50px 100px;
}
```

!> 注意，这一点与百分数值不同，因为偏移只是从一个左上角到另一个左上角。也就是说，图像的左上角与 `background-position` 声明中的指定的点对齐。

### 4.3.背景- 简写属性

---

在以上实例中我们可以看到页面的背景颜色通过了很多的属性来控制。

为了简化这些属性的代码，我们可以将这些属性合并在同一个属性中.

背景颜色的简写属性为 "background":

```css
body {
  background: #ffffff url('img_tree.png') no-repeat right top;
}
```

当使用简写属性时，属性值的顺序为：:

- `background-color`
- `background-image`
- `background-repeat`
- `background-attachment`
- `background-position`

以上属性无需全部使用，你可以按照页面的实际需要使用.

## CSS 文本

**CSS Text 文本格式**

通过 CSS 的 Text 属性，你可以改变页面中文本的颜色、字符间距、对齐文本、装饰文本、对文本进行缩进等等，你可以观察下述的一段简单的应用了 CSS 文本格式的段落。

### 文本颜色

颜色是通过 CSS 最经常的指定：

- 十六进制值 - 如"＃FF0000"
- 一个 RGB 值 - "RGB（255,0,0）"
- 颜色的名称 - 如"红"

参阅 [CSS 颜色值](#CSS颜色名称) 查看完整的颜色值。

---

### 文本对齐

文本排列属性是用来设置文本的水平对齐方式。

文本可居中或对齐到左或右,两端对齐.

当 `text-align`设置为`justify`，每一行被展开为宽度相等，左，右外边距是对齐（如杂志和报纸）。

```css
h1 {
  text-align: center;
}
p.date {
  text-align: right;
}
p.main {
  text-align: justify;
}
```

?> **提示：**如果想把一个行内元素的第一行“缩进”，可以用左内边距或外边距创造这种效果。

---

### 文本修饰

`text-decoration`属性用来设置或删除文本的装饰。

从设计的角度看 `text-decoration` 属性主要是用来删除链接的下划线：

```css
a {
  text-decoration: none;
}
```

也可以这样装饰文字：

```css
h1 {
  text-decoration: overline;
}
h2 {
  text-decoration: line-through;
}
h3 {
  text-decoration: underline;
}
```

!> 不建议强调指出不是链接的文本，因为这常常混淆用户。

---

### 文本转换

文本转换属性是用来指定在一个文本中的大写和小写字母。

可用于所有字句变成大写或小写字母，或每个单词的首字母大写。

```css
p.uppercase {
  text-transform: uppercase;
}
p.lowercase {
  text-transform: lowercase;
}
p.capitalize {
  text-transform: capitalize;
}
```

---

### 文本缩进

文本缩进属性是用来指定文本的第一行的缩进。

CSS 提供了 `text-indent` 属性，该属性可以方便地实现文本缩进。

通过使用 `text-indent` 属性，所有元素的第一行都可以缩进一个给定的长度。

```css
p {
  text-indent: 50px;
}
```

---

### 文本间隔

`word-spacing` 属性可以改变字（单词）之间的标准间隔。其默认值 `normal`与设置值为 0 是一样的。

指定段字之间的空间，应该是 30 像素：

```css
p {
  word-spacing: 30px;
}
```

---

### 所有 CSS 文本属性

| 属性                                                                | 描述                     |
| :------------------------------------------------------------------ | :----------------------- |
| [color](zh-cn/browser-side/css/css3-属性#color)                     | 设置文本颜色             |
| [direction](zh-cn/browser-side/css/css3-属性#direction)             | 设置文本方向。           |
| [letter-spacing](zh-cn/browser-side/css/css3-属性#letter-spacing)   | 设置字符间距             |
| [line-height](zh-cn/browser-side/css/css3-属性#line-height)         | 设置行高                 |
| [text-align](zh-cn/browser-side/css/css3-属性#text-align)           | 对齐元素中的文本         |
| [text-decoration](zh-cn/browser-side/css/css3-属性#text-decoration) | 向文本添加修饰           |
| [text-indent](zh-cn/browser-side/css/css3-属性#text-indent)         | 缩进元素中文本的首行     |
| [text-shadow](zh-cn/browser-side/css/css3-属性#text-shadow)         | 设置文本阴影             |
| [text-transform](zh-cn/browser-side/css/css3-属性#text-transform)   | 控制元素中的字母         |
| [unicode-bidi](zh-cn/browser-side/css/css3-属性#unicode-bidi)       | 设置或返回文本是否被重写 |
| [vertical-align](zh-cn/browser-side/css/css3-属性#vertical-align)   | 设置元素的垂直对齐       |
| [white-space](zh-cn/browser-side/css/css3-属性#white-space)         | 设置元素中空白的处理方式 |
| [word-spacing](zh-cn/browser-side/css/css3-属性#word-spacing)       | 设置字间距               |

## CSS 字体

## CSS 链接

### 链接样式

不同的链接可以有不同的样式。

链接的样式，可以用任何 CSS 属性（如颜色，字体，背景等）。

特别的链接，可以有不同的样式，这取决于他们是什么状态。

这四个链接状态是：

- `a:link` - 正常，未访问过的链接
- `a:visited` - 用户已访问过的链接
- `a:hover` - 当用户鼠标放在链接上时
- `a:active` - 链接被点击的那一刻

```css
a:link {
  color: #ff0000;
} /* 未访问链接*/
a:visited {
  color: #00ff00;
} /* visited link */
a:hover {
  color: #ff00ff;
} /* mouse over link */
a:active {
  color: #0000ff;
} /* selected link */
```

当设置为若干链路状态的样式，也有一些顺序规则：

- `a:hover` 必须跟在`a:link` 和 `a:visited` 后面
- `a:active` 必须跟在 `a:hover` 后面

> 参考:[link 选择器](zh-cn/browser-side/css/css3/css3-选择器#link选择器),[hover 选择器](zh-cn/browser-side/css/css3/css3-选择器#hover选择器),[visited 选择器](zh-cn/browser-side/css/css3/css3-选择器#visited选择器),[active 选择器](zh-cn/browser-side/css/css3/css3-选择器#active选择器)

---

### 常见的链接样式

#### 文本修饰

`text-decoration` 属性主要用于删除链接中的下划线：

```css
a:link {
  text-decoration: none;
}
a:visited {
  text-decoration: none;
}
a:hover {
  text-decoration: underline;
}
a:active {
  text-decoration: underline;
}
```

#### 背景颜色

背景颜色属性指定链接背景色：

```css
a:link {
  background-color: #b2ff99;
}
a:visited {
  background-color: #ffff85;
}
a:hover {
  background-color: #ff704d;
}
a:active {
  background-color: #ff704d;
}
```

#### 鼠标形状

常用鼠标形状如下所示：

| 属性值    | 描述               |
| :-------- | :----------------- |
| default   | 默认光标，箭头     |
| pointer   | 超链接的指针，手型 |
| wait      | 指示程序正在忙     |
| help      | 指示可用的帮忙     |
| text      | 指示文本           |
| crosshair | 鼠标呈现十字状     |

```css
a:hover {
  color: green;

  cursor: crosshair;
}
```

---

### 实例

[添加不同样式的超链接](https://www.w3cschool.cn/tryrun/showhtml/trycss_link2)

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>W3Cschool教程(w3cschool.cn)</title>
    <style>
      a.one:link {
        color: #ff0000;
      }
      a.one:visited {
        color: #0000ff;
      }
      a.one:hover {
        color: #ffcc00;
      }

      a.two:link {
        color: #ff0000;
      }
      a.two:visited {
        color: #0000ff;
      }
      a.two:hover {
        font-size: 150%;
      }

      a.three:link {
        color: #ff0000;
      }
      a.three:visited {
        color: #0000ff;
      }
      a.three:hover {
        background: #66ff66;
      }

      a.four:link {
        color: #ff0000;
      }
      a.four:visited {
        color: #0000ff;
      }
      a.four:hover {
        font-family: monospace;
      }

      a.five:link {
        color: #ff0000;
        text-decoration: none;
      }
      a.five:visited {
        color: #0000ff;
        text-decoration: none;
      }
      a.five:hover {
        text-decoration: underline;
      }
    </style>
  </head>

  <body>
    <p>将鼠标移至链接上改变样式.</p>

    <p>
      <b
        ><a class="one" href="/css/" target="_blank"
          >这个链接会改变字体颜色</a
        ></b
      >
    </p>
    <p>
      <b
        ><a class="two" href="/css/" target="_blank"
          >这个链接会改变字体大小</a
        ></b
      >
    </p>
    <p>
      <b
        ><a class="three" href="/css/" target="_blank"
          >这个链接会改变背景颜色</a
        ></b
      >
    </p>
    <p>
      <b
        ><a class="four" href="/css/" target="_blank"
          >这个链接会改变字体样式</a
        ></b
      >
    </p>
    <p>
      <b
        ><a class="five" href="/css/" target="_blank"
          >这个链接会改变下划线</a
        ></b
      >
    </p>
  </body>
</html>
```

[高级 - 创建链接框](https://www.w3cschool.cn/tryrun/showhtml/trycss_link_advanced)

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>W3Cschool教程(w3cschool.cn)</title>
    <style>
      a:link,
      a:visited {
        display: block;
        font-weight: bold;
        color: #ffffff;
        background-color: #98bf21;
        width: 120px;
        text-align: center;
        padding: 4px;
        text-decoration: none;
      }
      a:hover,
      a:active {
        background-color: #7a991a;
      }
    </style>
  </head>

  <body>
    <a href="/css/" target="_blank">This is a link</a>
  </body>
</html>
```

---

## CSS 列表

在 HTML 中，有两种类型的列表:

- 无序列表 - 列表项的标记使用特殊图形（如小黑点、小方框等）
- 有序列表 - 列表项的标记使用数字或字母

使用 CSS，可以列出进一步的样式，并可用图像作列表项标记。

---

### 不同的列表项标记

`list-style-type` 属性指定列表项标记的类型是：

```css
ul.a {
  list-style-type: circle;
}
ul.b {
  list-style-type: square;
}

ol.c {
  list-style-type: upper-roman;
}
ol.d {
  list-style-type: lower-alpha;
}
```

一些值是无序列表，以及有些是有序列表。

下列是对 `list-style-type` 属性的常见属性值的描述：

- `none`：不使用项目符号
- `disc`：实心圆
- `circle`：空心圆
- `square`：实心方块
- `decimal`：阿拉伯数字
- `lower-alpha`：小写英文字母
- `upper-alpha`：大写英文字母
- `lower-roman`：小写罗马数字
- `upper-roman`：大写罗马数字

---

### 作为列表项标记的图像

要指定列表项标记的图像，使用列表样式图像属性：

```css
ul {
  list-style-image: url('sqpurple.gif');
}
```

上面的例子在各大主流浏览器中的显示有所差异，IE 和 Opera 显示图像标记比火狐（ Firefox ），Chrome 和 Safari 更高一点点。

> 提示：
>
> - 利用 `list-style-position` 可以确定标志出现在列表项内容之外还是内容内部。
> - 如果你想在所有的浏览器放置同样的形象标志，就应使用浏览器兼容性解决方案，方法如下。\*\*

---

### 浏览器兼容性解决方案

同样在所有的浏览器，下面的例子会显示的图像标记：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>W3Cschool教程(w3cschool.cn)</title>
    <style>
      ul {
        list-style-type: none;
        padding: 0px;
        margin: 0px;
      }
      ul li {
        background-image: url('/statics/images/w3c/sqpurple.gif');
        background-repeat: no-repeat;
        background-position: 0px 5px;
        padding-left: 14px;
      }
    </style>
  </head>

  <body>
    <ul>
      <li>咖啡</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ul>
  </body>
</html>
```

例子解释：

- ul :
  - 设置列表样式类型为没有列表项标记
  - 设置填充和边距 0px（浏览器兼容性）
- ul 中所有 li :
  - 设置图像的 URL ，并设置它只显示一次（无重复）
  - 您需要的定位图像位置（左 0px 和上下 5px ）
  - 用 `padding-left` 属性把文本置于列表中

---

### 列表 -简写属性

在单个属性中可以指定所有的列表属性。这就是所谓的简写属性。

为列表使用简写属性，列表样式属性设置如下：

```css
ul {
  list-style: square url('sqpurple.gif');
}
```

如果使用缩写属性值的顺序是：

1. `list-style-type`
2. `list-style-position` (有关说明，请参见下面的 CSS 属性表)
3. `list-style-image`

在简写属性时，如果上述值丢失一个，其余仍在指定的顺序，就没关系。

---

### 实例

所有不同的列表标记

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>W3Cschool教程(w3cschool.cn)</title>
    <style>
      ul.a {
        list-style-type: circle;
      }
      ul.b {
        list-style-type: disc;
      }
      ul.c {
        list-style-type: square;
      }

      ol.d {
        list-style-type: armenian;
      }
      ol.e {
        list-style-type: cjk-ideographic;
      }
      ol.f {
        list-style-type: decimal;
      }
      ol.g {
        list-style-type: decimal-leading-zero;
      }
      ol.h {
        list-style-type: georgian;
      }
      ol.i {
        list-style-type: hebrew;
      }
      ol.j {
        list-style-type: hiragana;
      }
      ol.k {
        list-style-type: hiragana-iroha;
      }
      ol.l {
        list-style-type: katakana;
      }
      ol.m {
        list-style-type: katagana-iroha;
      }
      ol.n {
        list-style-type: lower-alpha;
      }
      ol.o {
        list-style-type: lower-greek;
      }
      ol.p {
        list-style-type: lower-latin;
      }
      ol.q {
        list-style-type: lower-roman;
      }
      ol.r {
        list-style-type: upper-alpha;
      }
      ol.s {
        list-style-type: upper-latin;
      }
      ol.t {
        list-style-type: upper-roman;
      }

      ol.u {
        list-style-type: none;
      }
      ol.v {
        list-style-type: inherit;
      }
    </style>
  </head>

  <body>
    <ul class="a">
      <li>Circle 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ul>

    <ul class="b">
      <li>Disc 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ul>

    <ul class="c">
      <li>Square 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ul>

    <ol class="d">
      <li>Armenian 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="e">
      <li>Cjk-ideographic 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="f">
      <li>Decimal 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="g">
      <li>Decimal-leading-zero 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="h">
      <li>Georgian 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="i">
      <li>Hebrew 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="j">
      <li>Hiragana 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="k">
      <li>Hiragana-iroha 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="l">
      <li>Katakana 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="m">
      <li>Katakana-iroha 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="n">
      <li>Lower-alpha 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="o">
      <li>Lower-greek 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="p">
      <li>Lower-latin 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="q">
      <li>Lower-roman 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="r">
      <li>Upper-alpha 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="s">
      <li>Upper-latin 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="t">
      <li>Upper-roman 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="u">
      <li>None 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>

    <ol class="v">
      <li>inherit 样式</li>
      <li>茶</li>
      <li>可口可乐</li>
    </ol>
  </body>
</html>
```

---

### 所有属性

| 属性                                                                        | 描述                                               |
| :-------------------------------------------------------------------------- | :------------------------------------------------- |
| [list-style](zh-cn/browser-side/css/css3-属性#list-style)                   | 简写属性。用于把所有用于列表的属性设置于一个声明中 |
| [list-style-image](zh-cn/browser-side/css/css3-属性#list-style-image)       | 将图象设置为列表项标志。                           |
| [list-style-position](zh-cn/browser-side/css/css3-属性#list-style-position) | 设置列表中列表项标志的位置。                       |
| [list-style-type](zh-cn/browser-side/css/css3-属性#list-style-type)         | 设置列表项标志的类型。                             |

---

## CSS 表格

### 表格边框

指定 CSS 表格边框，使用 `border` 属性。

下面的例子指定了一个表格的 `th` 和 `td` 元素的黑色边框：

```css
table, th, td
{ border: 1px solid black; }
```

!> 请注意，在上面的例子中的表格有双边框。这是因为表和 `th / td` 元素有独立的边界。<br>为了显示一个表的单个边框，使用 `border-collapse`属性。

---

### 折叠边框

`border-collapse` 属性设置表格的边框是否被折叠成一个单一的边框或隔开：

```css
table
{ border-collapse:collapse; }
table,th, td { border: 1px solid black; }
```

---

### 表格宽度和高度

`width` 和`height` 属性定义表格的宽度和高度。

下面的例子是设置 100％ 的宽度，50 像素的 th 元素的高度的表格：

```css
table
{ width:100%; }
th { height:50px; }
```

---

### 表格文字对齐

表格中的文本对齐和垂直对齐属性。

text-align 属性设置水平对齐方式，像左，右，或中心：

```css
td
{ text-align:right; }
```

垂直对齐属性设置垂直对齐，比如顶部，底部或中间：

```css
td
{ height:50px; vertical-align:bottom; }
```

---

### 表格填充

如果在表的内容中控制空格之间的边框，应使用 td 和 th 元素的填充属性：

```css
td
{ padding:15px; }
```

---

#### 表格颜色

下面的例子指定边框的颜色，和 th 元素的文本和背景颜色：

```css
table, td, th
{ border:1px solid green; }
th
{ background-color:green; color:white; }
```

---

### 更多实例

[制作一个个性表格](https://www.w3cschool.cn/tryrun/showhtml/trycss_table_fancy)
这个例子演示了如何创建一个个性的表格。

[设置表格标题的位置](https://www.w3cschool.cn/tryrun/showhtml/trycss_table_caption-side)
这个例子演示了如何定位表格标题。

[指定表格的宽度和高度](https://www.w3cschool.cn/tryrun/showhtml/trycss_table_width)

这个例子演示了如何指定表格的高度与宽度。

---

## CSS 盒子模型

**CSS Box Model (盒子模型)**

**所有 HTML 元素可以看作盒子，在 CSS 中，"box model "这一术语是用来设计和布局时使用。**

CSS 盒模型本质上是一个盒子，封装周围的 HTML 元素，它包括：边距，边框，填充，和实际内容。

盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。

下面的图片说明了盒子模型 (Box Model)：

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/WsaSki000box-model.gif)

不同部分的说明：

- **[Margin(外边距)](#css-外边距)** - 清除边框区域。Margin 没有背景颜色，它是完全透明
- **[Border（边框）](#css-边框)** - 边框周围的填充和内容。边框是受到盒子的背景颜色影响
- **[Padding（内边距）](css-内边距)** - 清除内容周围的区域。会受到框中填充的背景颜色影响
- **Content（内容）** - 盒子的内容，显示文本和图像

?> **提示：**在盒模型中，外边距可以是负值，而且在很多情况下都要使用负值的外边距。

> 扩展: [CSS盒子模型](zh-cn/browser-side/css/other/CSS-盒子模型.md)

---

## CSS 边框

CSS 边框 (border) 可以是围绕元素内容和内边距的一条或多条线，对于这些线条，您可以自定义它们的样式、宽度以及颜色。使用 CSS 边框属性，我们可以创建出比 HTML 中更加优秀的效果。

<div style="width:93%;border:10px groove #98bf21;padding:8px">
     <h2>CSS 边框属性</h2>
    <p>CSS边框属性允许你指定一个元素边框的样式和颜色。</p>
</div>

---

### 边框样式

边框样式属性指定要显示什么样的边界。

**border-style**属性用来定义边框的样式

#### border-style 值

<p style="border: 1px none #000000;padding:3px">none: 默认无边框</p>

<p style="border: 1px dotted #000000;padding:3px">dotted: 定义一个点线框</p>

<p style="border: 1px dashed #000000;padding:3px">dashed: 定义一个虚线框</p>

<p style="border: 1px solid #000000;padding:3px">solid: 定义实线边界</p>

<p style="border: 3px double #000000;padding:3px">double: 定义两个边界。 两个边界的宽度和border-width的值相同</p>

<p style="border: 5px groove #98bf21;padding:3px">groove: 定义3D沟槽边界。效果取决于边界的颜色值</p>

<p style="border: 5px ridge #98bf21;padding:3px">ridge: 定义3D脊边界。效果取决于边界的颜色值</p>

<p style="border: 5px inset #98bf21;padding:3px">inset:定义一个3D的嵌入边框。效果取决于边界的颜色值</p>

<p style="border: 5px outset #98bf21;padding:3px">outset: 定义一个3D突出边框。 效果取决于边界的颜色值</p>

```css
<style>
    p.none {border-style:none;}
    p.dotted {border-style:dotted;}
    p.dashed {border-style:dashed;}
    p.solid {border-style:solid;}
    p.double {border-style:double;}
    p.groove {border-style:groove;}
    p.ridge {border-style:ridge;}
    p.inset {border-style:inset;}
    p.outset {border-style:outset;}
    p.hidden {border-style:hidden;}
</style>
```

---

### 边框宽度

您可以通过 border-width 属性为边框指定宽度。

为边框指定宽度有两种方法：可以指定长度值，比如 2px 或 0.1em；或者使用 3 个关键字之一，它们分别是 thin 、medium（默认值） 和 thick。

!> **注意：**CSS 没有定义 3 个关键字的具体宽度，所以一个用户代理可能把 thin 、medium 和 thick 分别设置为等于 5px、3px 和 2px，而另一个用户代理则分别设置为 3px、2px 和 1px。

```css
p.one
{
border-style:solid;
border-width:5px;
}
p.two
{
border-style:solid;
border-width:medium;
}
```

!> **注意:** "border-width" 属性 如果单独使用则不起作用. 要先使用 "border-style" 属性来设置 borders .

---

### 边框颜色

border-color 属性用于设置边框的颜色，它一次可以接受最多 4 个颜色值。可以设置的颜色：

- name - 指定颜色的名称，如 "red"
- RGB - 指定 RGB 值, 如 "rgb(255,0,0)"
- Hex - 指定16进制值, 如 "#ff0000"

您还可以设置边框的颜色为"transparent"。

!> **注意：** border-color 单独使用是不起作用的，必须得先使用 border-style 来设置边框样式。

```css
p.one
{
border-style:solid;
border-color:red;
}
p.two
{
border-style:solid;
border-color:#98bf21;
}
```

---

### 边框-单独设置各边

在 CSS 中，可以指定不同的侧面不同的边框：

```css
p
{
border-top-style:dotted;
border-right-style:solid;
border-bottom-style:dotted;
border-left-style:solid;
}
```

等价于

```css
border-style:dotted solid;
```

border-style 属性可以有 1-4 个值：

- border-style:dotted solid double dashed;
  - 上边框是 dotted
  - 右边框是 solid
  - 底边框是 double
  - 左边框是 dashed
- border-style:dotted solid double;
  - 上边框是 dotted
  - 左、右边框是 solid
  - 底边框是 double
- border-style:dotted solid;
  - 上、底边框是 dotted
  - 左、右边框是 solid
- border-style:dotted;
  - 四面边框是 dotted

上面的例子用了 border-style。然而，它也可以和 border-width 、 border-color 一起使用。

---

### 透明边框

CSS2 引入了边框颜色值 transparent，这个值用于创建有宽度的不可见边框。

透明样式的定义如下：

```css
a:link, a:visited {

border-style: solid; border-width: 5px; border-color: transparent;

} a:hover {border-color: gray;}
```

利用 transparent，使用边框就像是额外的内边距一样；此外还有一个好处，就是能在你需要的时候使其可见。这种透明边框相当于内边距，因为元素的背景会延伸到边框区域（如果有可见背景的话）。

---

### 边框-简写属性

上面的例子用了很多属性来设置边框。

你也可以在一个属性中设置边框。

你可以在"border"属性中设置：

- border-width
- border-style (required)
- border-color

```css
border:5px solid red;
```

---

### 更多实例

[所有边框属性在一个声明之中](https://www.w3cschool.cn/tryrun/showhtml/trycss_border-top)
本例演示用简写属性来将所有四个边框属性设置于同一声明中。

[设置下边框的样式](https://www.w3cschool.cn/tryrun/showhtml/trycss_border-bottom-style)
本例演示如何设置下边框的样式。

[设置左边框的宽度](https://www.w3cschool.cn/tryrun/showhtml/trycss_border-left-width)
本例演示如何设置左边框的宽度。

[设置四个边框的颜色](https://www.w3cschool.cn/tryrun/showhtml/trycss_border-color)
本例演示如何设置四个边框的颜色。可以设置一到四个颜色。

[设置右边框的颜色](https://www.w3cschool.cn/tryrun/showhtml/trycss_border-right-color)
本例演示如何设置右边框的颜色。

---

### CSS 边框属性

| 属性                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [border](zh-cn/browser-side/css/css3-属性#border)     | 简写属性，用于把针对四个边的属性设置在一个声明。             |
| [border-style](zh-cn/browser-side/css/css3-属性#border-style) | 用于设置元素所有边框的样式，或者单独地为各边设置边框样式。   |
| [border-width](zh-cn/browser-side/css/css3-属性#border-width) | 简写属性，用于为元素的所有边框设置宽度，或者单独地为各边边框设置宽度。 |
| [border-color](zh-cn/browser-side/css/css3-属性#border-color) | 简写属性，设置元素的所有边框中可见部分的颜色，或为 4 个边分别设置颜色。 |
| [border-bottom](zh-cn/browser-side/css/css3-属性#border-bottom) | 简写属性，用于把下边框的所有属性设置到一个声明中。           |
| [border-bottom-color](zh-cn/browser-side/css/css3-属性#border-bottom-color) | 设置元素的下边框的颜色。                                     |
| [border-bottom-style](zh-cn/browser-side/css/css3-属性#border-bottom-style) | 设置元素的下边框的样式。                                     |
| [border-bottom-width](zh-cn/browser-side/css/css3-属性#border-bottom-width) | 设置元素的下边框的宽度。                                     |
| [border-left](zh-cn/browser-side/css/css3-属性#border-left) | 简写属性，用于把左边框的所有属性设置到一个声明中。           |
| [border-left-color](zh-cn/browser-side/css/css3-属性#border-left-color) | 设置元素的左边框的颜色。                                     |
| [border-left-style](zh-cn/browser-side/css/css3-属性#border-left-style) | 设置元素的左边框的样式。                                     |
| [border-left-width](zh-cn/browser-side/css/css3-属性#border-left-width) | 设置元素的左边框的宽度。                                     |
| [border-right](zh-cn/browser-side/css/css3-属性#border-right) | 简写属性，用于把右边框的所有属性设置到一个声明中。           |
| [border-right-color](zh-cn/browser-side/css/css3-属性#border-right-color) | 设置元素的右边框的颜色。                                     |
| [border-right-style](zh-cn/browser-side/css/css3-属性#border-right-style) | 设置元素的右边框的样式。                                     |
| [border-right-width](zh-cn/browser-side/css/css3-属性#border-right-width) | 设置元素的右边框的宽度。                                     |
| [border-top](zh-cn/browser-side/css/css3-属性#border-top) | 简写属性，用于把上边框的所有属性设置到一个声明中。           |
| [border-top-color](zh-cn/browser-side/css/css3-属性#border-top-color) | 设置元素的上边框的颜色。                                     |
| [border-top-style](zh-cn/browser-side/css/css3-属性#border-top-style) | 设置元素的上边框的样式。                                     |
| [border-top-width](zh-cn/browser-side/css/css3-属性#border-top-width) | 设置元素的上边框的宽度。                                     |

---

## CSS 轮廓

### **Outlines**

轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

轮廓（outline）属性指定了样式，颜色和外边框的宽度。

轮廓（outline）属性的位置让它不像边框那样参与到文档流中，因此轮廓出现或消失时不会影响文档流，即不会导致文档的重新显示。

---

### 轮廓（outline）实例

[在元素周围画线](https://www.w3cschool.cn/tryrun/showhtml/trycss_outline)
本例演示使用outline属性在元素周围画一条线。.

[设置轮廓的样式](https://www.w3cschool.cn/tryrun/showhtml/trycss_outline-style)
本例演示如何设置轮廓的样式。

[设置轮廓的颜色](https://www.w3cschool.cn/tryrun/showhtml/trycss_outline-color)
本例演示如何设置轮廓的颜色。

[设置轮廓的宽度](https://www.w3cschool.cn/tryrun/showhtml/trycss_outline-width)
本例演示如何设置轮廓的宽度。

---

### CSS 轮廓（outline）

轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

CSS outline 属性规定元素轮廓的样式、颜色和宽度。

![Outline](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/jKGZPf000box_outline.gif)

---

### 所有 CSS 轮廓（outline）属性

"CSS" 列中的数字表示哪个 CSS 版本定义了该属性 ( CSS1 或者 CSS2 )。

| 属性                                                         | 说明                             | 值                                                           | CSS  |
| :----------------------------------------------------------- | :------------------------------- | :----------------------------------------------------------- | :--- |
| [outline](zh-cn/browser-side/css/css3-属性#outline) | 在一个声明中设置所有的外边框属性 | *outline-color outline-style outline-width*inherit          | 2    |
| [outline-offset](zh-cn/browser-side/css/css3-属性#outline-offset) | outline-offset属性设置轮廓框架在 border 边缘外的偏移 | *length* inherit          | 2    |
| [outline-color](zh-cn/browser-side/css/css3-属性#outline-color) | 设置外边框的颜色                 | *color-name hex-number rgb-number*invert inherit            | 2    |
| [outline-style](zh-cn/browser-side/css/css3-属性#outline-style) | 设置外边框的样式                 | none dotted dashed solid double groove ridge inset outset inherit | 2    |
| [outline-width](zh-cn/browser-side/css/css3-属性#outline-width) | 设置外边框的宽度                 | thin medium thick *length*inherit                           | 2    |

---

## CSS 外边距

### CSS Margin(外边距)

CSS Margin (外边距)属性定义元素周围的空间。

CSS Margin 属性接受任何长度单位、百分数值甚至负值。

---

### Margin

margin 清除周围的元素（外边框）的区域。margin 没有背景颜色，是完全透明的

margin 可以单独改变元素的上，下，左，右边距。也可以一次改变所有的属性。

### 可能的值

| 值       | 说明                                        |
| :------- | :------------------------------------------ |
| auto     | 设置浏览器边距。 这样做的结果会依赖于浏览器 |
| *length* | 定义一个固定的margin（使用像素，pt，em等）  |
| *%*      | 定义一个使用百分比的边距                    |

---

### 所有的CSS填充属性

| 属性                                                         | 说明                                       |
| :----------------------------------------------------------- | :----------------------------------------- |
| [margin](zh-cn/browser-side/css/css3-属性#margin)          | 设置所有外边距属性 |
| [margin-bottom](zh-cn/browser-side/css/css3-属性#margin-bottom) | 设置元素的下边距                         |
| [margin-left](zh-cn/browser-side/css/css3-属性#margin-left) | 设置元素的左边距                         |
| [margin-right](zh-cn/browser-side/css/css3-属性#margin-right) | 设置元素的右边距                         |
| [margin-top](zh-cn/browser-side/css/css3-属性#margin-top) | 设置元素的上部边距                         |

---

## CSS 内边距

### CSS Padding（填充/内边距）

CSS Padding（填充）属性定义元素边框与元素内容之间的空间。

------

### Padding（填充/内边距）

当元素的 Padding（填充）（内边距）被清除时，所"释放"的区域将会受到元素背景颜色的填充。

单独使用填充属性可以改变上下左右的填充。缩写填充属性也可以使用，一旦改变一切都改变。

### 可能的值

| 值     | 说明                                |
| :----- | :---------------------------------- |
| length | 定义一个固定的填充(像素, pt, em,等) |
| %      | 使用百分比值定义一个填充            |

!> **提示：**CSS padding 属性可以使用长度值或百分比值，但与 margin 属性不同，它不允许使用负值。

---

### 内边距的百分比数值

CSS padding 属性的百分比数值是相对于其父元素的 width 计算的，如果改变了父元素的 width，则它们也会改变。

以下是将段落的内边距设置为父元素 width 的 20% 的示例：

```css
p {padding: 20%;}
```

假设一个段落的父元素是 div 元素，那么它的 padding 的 width 计算是根据 div 进行的：

!> **注意：**上下内边距与左右内边距一致，即上下内边距的百分数会相对于父元素宽度设置，而不是相对于高度。

---

### 填充- 单边内边距属性

在CSS中，它可以指定不同的侧面不同的填充：

```css
p.padding
{
 padding-top:25px;
 padding-bottom:25px;
 padding-right:50px;
 padding-left:50px;
}
```

---

### 填充 - 简写属性

为了缩短代码，它可以在一个属性中指定的所有填充属性。

这就是所谓的缩写属性。所有的填充属性的缩写属性是"padding":

```css
padding:25px 50px;
```

Padding 属性，可以有一到四个值。

**padding:25px 50px 75px 100px;**

- 上填充为25px
- 右填充为50px
- 下填充为75px
- 左填充为100px

**padding:25px 50px 75px;**

- 上填充为25px
- 左右填充为50px
- 下填充为75px

**padding:25px 50px;**

- 上下填充为25px
- 左右填充为50px

**padding:25px;**

- 所有的填充都是25px

---

### 更多实例

[在一个声明中的所有填充属性](https://www.w3cschool.cn/tryrun/showhtml/trycss_padding)
这个例子演示了使用缩写属性设置在一个声明中的所有填充属性，可以有一到四个值。

[设置左部填充](https://www.w3cschool.cn/tryrun/showhtml/trycss_padding-left)
这个例子演示了如何设置元素左填充。

[设置右部填充](https://www.w3cschool.cn/tryrun/showhtml/trycss_padding-right)
这个例子演示了如何设置元素右填充。.

[设置上部填充](https://www.w3cschool.cn/tryrun/showhtml/trycss_padding-top)
这个例子演示了如何设置元素上填充。

[设置下部填充](https://www.w3cschool.cn/tryrun/showhtml/trycss_padding-bottom)
这个例子演示了如何设置元素下填充。

---

### 所有的CSS填充属性

| 属性                                                         | 说明                                       |
| :----------------------------------------------------------- | :----------------------------------------- |
| [padding](zh-cn/browser-side/css/css3-属性#padding)          | 使用缩写属性设置在一个声明中的所有填充属性 |
| [padding-bottom](zh-cn/browser-side/css/css3-属性#padding-bottom) | 设置元素的底部填充                         |
| [padding-left](zh-cn/browser-side/css/css3-属性#padding-left) | 设置元素的左部填充                         |
| [padding-right](zh-cn/browser-side/css/css3-属性#padding-right) | 设置元素的右部填充                         |
| [padding-top](zh-cn/browser-side/css/css3-属性#padding-top) | 设置元素的顶部填充                         |

---

## CSS 尺寸

CSS 尺寸 (Dimension) 属性允许你控制元素的高度和宽度。同样，它允许你增加行间距。

### 更多实例

[设置元素的高度](https://www.w3cschool.cn/tryrun/showhtml/trycss_dim_height)

这个例子演示了如何设置不同元素的高度。

[使用百分比设置图像的高度](https://www.w3cschool.cn/tryrun/showhtml/trycss_dim_height_percent)

这个例子演示了如何使用百分比值设置元素的高度。

[使用像素值来设置元素的宽度](https://www.w3cschool.cn/tryrun/showhtml/trycss_dim_width)

本例演示如何使用像素值来设置元素的宽度。

[设置元素的最大高度](https://www.w3cschool.cn/tryrun/showhtml/trycss_dim_max_height)

此示例演示如何设置元素的最大高度。

[使用百分比来设置元素的最大宽度](https://www.w3cschool.cn/tryrun/showhtml/trycss_dim_max-width_percent)

本例演示如何使用百分比值来设置元素的最大宽度。

[设置元素的最低高度](https://www.w3cschool.cn/tryrun/showhtml/trycss_dim_min-height)

此示例演示如何设置元素的最小高度。

[使用像素值设置元素的最小宽度](https://www.w3cschool.cn/tryrun/showhtml/trycss_dim_min-width)

这个例子演示了如何使用像素值设置元素的最小宽度。

---

### 所有CSS 尺寸 (Dimension)属性

| 属性                                                         | 描述                 |
| :----------------------------------------------------------- | :------------------- |
| [height](zh-cn/browser-side/css/css3-属性#height) | 设置元素的高度。     |
| [line-height](zh-cn/browser-side/css/css3-属性#line-height) | 设置行高。           |
| [max-height](zh-cn/browser-side/css/css3-属性#max-height) | 设置元素的最大高度。 |
| [max-width](zh-cn/browser-side/css/css3-属性#max-width) | 设置元素的最大宽度。 |
| [min-height](zh-cn/browser-side/css/css3-属性#min-height) | 设置元素的最小高度。 |
| [min-width](zh-cn/browser-side/css/css3-属性#min-width) | 设置元素的最小宽度。 |
| [width](zh-cn/browser-side/css/css3-属性#width)   | 设置元素的宽度。     |

---

## CSS 显示与可见性

### CSS Display(显示) 与 Visibility（可见性）

CSS display 属性和 visibility 属性都可以用来隐藏某个元素，但是这两个属性有不同的定义，请详细阅读以下内容。

------

### 隐藏元素 - display:none 或 visibility:hidden

隐藏一个元素可以通过把 display 属性设置为"none"，或把 visibility 属性设置为"hidden"。

!> 但是请注意，这两种方法会产生不同的结果。

!> visibility:hidden 可以隐藏某个元素，但隐藏的元素仍需占用与未隐藏之前一样的空间

- display:none 可以隐藏某个元素，且隐藏的元素不会占用任何空间。也就是说，该元素不但被隐藏了，而且该元素原本占用的空间也会从页面布局中消失。

- visibility:hidden 可以隐藏某个元素，但隐藏的元素仍需占用与未隐藏之前一样的空间。也就是说，该元素虽然被隐藏了，但仍然会影响布局。

---

### CSS Display - 块和内联元素

- 块元素是一个元素，占用了全部宽度，在前后都是换行符。

- 内联元素只需要必要的宽度，不强制换行。

---

### 如何改变一个元素显示

?> 可以更改内联元素为块元素，反之亦然

下面的示例把列表项显示为内联元素：

```css
li{display:inline}
```

下面的示例把 span 元素作为块元素：

```css
span {display:block;}
```

!> **注意：**变更元素的显示类型看该元素是如何显示，它是什么样的元素。例如：一个内联元素设置为 display:block 是不允许有它内部的嵌套块元素。

---

### 所有CSS 尺寸 (Dimension)属性

| 属性                                                      | 描述                       |
| :-------------------------------------------------------- | :------------------------- |
| [display](zh-cn/browser-side/css/css3-属性#display)       | 规定元素应该生成的框的类型 |
| [visibility](zh-cn/browser-side/css/css3-属性#visibility) | 指定一个元素是否是可见的   |

---

## CSS 定位

### CSS Positioning (定位)

CSS position 属性，允许您将布局的一部分与另一部分重叠

---

<div style="width:98%;height:100px;position:relative">
    <p style="position:absolute;top:0px;right:0px">定位有时很棘手！</p>
    <div style="width:55%;height:60%;position:absolute;top:0px;left:0px;border:1px solid #c3c3c3;background-color:#e5eecc;padding:0px 10px;z-index:1;">
         <h2>决定显示在前面的元素！</h2>
    </div>
    <div style="width:55%;height:60%;position:absolute;bottom:0px;right:0px;border:1px solid #c3c3c3;background-color:#e5eecc;padding:0px 10px;">
         <h2 style="margin-top:25px">元素可以重叠！</h2>
    </div>
</div>

---

### Positioning (定位)

CSS 定位属性允许你为一个元素定位。它也可以将一个元素放在另一个元素后面，并指定一个元素的内容太大时，应该发生什么。

元素可以使用的顶部，底部，左侧和右侧属性定位。然而，这些属性无法工作，除非事先设定 position 属性。他们也有不同的工作方式，这取决于定位方法.

?> 有四种不同的定位方法。

---

#### Static 定位

HTML 元素的默认值，即没有定位，元素出现在正常的流中。

静态定位的元素不会受到 top, bottom, left, right 影响。

---

#### Fixed 定位

元素的位置相对于浏览器窗口是固定位置。

即使窗口是滚动的它也不会移动：

```css
p.pos_fixed
{
position:fixed;
top:30px;
right:5px;
}
```

Fixed 定位使元素的位置与文档流无关，因此不占据空间。

Fixed 定位的元素和其他元素重叠。

---

#### Relative 定位

相对定位元素的定位是相对其正常位置。

```css
h2.pos_left
{
position:relative;
left:-20px;
}
h2.pos_right
{
position:relative;
left:20px;
}
```

?> 可以移动的相对定位元素的内容和相互重叠的元素，它原本所占的空间不会改变。

?> 相对定位元素经常被用来作为绝对定位元素的容器块。

---

#### Absolute 定位

绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于 \<html>:

Absolutely 定位使元素的位置与文档流无关，因此不占据空间。

Absolutely 定位的元素和其他元素重叠。

---

### 重叠的元素

元素的定位与文档流无关，所以它们可以覆盖页面上的其它元素

z-index 属性指定了一个元素的堆叠顺序（哪个元素应该放在前面，或后面）

一个元素可以有正数或负数的堆叠顺序：

具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面。

!> **注意：** 如果两个定位元素重叠，没有指定 z - index，最后定位在 HTML 代码中的元素将被显示在最前面。

---

### 更多实例

[裁剪元素的外形](https://www.w3cschool.cn/tryrun/showhtml/trycss_clip)

此示例演示如何设置元素的外形。该元素被剪裁成这种形状，并显示出来。

[如何使用滚动条来显示元素内溢出的内容](https://www.w3cschool.cn/tryrun/showhtml/trycss_overflow)

这个例子演示了overflow属性创建一个滚动条，当一个元素的内容在指定的区域过大时如何设置以适应。

[如何设置浏览器自动溢出处理](https://www.w3cschool.cn/tryrun/showhtml/trycss_pos_overflow_auto)

这个例子演示了如何设置浏览器来自动处理溢出。

[更改光标](https://www.w3cschool.cn/tryrun/showhtml/trycss_cursor)

这个例子演示了如何改变光标。

---

### 所有的CSS定位属性

"CSS" 列中的数字表示哪个CSS(CSS1 或者CSS2)版本定义了该属性。

| 属性                                                         | 说明                                                         | 值                                                           | CSS  |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :--- |
| [bottom](zh-cn/browser-side/css/css3-属性#visibility) | 定义了定位元素下外边距边界与其包含块下边界之间的偏移。       | auto   length   inherit                                      | 2    |
| [clip](zh-cn/browser-side/css/css3-属性#clip)     | 剪辑一个绝对定位的元素                                       | shape auto inherit                                         | 2    |
| [cursor](zh-cn/browser-side/css/css3-属性#cursor) | 显示光标移动到指定的类型                                     | url auto crosshair default pointer move e-resize ne-resize nw-resize n-resize se-resize sw-resize s-resize w-resize text wait help | 2    |
| [left](zh-cn/browser-side/css/css3-属性#left)     | 定义了定位元素左外边距边界与其包含块左边界之间的偏移。       | auto length inherit                                      | 2    |
| [overflow](zh-cn/browser-side/css/css3-属性#overflow) | 设置当元素的内容溢出其区域时发生的事情。                     | auto hidden scroll visible inherit                           | 2    |
| [position](zh-cn/browser-side/css/css3-属性#position) | 指定元素的定位类型                                           | absolute fixed relative static inherit                       | 2    |
| [right](zh-cn/browser-side/css/css3-属性#right)   | 定义了定位元素右外边距边界与其包含块右边界之间的偏移。       | auto length inherit                                      | 2    |
| [top](zh-cn/browser-side/css/css3-属性#top)       | 定义了一个定位元素的上外边距边界与其包含块上边界之间的偏移。 | auto length inherit                                      | 2    |
| [z-index](zh-cn/browser-side/css/css3-属性#z-index) | 设置元素的堆叠顺序                                           | numberauto inherit                                        | 2    |

---

## CSS 浮动

### CSS Float(浮动)

CSS float 属性定义元素在哪个方向浮动，浮动元素会生成一个块级框，直到该块级框的外边缘碰到包含框或者其他的浮动框为止。

!> CSS 的 Float（浮动），会使元素向左或向右移动，其周围的元素也会重新排列。

Float（浮动），往往是用于图像，但它在布局时一样非常有用。

---

### 元素怎样浮动

元素的水平方向浮动，意味着元素只能左右移动而不能上下移动。

一个浮动元素会尽量向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。

浮动元素之后的元素将围绕它。

浮动元素之前的元素将不会受到影响。

如果图像是右浮动，下面的文本流将环绕在它左边：

![image-20220726112102028](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/sRgVrt000image-20220726112102028.png)

---

### 彼此相邻的浮动元素

如果你把几个浮动的元素放到一起，如果有空间的话，它们将彼此相邻。

在这里，我们对图片廊使用 float 属性：

![image-20220726112134245](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/ROMMHa000image-20220726112134245.png)

---

### 清除浮动 - 使用 clear

元素浮动之后，周围的元素会重新排列，为了避免这种情况，使用 clear 属性。

clear 属性指定元素两侧不能出现浮动元素。

使用 clear 属性往文本中添加图片廊：

![image-20220726112230952](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/fJdUl5000image-20220726112230952.png)

---

### 更多实例

[为图像添加边框和边距并浮动到段落的右侧](https://www.w3cschool.cn/tryrun/showhtml/trycss_float2)

让我们为图像添加边框和边距并浮动到段落的右侧

[标题和图片向右侧浮动](https://www.w3cschool.cn/tryrun/showhtml/trycss_float3)

让标题和图片向右侧浮动。

[让段落的第一个字母浮动到左侧](https://www.w3cschool.cn/tryrun/showhtml/trycss_float4)

改变样式，让段落的第一个字母浮动到左侧。

[创建一个没有表格的网页](https://www.w3cschool.cn/tryrun/showhtml/trycss_float6)

使用 float 创建一个网页页眉、页脚、左边的内容和主要内容。

---

### CSS 中所有的浮动属性

"CSS" 列中的数字表示不同的 CSS 版本（CSS1 或 CSS2）定义了该属性。

| 属性                                            | 描述                               | 值                           | CSS  |
| :---------------------------------------------- | :--------------------------------- | :--------------------------- | :--- |
| [clear](zh-cn/browser-side/css/css3-属性#clear) | 指定不允许元素周围有浮动元素。     | left right both none inherit | 1    |
| [float](zh-cn/browser-side/css/css3-属性#float) | 指定一个盒子（元素）是否可以浮动。 | left right none inherit      | 1    |

---

## CSS 水平对齐

### CSS 水平对齐 (Horizontal Align)

关于 CSS 中元素的水平对齐 (Horizontal Align)，你可以使用多种属性来进行设置。

---

<div style="text-align:center">
    <div style="border:1px solid gray;padding:2px 14px;margin-left:auto;margin-right:auto;width:45%;background-color:#e5eecc;text-align:left">
         <h2>在CSS中，有几个属性用于元素水平对齐。</h2>
    </div>
</div>

---

#### 块元素对齐

块元素是一个元素，占用了全宽，前后都是换行符。

块元素的例子：

- <h1>

- <p>

- <div>

文本对齐，请参阅 [CSS文本](#css-文本) 章节。.

在这一章中，我们会告诉你块元素如何水平对齐布局。

---

### 中心对齐,使用margin属性

块元素可以把左，右页边距设置为"自动"对齐。

margin属性可任意拆分为左，右页边距设置自动指定，结果都是出现居中元素：

```css
.center
{
margin-left:auto;
margin-right:auto;
width:70%;
background-color:#b0e0e6;
}
```

?>  **提示:** 如果宽度是 100％，对齐是没有效果的。

---

### 使用 position 属性设置左，右对齐

元素对齐的方法之一是使用绝对定位：

```css
.right
{
position:absolute;
right:0px;
width:300px;
background-color:#b0e0e6;
}
```

!> **注意：**绝对定位与文档流无关，所以它们可以覆盖页面上的其它元素。

---

### 使用 float 属性设置左，右对齐

使用 float 属性是对齐元素的方法之一：

```css
.right
{
float:right;
width:300px;
background-color:#b0e0e6;
}
```

---

### 使用 Padding 设置垂直居中对齐

CSS 中一个简单的设置垂直居中对齐的方式就是头部顶部使用 padding:

```css
.center { padding: 70px 0; border: 3px solid green; }
```

如果要水平和垂直都居中，可以使用 padding 和 text-align: center:

```css
.center { padding: 70px 0; border: 3px solid green; text-align: center; }
```

---

## CSS 组合选择符

CSS 组合选择符可以让你直观的明白选择器与选择器之间的关系。

CSS组合选择符包括各种简单选择符的组合方式。

在 CSS3 中包含了四种组合方式:

- 后代选取器(以空格分隔)
- 子元素选择器(以大于号分隔）
- 相邻兄弟选择器（以加号分隔）
- 普通兄弟选择器（以波浪号分隔）

---

### 后代选取器

后代选取器匹配所有指定元素的后代元素。

以下实例选取所有 <p> 元素插入到 <div> 元素中:

```css
div p
{
background-color:yellow;
}
```

---

### 子元素选择器

与后代选择器相比，子元素选择器（Child selectors）只能选择作为某元素子元素的元素。

以下实例选择了<div>元素中所有直接子元素 <p> ：

```css
div>p
{
background-color:yellow;
}
```

---

### 相邻兄弟选择器

相邻兄弟选择器（Adjacent sibling selector）可选择紧接在另一元素后的元素，且二者有相同父元素。

如果需要选择紧接在另一个元素后的元素，而且二者有相同的父元素，可以使用相邻兄弟选择器（Adjacent sibling selector）。

以下实例选取了所有位于 <div> 元素后的第一个 <p> 元素:

```js
div+p
{
background-color:yellow;
}
```

---

### 普通相邻兄弟选择器

```css
div~p
{
background-color:yellow;
}
```

---

## CSS 伪类

### CSS 伪类 (Pseudo-classes)

CSS 伪类是用来添加一些选择器的特殊效果。

由于状态的变化是非静态的，所以元素达到一个特定状态时，它可能得到一个伪类的样式；当状态改变时，它又会失去这个样式。由此可以看出，它的功能和 class 有些类似，但它是基于文档之外的抽象，所以叫伪类。

---

### 语法

伪类的语法：

```css
selector:pseudo-class {property:value;}
```

CSS 类也可以使用伪类：

```css
selector.class:pseudo-class {property:value;}
```

---

### anchor 伪类

在支持 CSS 的浏览器中，链接的不同状态都可以以不同的方式显示

```css
a:link {color:#FF0000;} /* 未访问的链接 */
a:visited {color:#00FF00;} /* 已访问的链接 */
a:hover {color:#FF00FF;} /* 鼠标划过链接 */
a:active {color:#0000FF;} /* 已选中的链接 */
```

!> **注意：** 在 CSS 定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。

!> **注意：** 在 CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。

!> **注意：**伪类的名称不区分大小写。

---

### 伪类和 CSS 类

伪类可以与 CSS 类配合使用：

```css
a.red:visited {color:#FF0000;}

<a class="red" href="css-syntax.html">CSS Syntax</a>
```

如果在上面的例子的链接已被访问，它会显示为红色。

------

### CSS - :first - child 伪类

您可以使用 :first-child 伪类来选择元素的第一个子元素

#### 匹配第一个\<p\> 元素

```css
p:first-child
{
color:blue;
}
```

#### 匹配所有 \<p> 元素中的第一个 \<i> 元素

```css
p > i:first-child
{
color:blue;
}
```

#### 匹配所有作为第一个子元素的 <p> 元素中的所有 <i> 元素

```css
p:first-child i
{
color:blue;
}
```

---

### CSS - :lang 伪类

:lang 伪类使你有能力为不同的语言定义特殊的规则

![image-20220726145253436](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/VrXk1V000image-20220726145253436.png)

---

### 更多实例

[为超链接添加不同样式](https://www.w3cschool.cn/tryrun/showhtml/trycss_link2)
这个例子演示了如何为超链接添加其他样式。

[使用 :focus](https://www.w3cschool.cn/tryrun/showhtml/trycss_link_focus)
这个例子演示了如何使用 :focus 伪类。

---

### 所有CSS伪类/元素

| 选择器                                                       | 示例           | 示例说明                                         |
| :----------------------------------------------------------- | :------------- | :----------------------------------------------- |
| [:link](zh-cn/browser-side/css/css3/css3-选择器#link选择器)       | a:link         | 选择所有未访问链接                               |
| [:visited](zh-cn/browser-side/css/css3/css3-选择器#visited选择器) | a:visited      | 选择所有访问过的链接                             |
| [:active](zh-cn/browser-side/css/css3/css3-选择器#active选择器)   | a:active       | 选择正在活动链接                                 |
| [:hover](zh-cn/browser-side/css/css3/css3-选择器#hover选择器)     | a:hover        | 把鼠标放在链接上的状态                           |
| [:focus](zh-cn/browser-side/css/css3/css3-选择器#focus选择器)     | input:focus    | 选择元素输入后具有焦点                           |
| [:first-letter](zh-cn/browser-side/css/css3/css3-选择器#first-letter选择器) | p:first-letter | 选择每个<p> 元素的第一个字母                     |
| [:first-line](zh-cn/browser-side/css/css3/css3-选择器#first-line选择器) | p:first-line   | 选择每个<p> 元素的第一行                         |
| [:first-child](zh-cn/browser-side/css/css3/css3-选择器#lfirst-child选择器) | p:first-child  | 选择器匹配属于任意元素的第一个子元素的 <]p> 元素 |
| [:before](zh-cn/browser-side/css/css3/css3-选择器#before选择器)   | p:before       | 在每个<p>元素之前插入内容                        |
| [:after](zh-cn/browser-side/css/css3/css3-选择器#after选择器)     | p:after        | 在每个<p>元素之后插入内容                        |
| [:lang(*language*)](zh-cn/browser-side/css/css3/css3-选择器#lang选择器) | p:lang(it)     | 为<p>元素的lang属性选择一个开始值                |

---

### 扩展阅读

CSS 拾遗系列：[浅谈CSS中的伪元素和伪类](zh-cn/browser-side/css/other/CSS-伪元素和伪类)

---

## CSS 伪元素

CSS 伪元素是用来添加一些选择器的特殊效果。

CSS 伪元素控制的内容和元素是没有差别的，但是它本身只是基于元素的抽象，并不存在于文档中，所以称为伪元素。

### 语法

伪元素的语法：

```css
selector:pseudo-element {property:value;}
```

CSS类也可以使用伪元素：

```css
selector.class:pseudo-element {property:value;}
```

?> 在CSS1和CSS2中，伪元素和伪类都采用单冒号进行表示，在CSS3中为了区分伪元素和伪类，规定使用双冒号代表伪元素，单冒号代表伪类，即CSS3标准中应该这么写：`selector.class:：pseudo-element {property:value;}`

?> 虽然CSS3规定了必须使用双冒号，但实际上使用单冒号也可以工作，这是由于CSS的兼容性带来的，但这并不意味着可以无所忌惮的使用单冒号，因为单双冒号的区分，可以给CSS代码带来更高的可读性。

---

### :first-line 伪元素

"first-line" 伪元素用于向文本的首行设置特殊样式。

!> **注意：**"first-line" 伪元素只能用于块级元素。

!> **注意：** 下面的属性可应用于 "first-line" 伪元素：

- font properties
- color properties
- background properties
- word-spacing
- letter-spacing
- text-decoration
- vertical-align
- text-transform
- line-height
- clear

---

### :first-letter 伪元素

"first-letter" 伪元素用于向文本的首字母设置特殊样式：

!> **注意：** "first-letter" 伪元素只能用于块级元素。

!> **注意：** 下面的属性可应用于 "first-letter" 伪元素：

- font properties
- color properties
- background properties
- margin properties
- padding properties
- border properties
- text-decoration
- vertical-align (only if "float" is "none")
- text-transform
- line-height
- float
- clear

---

### Multiple Pseudo-elements

可以结合多个伪元素来使用。

在下面的例子中，段落的第一个字母将显示为红色，其字体大小为 xx-large。第一行中的其余文本将为蓝色，并以小型大写字母显示。

段落中的其余文本将以默认字体大小和颜色来显示：

```css
p:first-letter
{
color:#ff0000;
font-size:xx-large;
}
p:first-line
{
color:#0000ff;
font-variant:small-caps;
}
```

---

### :before 伪元素

":before" 伪元素可以在元素的内容前面插入新内容。

下面的例子在每个 <h1>元素前面插入一幅图片：

```css
h1:before
{
content:url(smiley.gif);
}
```

---

### :after 伪元素

":after" 伪元素可以在元素的内容之后插入新内容。

下面的例子在每个 <h1> 元素后面插入一幅图片：

```css
h1:after
{
content:url(smiley.gif);
}

```

---

### 所有CSS伪类/元素

| 选择器                                                       | 示例           | 示例说明                                         |
| :----------------------------------------------------------- | :------------- | :----------------------------------------------- |
| [:link](zh-cn/browser-side/css/css3/css3-选择器#link选择器)  | a:link         | 选择所有未访问链接                               |
| [:visited](zh-cn/browser-side/css/css3/css3-选择器#visited选择器) | a:visited      | 选择所有访问过的链接                             |
| [:active](zh-cn/browser-side/css/css3/css3-选择器#active选择器) | a:active       | 选择正在活动链接                                 |
| [:hover](zh-cn/browser-side/css/css3/css3-选择器#hover选择器) | a:hover        | 把鼠标放在链接上的状态                           |
| [:focus](zh-cn/browser-side/css/css3/css3-选择器#focus选择器) | input:focus    | 选择元素输入后具有焦点                           |
| [:first-letter](zh-cn/browser-side/css/css3/css3-选择器#first-letter选择器) | p:first-letter | 选择每个<p> 元素的第一个字母                     |
| [:first-line](zh-cn/browser-side/css/css3/css3-选择器#first-line选择器) | p:first-line   | 选择每个<p> 元素的第一行                         |
| [:first-child](zh-cn/browser-side/css/css3/css3-选择器#lfirst-child选择器) | p:first-child  | 选择器匹配属于任意元素的第一个子元素的 <]p> 元素 |
| [:before](zh-cn/browser-side/css/css3/css3-选择器#before选择器) | p:before       | 在每个<p>元素之前插入内容                        |
| [:after](zh-cn/browser-side/css/css3/css3-选择器#after选择器) | p:after        | 在每个<p>元素之后插入内容                        |
| [:lang(*language*)](zh-cn/browser-side/css/css3/css3-选择器#lang选择器) | p:lang(it)     | 为<p>元素的lang属性选择一个开始值                |

---

## CSS 导航栏

### 垂直导航栏

```html
<!DOCTYPE html>
<html>
<head>
<style>
    ul
    {
    list-style-type:none;
    margin:0;
    padding:0;
    }
    a:link,a:visited
    {
    display:block;
    font-weight:bold;
    color:#FFFFFF;
    background-color:#98bf21;
    width:120px;
    text-align:center;
    padding:4px;
    text-decoration:none;
    text-transform:uppercase;
    }
    a:hover,a:active
    {
    background-color:#7A991A;
    }
</style>
</head>

<body>
    <ul>
        <li>
            <a href="#home">主页</a>
        </li>
        <li>
            <a href="#news">新闻</a>
        </li>
        <li>
            <a href="#contact">联系</a>
        </li>
        <li>
            <a href="#about">关于</a>
        </li>
    </ul>
</body>
</html>
```

![image-20220728135639588](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/0SKgPT000image-20220728135639588.png)

---

### 水平导航栏

有两种方法创建横向导航栏。使用**内联**或**浮动**的列表项。

这两种方法都很好，但如果你想链接到具有相同的大小，你必须使用浮动的方法。

---

### 内嵌列表项

```html
<!DOCTYPE html>
<html>
<head>
<style>
ul
{
list-style-type:none;
margin:0;
padding:0;
padding-top:6px;
padding-bottom:6px;
}
li
{
display:inline;
}
a:link,a:visited
{
font-weight:bold;
color:#FFFFFF;
background-color:#98bf21;
text-align:center;
padding:6px;
text-decoration:none;
text-transform:uppercase;
}
a:hover,a:active
{
background-color:#7A991A;
}

</style>
</head>

<body>
<ul>
<li><a href="#home">Home</a></li>
<li><a href="#news">News</a></li>
<li><a href="#contact">Contact</a></li>
<li><a href="#about">About</a></li>
</ul>

<p><b>Note:</b> If you only set the padding for a elements (and not ul), the links will go outside the ul element. Therefore, we have added a top and bottom padding for the ul element.</p>
</body>
</html>
```

![image-20220728140103089](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/ID8feg000image-20220728140103089.png)

---

### 浮动列表项

在上面的例子中链接有不同的宽度。对于所有的链接宽度相等，浮动元素，并指定为 元素的宽度：

```html
<!DOCTYPE html>
<html>
<head>
<style>
    ul
    {
    list-style-type:none;
    margin:0;
    padding:0;
    overflow:hidden;
    }
    li
    {
    float:left;
    }
    a:link,a:visited
    {
    display:block;
    width:120px;
    font-weight:bold;
    color:#FFFFFF;
    background-color:#98bf21;
    text-align:center;
    padding:4px;
    text-decoration:none;
    text-transform:uppercase;
    }
    a:hover,a:active
    {
    background-color:#7A991A;
    }

</style>
</head>

<body>
    <ul>
        <li>
            <a href="#home">主页</a>
        </li>
        <li>
            <a href="#news">新闻</a>
        </li>
        <li>
            <a href="#contact">联系</a>
        </li>
        <li>
            <a href="#about">关于</a>
        </li>
    </ul>
</body>
</html>
```

实例解析：

- `float:left` - 使用浮动块元素的幻灯片彼此相邻
- `display:block` - 显示块元素的链接，让整体变为可点击链接区域（不只是文本），它允许我们指定宽度
- width:60px - 块元素默认情况下是最大宽度。我们要指定一个60像素的宽度
- `display:inline;` -默认情况下，元素是块元素。在这里，我们删除换行符之前和之后每个列表项，以显示一行 。

![image-20220728140222943](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/h1gOu7000image-20220728140222943.png)

---

## CSS 下拉菜单

### 基本下拉菜单

```html
<!DOCTYPE html>
<html>
<head>
<title>下拉菜单实例|W3Cschool教程(w3cschool.cn)</title>
<meta charset="utf-8">
<style>
.dropbtn {
    background-color: #4CAF50;
    color: white;
    padding: 16px;
    font-size: 16px;
    border: none;
    cursor: pointer;
}

.dropdown {
    position: relative;
    display: inline-block;
}

.dropdown-content {
    display: none;
    position: absolute;
    background-color: #f9f9f9;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
}

.dropdown-content a {
    color: black;
    padding: 12px 16px;
    text-decoration: none;
    display: block;
}

.dropdown-content a:hover {background-color: #f1f1f1}

.dropdown:hover .dropdown-content {
    display: block;
}

.dropdown:hover .dropbtn {
    background-color: #3e8e41;
}
</style>
</head>
<body>

<h2>下拉菜单</h2>
<p>鼠标移动到按钮上打开下拉菜单。</p>

<div class="dropdown">
  <button class="dropbtn">下拉菜单</button>
  <div class="dropdown-content">
    <a href="http://www.w3cschool.cn">W3Cschool教程 1</a>
    <a href="http://www.w3cschool.cn">W3Cschool教程 2</a>
    <a href="http://www.w3cschool.cn">W3Cschool教程 3</a>
  </div>
</div>

</body>
</html>
```

![image-20220728140526977](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/pXdWgY000image-20220728140526977.png)

#### 实例解析

**HTML 部分：**

我们可以使用任何的 HTML元素来打开下拉菜单，如\<span>, 或 a\<button> 元素。

使用容器元素 (如：\<div>) 来创建下拉菜单的内容，并放在任何你想放的位置上。

使用\<div> 元素来包裹这些元素，并使用 CSS 来设置下拉内容的样式。

**CSS 部分：**

`.dropdown` 类使用 `position:relative`, 这将设置下拉菜单的内容放置在下拉按钮 (使用 `position:absolute`) 的右下角位置。

`.dropdown-content` 类中是实际的下拉菜单。默认是隐藏的，在鼠标移动到指定元素后会显示。 注意 `min-width` 的值设置为 160px。你可以随意修改它。 **注意:** 如果你想设置下拉内容与下拉按钮的宽度一致，可设置 `width` 为 100% ( `overflow:auto` 设置可以在小尺寸屏幕上滚动)。

我们使用 `box-shadow` 属性让下拉菜单看起来像一个"卡片"。

`:hover` 选择器用于在用户将鼠标移动到下拉按钮上时显示下拉菜单。

---

### 下拉内容对齐方式

**float:left;**

**float:right;**

```css
.dropdown-content {
right: 0; }
```

---

### 更多实例

[图片下拉](https://www.w3cschool.cn/tryrun/showhtml/trycss_dropdown_image)
该实例演示了如何如何在下拉菜单中添加图片。

[导航条下拉](https://www.w3cschool.cn/tryrun/showhtml/trycss_dropdown_navbar)
该实例演示了如何在导航条上添加下拉菜单。

---

## CSS 图片廊

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>W3Cschool教程(w3cschool.cn)</title>
<style>


    div.img
    {
      margin: 2px;
      border: 1px solid #000000;
      height: auto;
      width: auto;
      float: left;
      text-align: center;
    }
    div.img img
    {
      display: inline;
      margin: 3px;
      border: 1px solid #ffffff;
    }
    div.img a:hover img {border: 1px solid #0000ff;}
    div.desc
    {
      text-align: center;
      font-weight: normal;
      width: 120px;
      margin: 2px;
    }
</style>
</head>


<body>
    <div class="img">
         <a target="_blank" href="javascript;:"><img src="/statics/images/course/klematis_small.jpg" alt="Klematis" width="110" height="90"></a>
         <div class="desc">Add a description of the image here</div>
    </div>
    <div class="img">
         <a target="_blank" href="javascript;:"><img src="/statics/images/course/klematis2_small.jpg" alt="Klematis" width="110" height="90"></a>
         <div class="desc">Add a description of the image here</div>
    </div>
    <div class="img">
         <a target="_blank" href="javascript;:"><img src="/statics/images/course/klematis3_small.jpg" alt="Klematis" width="110" height="90"></a>
         <div class="desc">Add a description of the image here</div>
    </div>
    <div class="img">
         <a target="_blank" href="javascript;:"><img src="/statics/images/course/klematis4_small.jpg" alt="Klematis" width="110" height="90"></a>
         <div class="desc">Add a description of the image here</div>
    </div>
</body>
```

![image-20220728140826390](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/GmGwOA000image-20220728140826390.png)

---

### CSS 图像透明

使用 CSS 很容易创建透明的图像。

!> **注意：**CSS Opacity 属性是W3C 的 CSS3 建议的一部分。

---

### 创建一个透明图像

```html
<!DOCTYPE html>
<html>
<head>
<style>
    img
    {
    opacity:0.4;
    filter:alpha(opacity=40); /* For IE8 and earlier */
    }
</style>
</head>
<body>

    <h1>Image Transparency</h1>
    <img src="/statics/images/course/klematis.jpg" width="150" height="113" alt="klematis">
    <img src="/statics/images/course/klematis2.jpg" width="150" height="113" alt="klematis">

    <p><b>Note:</b> In IE, a &lt;!DOCTYPE&gt; must be added for the :hover selector to work on other elements than the &lt;a&gt; element.</p>
</body>
</html>
```

![image-20220728140924368](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/EYZ3GV000image-20220728140924368.png)

---

### 图像的透明度 - 悬停效果

```html
<!DOCTYPE html>
<html>
<head>
<style>
    img
    {
    opacity:0.4;
    filter:alpha(opacity=40); /* For IE8 and earlier */
    }
    img:hover
    {
    opacity:1.0;
    filter:alpha(opacity=100); /* For IE8 and earlier */
    }
</style>
</head>
<body>

    <h1>Image Transparency</h1>
    <img src="/statics/images/course/klematis.jpg" width="150" height="113" alt="klematis">
    <img src="/statics/images/course/klematis2.jpg" width="150" height="113" alt="klematis">

    <p><b>Note:</b> In IE, a &lt;!DOCTYPE&gt; must be added for the :hover selector to work on other elements than the &lt;a&gt; element.</p>
</body>
</html>
```

![image-20220728141030812](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/79h1iZ000image-20220728141030812.png)

---

### 透明的盒子中的文字

```html
<!DOCTYPE html>
<html>
<head>
<style>
    div.background
    {
      width: 500px;
      height: 250px;
      background: url(/statics/images/course/klematis.jpg) repeat;
      border: 2px solid black;
    }
    div.transbox
    {
      width: 400px;
      height: 180px;
      margin: 30px 50px;
      background-color: #ffffff;
      border: 1px solid black;
      opacity:0.6;
      filter:alpha(opacity=60); /* For IE8 and earlier */
    }
    div.transbox p
    {
      margin: 30px 40px;
      font-weight: bold;
      color: #000000;
    }
</style>
</head>
<body>
    <div class="background">
        <div class="transbox">
            <p>This is some text that is placed in the transparent box.
            This is some text that is placed in the transparent box.
            This is some text that is placed in the transparent box.
            This is some text that is placed in the transparent box.
            This is some text that is placed in the transparent box.
            </p>
        </div>
    </div>
</body>
</html>
```

![image-20220728141104003](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/JDnOLd000image-20220728141104003.png)

---

### CSS 图像拼合技术

### 图像拼合

图像拼合就是单个图像的集合。

有许多图像的网页可能需要很长的时间来加载和生成多个服务器的请求。

使用图像拼合会降低服务器的请求数量，并节省带宽。

---

与其使用三个独立的图像，不如我们使用这种单个图像`（"img_navsprites.gif"）`：

![navigation images](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/Hk1Dgu000img_navsprites.gif)

有了CSS，我们可以只显示我们需要的图像的一部分。

在下面的例子CSS指定显示 "img_navsprites.gif" 的图像的一部分：

```html
<!DOCTYPE html>
<html>
<head>
<style>
    img.home {
        width: 46px;
        height: 44px;
        background: url(/statics/images/course/img_navsprites.gif) 0 0;
    }

    img.next {
        width: 43px;
        height: 44px;
        background: url(/statics/images/course/img_navsprites.gif) -91px 0;
    }
</style>
</head>
<body>

    <img class="home" src="/statics/images/course/img_trans.gif"><br><br>
    <img class="next" src="/statics/images/course/img_trans.gif">

</body>
</html>
```

![image-20220728141937317](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/emxexY000image-20220728141937317.png)

**实例解析：**

- <img class="home" src="img_trans.gif" /> -因为不能为空，`src` 属性只定义了一个小的透明图像。显示的图像将是我们在 CSS 中指定的背景图像
- 宽度：46px;高度：44px; - 定义我们使用的那部分图像
- `background:url(img_navsprites.gif) 0 0`; - 定义背景图像和它的位置（左 0px，顶部 0px）

这是使用图像拼合最简单的方法，现在我们使用链接和悬停效果。

---

### 图像拼合 - 创建一个导航列表

```html
<!DOCTYPE html>
<html>
<head>
<style>
    #navlist{position:relative;}
    #navlist li{margin:0;padding:0;list-style:none;position:absolute;top:0;}
    #navlist li, #navlist a{height:44px;display:block;}

    #home{left:0px;width:46px;}
    #home{background:url('/statics/images/course/img_navsprites.gif') 0 0;}

    #prev{left:63px;width:43px;}
    #prev{background:url('/statics/images/course/img_navsprites.gif') -47px 0;}

    #next{left:129px;width:43px;}
    #next{background:url('/statics/images/course/img_navsprites.gif') -91px 0;}
</style>
</head>

<body>
    <ul id="navlist">
      <li id="home"><a href="http://www.w3cschool.cn/css"></a></li>
      <li id="prev"><a href="http://www.w3cschool.cn/css"></a></li>
      <li id="next"><a href="http://www.w3cschool.cn/css"></a></li>
    </ul>
</body>
</html>
```

![image-20220728142016090](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/GURvN4000image-20220728142016090.png)

**实例解析：**

- `#navlist{position:relative;}` - 位置设置相对定位，让里面的绝对定位
- `#navlist li{margin:0;padding:0;list-style:none;position:absolute;top:0;}`- `margin`和`padding` 设置为0，列表样式被删除，所有列表项是绝对定位
- `#navlist li, #navlist a{height:44px;display:block;}` - 所有图像的高度是 44px

现在开始每个具体部分的定位和样式：

- `#home{left:0px;width:46px;}` - 定位到最左边的方式，以及图像的宽度是 46px
- `#home{background:url(img_navsprites.gif) 0 0;}` - 定义背景图像和它的位置（左0px，顶部0px）
- `#prev{left:63px;width:43px;}` - 左外边距定位63px（＃home宽46px+项目之间的一些多余的空间），宽度为43px。
- `#prev{background:url('img_navsprites.gif') -47px 0;}` - 定义背景图像向右侧定位47px（＃home宽46px+分隔线的1px）
- `#next{left:129px;width:43px;}`- 左外边距定位129px(#prev 63px + #prev宽是43px + 剩余的空间), 宽度是43px.
- `>#next{background:url('img_navsprites.gif') no-repeat -91px 0;}`
- \- 定义背景图像

- 向右侧定位

- 91px（＃home 46px+1px的分割线+＃prev宽43px+1px的分隔线）

---

### 图像拼合 - 悬停效果

我们的新图像 ("img_navsprites_hover.gif") 包含三个导航图像和三幅图像：

![navigation images](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/HLWMrv000img_navsprites_hover.gif)

因为这是一个单一的图像，而不是6个单独的图像文件，当用户停留在图像上不会有延迟加载。

我们添加悬停效果只添加三行代码：

```css
#home a:hover{background: url('img_navsprites_hover.gif') 0 -45px;}
#prev a:hover{background: url('img_navsprites_hover.gif') -47px -45px;}
#next a:hover{background: url('img_navsprites_hover.gif') -91px -45px;}
```

**实例解析：**

- 由于该列表项包含一个链接，我们可以使用`：hover` 伪类
- `#home a:hover{background: transparent url(img_navsprites_hover.gif) 0 -45px;}` - 对于所有三个悬停图像，我们指定相同的背景位置，只是每个再向下45px

---

## CSS 媒体类型

媒体类型允许你指定文件将如何在不同媒体呈现。该文件可以以不同的方式显示在屏幕上，在纸张上，或听觉浏览器等等。

---

### 媒体类型

某些 CSS 属性仅仅被设计为针对某些媒介。

比方说 `voice-family` 属性被设计为针对听觉用户终端。

其他一些属性可用于不同的媒体类型。

例如，`font-size`属性可用于屏幕和印刷媒体，但有不同的值。

屏幕和纸上的文件不同，通常需要一个更大的字体，`sans - serif`字体比较适合在屏幕上阅读，而 serif 字体更容易在纸上阅读。

---

### @media 规则

@media 规则允许在相同样式表为不同媒体设置不同的样式。

在下面的例子告诉我们浏览器屏幕上显示一个14像素的 Verdana 字体样式。但是如果页面打印，将是10个像素的 Times 字体。请注意，`font-weight`在屏幕上和纸上设置为粗体：

```css
<style>

@media screen

{ p.test {font-family:verdana,sans-serif;font-size:14px; } }

@media print

{ p.test {font-family:times,serif;font-size:10px;} }

@media screen,print

{ p.test {font-weight:bold;}}

</style>
```

有关 @media 规则的更多信息，请参考[CSS @media查询](zh-cn/browser-side/css/css3-属性#media)

---

### 其他媒体类型

**注意：**媒体类型名称不区分大小写。

| 媒体类型   | 描述                                                   |
| :--------- | :----------------------------------------------------- |
| all        | 用于所有的媒体设备。                                   |
| aural      | 用于语音和音频合成器。                                 |
| braille    | 用于盲人用点字法触觉回馈设备。                         |
| embossed   | 用于分页的盲人用点字法打印机。                         |
| handheld   | 用于小的手持的设备。                                   |
| print      | 用于打印机。                                           |
| projection | 用于方案展示，比如幻灯片。                             |
| screen     | 用于电脑显示器。                                       |
| tty        | 用于使用固定密度字母栅格的媒体，比如电传打字机和终端。 |
| tv         | 用于电视机类型的设备。                                 |

---

## 响应式Web设计

### 什么是响应式 Web 设计?

响应式 Web 设计让你的网页能在所有设备上有好显示。

响应式 Web 设计只使用 HTML 和 CSS。

响应式 Web 设计不是一个程序或Javascript脚本。

---

## 设计最好的用户体验

友好的用户体验是网页可以在任何设备上展示和操作，设备包括桌面系统设备，平板电脑，iPhone等手机等。

网页应该根据设备的大小自动调整内容。

#### 桌面设备

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/8PFcQl000rwd_desktop.png)

#### 平板设备

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/pOyqX9000rwd_tablet.png)

#### 手机设备

![img](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/uPic/2MQT2E000rwd_phone.png)

页面的设计与开发根据用户行为以及设备环境(系统平台、屏幕尺寸、屏幕定向等)进行相应的响应和调整称之为响应式 Web 设计。

---

### 相关参考

- [viewport](zh-cn/browser-side/css/响应式设计/01-viewport)
- [网格视图](zh-cn/browser-side/css/响应式设计/02-网格视图)
- [媒体查询](zh-cn/browser-side/css/响应式设计/03-媒体查询)
- [图片](zh-cn/browser-side/css/响应式设计/04-图片)
- [视频](zh-cn/browser-side/css/响应式设计/05-视频)
- [框架](zh-cn/browser-side/css/响应式设计/06-框架)

---

