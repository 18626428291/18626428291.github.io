# CSS3 多媒体查询<!-- {docsify-ignore} -->

**文档更新日期: {docsify-updated}**



## CSS3 多媒体查询

CSS3 的多媒体查询继承了 CSS2 多媒体类型的所有思想： 取代了查找设备的类型，CSS3 根据设置自适应显示。

媒体查询可用于检测很多事情，例如：

- viewport(视窗) 的宽度与高度
- 设备的宽度与高度
- 朝向 (智能手机横屏，竖屏) 。
- 分辨率

目前很多针对苹果手机，Android 手机，平板等设备都会使用到多媒体查询。

---

## 多媒体查询语法

多媒体查询由多种媒体组成，可以包含一个或多个表达式，表达式根据条件是否成立返回 true 或 false。

```css
@media not|only mediatype and (expressions) {
    CSS-Code;
}
```

如果指定的多媒体类型匹配设备类型则查询结果返回 true，文档会在匹配的设备上显示指定样式效果。

除非你使用了 not 或 only 操作符，否则所有的样式会适应在所有设备上显示效果。

- **not:** not是用来排除掉某些特定的设备的，比如 @media not print（非打印设备）。
- **only:** 用来定某种特别的媒体类型。对于支持Media Queries的移动设备来说，如果存在only关键字，移动设备的Web浏览器会忽略only关键字并直接根据后面的表达式应用样式文件。对于不支持Media Queries的设备但能够读取Media Type类型的Web浏览器，遇到only关键字时会忽略这个样式文件。
- **all:** 所有设备，这个应该经常看到。

你也可以在不同的媒体上使用不同的样式文件：

```css
<link rel="stylesheet" media="mediatype and|not|only (expressions)" href="print.css">
```

---

## CSS3 多媒体类型

| 值     | 描述                             |
| :----- | :------------------------------- |
| all    | 用于所有多媒体类型设备           |
| print  | 用于打印机                       |
| screen | 用于电脑屏幕，平板，智能手机等。 |
| speech | 用于屏幕阅读器                   |

---

## 多媒体查询简单实例

[在屏幕可视窗口尺寸大于 480 像素的设备上修改背景颜色](https://www.w3cschool.cn/tryrun/showhtml/trycss3_media_queries1)

[在屏幕可视窗口尺寸大于 480 像素时将菜单浮动到页面左侧](https://www.w3cschool.cn/tryrun/showhtml/trycss3_media_queries2)

---

## CSS3 @media 参考

有关 @media 规则的更多信息，请参考[CSS @media查询](zh-cn/browser-side/css/css3-属性#media)

[相关教程参考](zh-cn/browser-side/css/README#css-媒体类型)