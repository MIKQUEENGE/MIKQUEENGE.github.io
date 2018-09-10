---
title: HTML与CSS学习记录
toc: true
date: 2018-09-10 14:04:59
categories:
- Web
tags:
- HTML
- CSS
---



《HTML与CSS进阶教程读书笔记》

---



# HTML基础知识

## HTML与XHTML

HTML指超文本标记语言，是构成网页文档的主要语言。我们常说的HTML指HTML4.01。

XHTML指扩展的超文本标记语言，是XML风格的、更严格、更纯净的HTML。

两者的主要区别：

- XHTML标签必须闭合。
- XHTML标签和属性必须小写。
- XHTML标签属性必须加引号。
- XHTML标签用id属性代替name属性。

## id和class

由于id属性具有唯一性，因此W3C建议，对于页面关键的结构或大结构，才能使用id属性，其他地方使用class属性。

<u>因为搜索引擎是根据标签的语义和id属性来识别的，因此id属性的使用和命名都需要谨慎。</u>

一般来说，定义多个class的目的在于：一个class抽取公共样式，一个class定义单独样式。

## 标题栏小图标

在`head`标签内加入：

```html
<link rel="shortcut icon" type="image/x-icon" href="图标路径.ico" />
```

其中`rel`和`type`是固定属性不用更改，只需要修改图片路径即可。



# 语义化

HTML的精髓在于标签的语义。搜索引擎根据HTML代码识别页面结构。

编写语意结构良好的页面的好处：

- 利于开发调试和后期维护。
- 利于搜索引擎优化。

应优先使用正确的语义化标签，如果没有语义化标签可用，再考虑div或者span等无语义标签。

## 标题语义化

h1-h6是标题标签，相比于其他标签，它们在搜索引擎优化（SEO）中占有相当重要的地位。

一般用到h4，h5和h6权重和普通标签差不多，很少使用。

对于标题语义化，我们需要注意的是：

- 一个页面只能有一个h1标签。
- h1-h6之间不要出现断层。
- 不要用标题标签来定义样式（如为了加粗字体而为文本加上标题标签）。
- 不要用div来代替标题标签。

div是无语义的标签，如果使用div代替标题标签会使网页在SEO中丢失大量权重。

## 图片语义化

### alt属性和title属性

alt是给搜索引擎看的，title是给用户看的。

搜索引擎根据alt属性或上下文判断图片内容。

因此**img标签必须添加alt属性。**

### figure元素和figcaption元素

对于图片+图注的效果，使用figure和figcaption来增强图片语义化。

例：

```html
<figure>
    <img src="xxx" alt="xxx" />
    <figcaption>这是一个图注</figcaption>
</figure>
```

更详细的介绍可以看[这一篇博客](https://www.w3cplus.com/html5/quick-tip-the-right-way-to-use-figure-and-figcaption-elements.html)。

## 表格语义化

| 标签    | 说明             |
| ------- | ---------------- |
| table   | 表格             |
| caption | 标题             |
| thead   | 表头（语义划分） |
| tbody   | 表身（语义划分） |
| tfoot   | 表尾（语义划分） |
| tr      | 行               |
| th      | 表头单元格       |
| td      | 表格单元格       |

## 表单语义化

### label标签

label标签的for属性有两个作用：

- 语义上绑定了label元素和表单元素。（\<label for="*element_id*">）
- 当我们点击label中的文本时，其关联的表单元素也会获得焦点。

例：

```html
<input id="rdo" name="rdo" type="radio" /><label for="rdo">单选框</label>
```

### fieldset标签和legend标签

fieldset标签用于给表单元素进行分组并绘制一个边框，legend标签用于定义某一组表单的标题。

例如这个[例子](http://www.runoob.com/tags/tag-fieldset.html)：

```html
<form>
  <fieldset>
    <legend>Personalia:</legend>
    Name: <input type="text"><br>
    Email: <input type="text"><br>
    Date of birth: <input type="text">
  </fieldset>
</form>
```

作用：

- 增强表单语义。
- 可以使用fieldset标签的disabled属性来禁用整个组中的表单元素。

## 其他语义化

### 换行符\<br />

W3C标准规定，\<br />标签只能用于段落中的换行。即只能用于p标签内部。

### 无序列表ul

对于列表型数据，不建议使用div实现，而应用无序列表或有序列表实现。

为了实现外观效果，一般使用无序列表而不是有序列表。

### strong 标签和em标签

W3C对这两个标签赋予了“强调”的语义。

可以在CSS中重新定义它们的样式而不会改变它们的语义。

### del标签和ins标签

这两个标签一般是配合使用表示更新文本：“delete”和“insert”，被删除的文本和被更新的文本。

一般会用CSS重新定义它们的样式。

[实例链接](http://www.runoob.com/tags/tag-del.html)

### img标签

对于什么时候使用img标签，什么时候使用背景图片，应该根据HTML的语义来判断。

- img标签：作为HTML的一部分，希望被搜索引擎识别。

- 背景图片： 只起到修饰作用，不希望被搜索引擎识别。

## 语义化验证

通过去掉CSS样式，观察页面是否还有很好的可读性来判断一个页面是否语义良好。

## HTML5舍弃的标签

下边这些已经被舍弃的标签(仅为了定义样式的标签和很少使用或已经被新标签代替的标签)应停止使用：

- \<acronym>  定义首字母缩写，应用abbr代替。
- \<applet> 定义嵌入的applet，应用object代替。
- \<basefont>
- \<big>
- \<center>
- \<dir> 定义目录列表，应用ul代替。
- \<font>
- \<frame>
- \<frameset>
- \<noframes>
- \<strike>
- \<tt>

# CSS基础知识

## CSS单位

### px

pixel，像素，一个图片或计算机屏幕中最小的点。

### %

CSS中支持百分比的属性：

- **width、height、font-size**，它们的百分比是相对于父元素的“相同元素”的值来计算的。
- **line-height**，它的百分比是相对于**父元素**的**font-size**值来计算的。
- **vertical-align**，它的百分比是相对于**当前元素**的**line-height**值来计算的。

### em

1em等于当前元素的以px为单位的font-size值，

若当前元素的font-size值没有定义，则从父元素继承，

若当前元素的所有祖先元素都没有定义font-size，则继承浏览器默认的font-size值：16px。

<u>使用em的小技巧：</u>首行缩进使用 `text-indent: 2em`实现。

### rem

CSS3新引入的单位，指相对根元素（即html元素）的字体大小。

## CSS特性

### 继承性

指子元素继承了父元素的某些样式属性。

在CSS中，具有继承性的样式有三大类：

- **文本**相关属性： font--family，font-size，font-style，font-weight，font，line-height，text-align，text-indent，word-spacing。
- **列表**相关属性： list-style-image，list-style-position，list-style-type，list-style。
- **颜色**相关属性： color。

### 层叠性

“后者居上”原则。

CSS的层叠性指样式的覆盖。对于具有**相同权重**的**相同属性**，以最后定义的值为准。

## CSS优先级

### 引用方式

行内样式>(内部样式=外部样式)

若同时存在权重相同内部样式和外部样式，则以最后引入的样式为准。

### 继承方式

以最近的祖先元素为准。

### 指定样式

常见的伪元素——:before、:after、:first-letter、:first-line。

常见的伪类——:hover、:first-child等。

常用的选择器优先级：行内样式>id选择器>class选择器>元素选择器。

选择器权值表：

| 选择器      | 权值 |
| ----------- | ---- |
| 通配符      | 0    |
| 伪元素      | 1    |
| 元素选择器  | 1    |
| class选择器 | 10   |
| 伪类        | 10   |
| 属性选择器  | 10   |
| id选择器    | 100  |
| 行内样式    | 1000 |

### 继承样式和和指定样式

指定样式权重更高。

### !important

权值最高，不推荐使用。

## CSS引入方式

- 导入样式表（加载html后加载css，不推荐）
- 外部样式表（link标签）
- 内部样式表（style标签）
- 行内样式表

## CSS选择器

CSS出去基本的选择器（元素选择器、id选择器、class选择器、群组或分组选择器），

还有<u>层次选择器</u>：

| 选择器 | 说明                                      |
| ------ | ----------------------------------------- |
| M N    | 后代选择器，选择M元素所有内部后代N元素    |
| M>N    | 子代选择器，选择M元素所有内部子代N元素    |
| M~N    | 兄弟选择器，选择M元素所有同级N元素        |
| M+N    | 相邻选择器，选择M元素相邻的下一个同级元素 |

# CSS规范

