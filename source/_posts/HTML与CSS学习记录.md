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

- `<acronym>`  定义首字母缩写，应用abbr代替。
- `<applet>` 定义嵌入的applet，应用object代替。
- `<basefont>`
- `<big>`
- `<center>`
- `<dir>` 定义目录列表，应用ul代替。
- `<font>`
- `<frame>`
- `<frameset>`
- `<noframes>`
- `<strike>`
- `<tt>`

# CSS基础知识

## CSS单位

### px

pixel，像素，一个图片或计算机屏幕中最小的点。

### 百分比%

CSS中支持百分比的属性：

- **width、height、font-size**，它们的百分比是相对于父元素的“相同元素”的值来计算的。
- **line-height**，它的百分比是相对于**父元素**的**font-size**值来计算的。
- **vertical-align**，它的百分比是相对于**当前元素**继承的**line-height**值来计算的。

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

## 命名规范

### CSS文件命名

开发阶段按照功能模块划分CSS文件。

| 文件名      | 说明                                 |
| ----------- | ------------------------------------ |
| reset.css   | 重置样式，重置元素默认样式           |
| global.css  | 全局样式，全站公用，定义页面基础样式 |
| themes.css  | 主题样式，用于实现换肤功能           |
| module.css  | 模块样式，用于模块的样式             |
| master.css  | 母版样式，用于母版页的样式           |
| columes.css | 专栏样式，用于专栏的样式             |
| forms.css   | 表单样式，用于表单的样式             |
| mend.css    | 补丁样式，用于维护、修改的样式       |
| print.css   | 打印样式，用于打印的样式             |

### id和class命名

建议使用中划线命名，例如`column-title`。

为了避免class命名的重复，一般取父元素的class名作为前缀，例如`column-title`。

| 网页主体部分 | 命名                          |
| ------------ | ----------------------------- |
| 最外层       | wrapper(一般用于包裹整个页面) |
| 头部         | header                        |
| 内容         | content                       |
| 侧栏         | sidebar                       |
| 栏目         | column                        |
| 热点         | hot                           |
| 新闻         | news                          |
| 下载         | download                      |
| 标志         | logo                          |
| 导航条       | nav                           |
| 主体         | main                          |
| 左侧         | main-left                     |
| 右侧         | main-right                    |
| 底部         | footer                        |
| 友情链接     | friendlink                    |
| 加入我们     | joinus                        |
| 版权         | copyright                     |
| 服务         | service                       |
| 登录         | login                         |
| 注册         | register                      |

| 导航部分 | 命名          |
| -------- | ------------- |
| 主导航   | main-nav      |
| 子导航   | sub-nav       |
| 边导航   | side-nav      |
| 左导航   | leftside-nav  |
| 右导航   | rightside-nav |
| 顶导航   | top-nav       |

| 菜单部分 | 命名    |
| -------- | ------- |
| 菜单     | menu    |
| 子菜单   | submenu |

| 其他     | 命名          |
| -------- | ------------- |
| 标题     | title         |
| 摘要     | summary       |
| 登录条   | loginbar      |
| 搜索     | search        |
| 标签页   | tab           |
| 广告     | banner        |
| 小技巧   | tips          |
| 图标     | icon          |
| 法律声明 | siteinfolegal |
| 网站地图 | sitemap       |
| 工具条   | tool、toolbar |
| 首页     | homepage      |
| 子页     | subpage       |
| 合作伙伴 | partner       |
| 帮助     | help          |
| 指南     | guide         |
| 滚动     | scroll        |
| 提示信息 | msg           |
| 投票     | vote          |
| 相关文章 | related       |
| 文章列表 | list          |

## 书写规范

对于功能代码，应该集中放在一起，

对于其他代码，应按照如下顺序：

1. 影响文档流属性（布局属性）——display，position，float，clear等
2. 自身盒模型属性——width，height，padding，margin，border，overflow等
3. 文本性属性——font，line-height，text-align，text-indent，vertical-align等
4. 装饰性属性——color，background-color，opacity等
5. 其他属性——cursor，content，list-style，quotes等

例如：

```css
#main {
  /* 影响文档流属性 */
  display: inline-block;
  position: absolute;
  top: 0;
  left: 0;
  /* 盒子模型属性 */
  width: 100px;
  height: 100px;
  border: 2px solid gray;
  /* 文本性属性 */
  font-size: 15px;
  font-weight: bold;
  text-indent: 2em;
  /* 装饰性属性 */
  color: white;
  background-color: red;
  /* 其他属性 */
  cursor: pointer;
}
```

## 注释规范

由于压缩工具会删除所有的注释，因此有时为了保留版权声明等注释信息，需要在注释内容前加一个叹号，如`/*! 注释内容 */`，这样压缩工具就不会删除这条注释信息。

### 顶部注释

```css
/*
 *@description:说明
 *@author:作者
 *@update:更新时间，如2018-09-10 17:42
 */
```

### 模块注释

```css
/* 导航部分，开始 */
......
/* 导航部分，结束 */
```

### 简单注释

```css
/* 单行注释 */
```

或者

```css
/*多行注释
 *多行注释
 *多行注释
 */
```

## CSS reset

重置样式，去除元素在浏览器中的默认样式。

是否使用CSS reset根据实际开发需求而定。

# 盒子模型

![标准盒子模型](https://gss0.baidu.com/-4o3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D600%2C800/sign=5ad0ba6a0c7b02080c9c37e752e9deeb/0824ab18972bd407012c41327d899e510eb30911.jpg)

![IE盒子模型](https://gss0.baidu.com/9fo3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D600%2C800/sign=6cdf3ae9fbfaaf5184b689b9bc64b8d6/1b4c510fd9f9d72ac29d82d2d22a2834359bbb00.jpg)

## 外边距叠加

又称为“margin叠加”，指当两个外边距相遇时会“合二为一”。叠加后的外边距为两个外边距的最大值。

<u>只有普通文档流中块框的垂直外边距才会发生外边距合并。行内框、浮动框或绝对定位之间的外边距不会合并。</u>

以下图片均来自[w3school](http://www.w3school.com.cn/css/css_margin_collapsing.asp)

![同级元素](http://www.w3school.com.cn/i/ct_css_margin_collapsing_example_1.gif)

![父子元素](http://www.w3school.com.cn/i/ct_css_margin_collapsing_example_2.gif)

![空元素1](http://www.w3school.com.cn/i/ct_css_margin_collapsing_example_3.gif)

![空元素2](http://www.w3school.com.cn/i/ct_css_margin_collapsing_example_4.gif)

![外边距合并的意义](http://www.w3school.com.cn/i/ct_css_margin_collapsing.gif)

## 负margin

- 当margin-top或者margin-left为负数时，**当前元素**会被拉向指定方向。
- 当margin-bottom或者margin-right为负数时，**后续元素**会被拉向指定方向。

[这里有一篇文章](https://www.jianshu.com/p/549aaa5fabaa)讲得不错，可以参考一下。

[圣杯布局、双飞翼布局](https://www.cnblogs.com/star91/p/5773436.html)就是利用这个实现的。

## overflow

当浮动引起父元素高度塌陷时，可以为父元素加上`overflow: hidden`来清除浮动。

# display属性

| 属性值       | 说明                               |
| ------------ | ---------------------------------- |
| inline       | 行内元素                           |
| block        | 块元素                             |
| inline-block | 行内块元素                         |
| table        | 以表格形式显示，类似于table元素    |
| table-row    | 以表格行形式显示，类似于tr元素     |
| table-cell   | 以表格单元格形式显示，类似于td元素 |
| none         | 隐藏元素                           |

## 块元素

- 独占一行
- 内部可以容纳其他块元素或行元素
- 可以定义width和height
- 可以定义四个方向的margin

## inline元素

- 可以与其他行内元素位于同一行
- 可以容纳行内元素，但不能容纳块元素
- 无法定义width和height
- 可以定义margin-left和margin-right，不能定义margin-top和margin-bottom

## inline-block元素

- 可以定义width和height
- 可以与其他行内元素位于同一行

常见的inline-block元素：img元素和input元素

## display: table-cell

可以用于实现：

- [图片垂直居中](https://www.jianshu.com/p/a7ee7dd60166)于元素
- 等高布局
- 自动平均划分元素，并在同一行显示

## 去除inline-block元素间距

在父元素中添加`font-size: 0`

# 文本效果

| 文本属性        | 说明                         |
| --------------- | ---------------------------- |
| text-decoration | 下划线、删除线、顶划线       |
| text-transform  | 文本大小写                   |
| font-variant    | 将英文文本转换为小型大写字母 |
| text-indent     | 段落首行缩进                 |
| text-align      | 文本水平对齐                 |
| vertical-align  | 文本垂直对齐                 |
| line-height     | 行高                         |
| letter-spacing  | 字距                         |
| word-spacing    | 词距                         |

## text-indent

可以使用	`text-indent: -9999px;`来隐藏文本。

## text-align

主要使用的值为left、right、center，对文字、inline元素、inline-block元素都起作用，对块元素不起作用。

利用`margin: 0 auto`实现块元素的水平居中。

`text-align: center`在父元素中定义，`margin: 0 auto`在当前元素中定义。

## line-height

关于顶线、中线、基线、底线可以自行查阅。

行高（line-height）指的是两行基线之间的距离。

- 将height和line-height设为相同值可以实现文字垂直居中。
- 当取值为%或者em时，是相对与父元素的font-size计算的。
- 当取值为无单位数字时，是相对于当前元素的font-size计算的。

## vertical-align

vertical-align对inline、inline-block、table-cell元素有效，对块元素无效。

用于定义<u>周围的文字、inline元素、inline-block元素</u>相对于该元素**基线**的垂直对齐方式。

可以取负长度值和百分比值。

### 取值

1. 负值 ： `vertical-align: -2px`指的是该元素相对于基线向下偏移2px；

2. 百分比 ： 相对于当前元素继承的line-height值计算的，也是该元素相对于基线偏移的值；

3. [关键字](http://www.runoob.com/cssref/pr-pos-vertical-align.html) （前四个比较常用）：



| 值           | 描述                                           |
| ------------ | ---------------------------------------------- |
| **top**      | 把元素的顶端与行中最高元素的顶端对齐           |
| **middle**   | 把此元素放置在父元素的中部。                   |
| **baseline** | 默认。元素放置在父元素的基线上。               |
| **bottom**   | 把元素的底端与行中最低的元素的顶端对齐。       |
| text-top     | 把元素的顶端与父元素字体的顶端对齐             |
| text-bottom  | 把元素的底端与父元素字体的底端对齐。           |
| sub          | 垂直对齐文本的下标。                           |
| super        | 垂直对齐文本的上标                             |
| inherit      | 规定应该从父元素继承 vertical-align 属性的值。 |



### 应用

- 为img添加`vertical-align: middle`可以实现图片与周围的文字居中对齐
- 要使块元素（如div）也可以使用此属性，可以为其先定义`display: table-cell`

# 表单效果

## radio与checkbox

默认情况下由于是基线对齐因此视觉上会感觉单选框或复选框旁边的文字比它们低，这个时候可以使用vertical-align来让他们垂直居中对齐。

可以使用关键字，也可以使用数值。

## textarea

- 可以使用max-width和max-height来限制拖拽大小
- 可以使用`resize: none`来禁止拖拽

要使textarea在不同浏览器中具有相同的外观，可以：

- 使用CSS的width和height定义大小
- 使用`overflow: auto`来定义textarea滚动条自适应

## 表单对齐

书上给了**注册**的例子：

![图片来源于网络](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1536639853274&di=16c88d63b9118620ba546c2f6237b78c&imgtype=0&src=http%3A%2F%2Fpic.orsoon.com%2Fuploads%2Fallimg%2F24631428484696.png)

实现方法：

1. 每一行表单分为左栏加若干右栏
   1. 所有行的左栏长度相等
   2. 所有行的右栏所有盒子长度之和相等
   3. 左栏一般为一个label，右栏为若干文本框
2. 所有左栏和右栏盒子都设为左浮动
3. 左栏添加属性`text-align: right`使得文字右对齐
4. 每一行左栏盒子长度加上所有右栏盒子长度之和等于行宽
5. 每一行由一个p包裹住，并为p添加`overflow: hidden`来清除浮动

然后我又去看了一下各网站的**登录**界面，基本上是一个icon+一个input的模式：

![图片来源于网络](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1536641005969&di=02e6a2469281303e926eb8878cf64db9&imgtype=0&src=http%3A%2F%2Fd.hiphotos.baidu.com%2Fexp%2Fw%3D480%2Fsign%3D00237679845494ee87220e111df7e0e1%2Fa686c9177f3e67093ac0b23933c79f3df9dc5530.jpg)



实现方法：

- icon使用`position: absolute`脱离文档流并盖在input上
- input将padding-left调到合适大小使得输入框不被icon盖住



# 浮动布局

## 文档流

简单来说就是元素在页面中出现的先后顺序。

- 正常文档流 ： “normal flow”，指默认情况下页面元素的布局情况。
- 脱离文档流：脱离正常文档流。要改变正常文档流，使用浮动和定位方法。

## 浮动

可以使元素移到左边或者右边，并且允许后边的文字和元素环绕着它。

浮动后使用margin来定义和其他元素之间的间距。

绝对定位的元素忽略float属性。

float的取值表如下，默认为**none**：

| 值      | 描述                                                 |
| ------- | ---------------------------------------------------- |
| left    | 元素向左浮动。                                       |
| right   | 元素向右浮动。                                       |
| none    | 默认值。元素不浮动，并会显示在其在文本中出现的位置。 |
| inherit | 规定应该从父元素继承 float 属性的值。                |



- 当一个元素添加float属性为left或者right时，它将变为block类型。
- 浮动元素脱离正常文档流，若其height大于父元素的height或者父元素的height未定义，会造成父元素高度塌陷。可以为父元素添加`overflow: hidden`来解决。
- 若父元素和子元素都是浮动元素，则父元素会自适应地包含子元素。

- 若兄弟元素不是浮动元素，由于浮动元素脱离文档流，可能会出现覆盖等情况。

## 清除浮动

- `clear: both`，用于浮动元素后边的元素，表示两边不允许出现浮动元素。
- `overflow: hidden`，用于浮动元素的父元素，但会隐藏超出父元素的内容部分。
- 实际开发中，更经常使用`:after`伪元素结合`clear: both`来实现。
- 为了兼容ie，为父元素添加`zoom: 1`来消除浮动。

# 定位布局

## [属性值](http://www.runoob.com/cssref/pr-class-position.html)

| 值                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [static](http://www.runoob.com/css/css-positioning.html#position-static) | <u>默认值</u>。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。 |
| [absolute](http://www.runoob.com/css/css-positioning.html#position-absolute) | 生成绝对定位的元素，相对于 <u>static 定位以外的第一个父元素</u>进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| [fixed](http://www.runoob.com/css/css-positioning.html#position-fixed) | 生成固定定位的元素，相对于<u>浏览器窗口</u>进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| [relative](http://www.runoob.com/css/css-positioning.html#position-relative) | 生成相对定位的元素，相对于<u>其正常位置</u>进行定位。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。 |
| [sticky](http://www.runoob.com/css/css-positioning.html#position-sticky) | 粘性定位，该定位基于用户滚动的位置。它的行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置。**注意:** Internet Explorer, Edge 15 及更早 IE 版本不支持 sticky 定位。 Safari 需要使用 -webkit- prefix (查看以下实例)。 |
| inherit                                                      | 规定应该从父元素继承 position 属性的值。                     |
| initial                                                      | 设置该属性为默认值，详情查看 [CSS initial 关键字](http://www.runoob.com/cssref/css-inherit.html)。 |

- absolute会将元素转换为块元素。
- 若想要子元素相对于父元素定位，一般给父元素添加`position: relative`，给子元素定义`position: absolute`来实现。祖先元素同理。

## z-index属性

默认情况下设置z-index无效，只有当元素定义position为relative、absolute、fixed时才会激活，z-index值越大，其堆叠顺序越高，越靠上（z方向上的靠上）。

# CSS图形

由于图片大小比较大，数据传输量大且一张图片会引发一次HTTP请求，因此对徐图形效果，一般更倾向于用CSS实现。

这里有一篇[CSS制作图形速查表](https://www.w3cplus.com/css/css-simple-shapes-cheat-sheet)总结得比较全面，可以参考。

另外对于带有边框的图形，一般是用大小不同的两个相同图形实现，小的覆盖在大的上边。

# 性能优化

## 属性缩写

- border：`border: 1px solid red`
  - 若不想要底边框，可以加上`border-bottom: 0`
  - 若只想要底边框，可以`border-bottom: 1px solid red`

- padding： 
  - `padding: 10px`指上右下左均为10px
  - `padding: 10px 20px`指上下为10px，左右为20px
  - `padding: 10px 20px 30px 40px`的顺序为上右下左，从上开始按照顺时针顺序

- margin： 类似于padding
- background： `background: url('xxx.jpg') no-repeat 80px 40px`,最后为background-position
- font： `font: "微软雅黑" 12px/1.5em bold`
  - 顺序为`font-family`、`font-size`、`line-height`、`font-weight`
  - 简写形式必须指定`font-family`和`font-size`的值，其他属性没有指定则为默认值
  - 简写形式中`font-size`和`line-height`之间需要加入一个斜杠`/`

- color： 十六进制的颜色值若每两位值相同，可以缩写一半，比如`color: #112233`可以缩写为`color: #123`

## 语法压缩

- 当一个CSS规则只有一两个属性的时候，使用横向书写
- 可以省略最后一个属性的分号
- background-image、cursor等属性url()中的路径不用加引号
- 如果某一个属性值为0，则不需要加单位
- 如果某一个属性值是以0为开头的小数，可以吧0省略
- 使用群组选择器合并相同样式
- 若同一个父元素的多个子元素都定义了相同的可继承属性，把这些属性定义在父元素中来精简代码

## CSS压缩

书中推荐了两个在线的压缩工具：[CSS Compressor](https://csscompressor.com) 和 [YUI Compressor](http://tool.oschina.net/jscompress)

以YUI Compressor为例，它会对CSS文件执行如下操作：

- 删除所有注释
- 删除无用空白符
- 删除结尾分号
- 删除属性值为0的单位
- 删除以0开头的小数前的0
- 合并相似属性（属性缩写）
- 将RGB颜色转换为十六进制颜色

## 图片压缩

书中推荐的图片压缩工具：

在线的[JPEGmini](https://www.jpegmini.com)和[TinyPNG](https://tinypng.com)以及本地的[ImageOptim](https://imageoptim.com/versions.html)

## 高性能的选择器

浏览器对选择器规则是从右到左进行解析的。

CSS选择的匹配效率：

1. id选择器
2. class选择器
3. 元素选择器
4. 相邻选择器
5. 子选择器
6. 后代选择器
7. 通配符选择器
8. 属性选择器
9. 伪类选择器

因此在使用选择器时应注意：

- 尽量不要使用通配符
- 不要在id选择器和class选择器前添加元素名
- 选择器最好不要超过三层，靠右的选择条件应尽可能精确
- 避免使用后代选择器，尽量少使用子选择器

# CSS技巧

## 水平居中

- 文字、inline元素和inline-*元素： `text-align: center`
- 块元素： `margin: 0 auto`

## 垂直居中

- 行内块元素使用`vertical-align: middle`
- 块元素将display改为table-cell然后使用vertical-align
- 多行文字使用一个标签将文字包起来并设为table-cell，然后再设置vertical-align
- 单行文字设置line-height和height属性值相同来实现

## CSS Sprite

又称为CSS精灵或CSS**雪碧图**，它将零散的小背景图合并成一张大背景图，然后再利用background-position属性进行定位从而现实小背景图。

使用CSS Sprite技术时，需要注意：

- 在开发后期而不是开发前期使用此技术
- 有条理地组织图片顺序，应将小背景图按照类别、风格、大小等分门别类地放好
- 控制雪碧图的大小，当图片大小小于200KB时传输时间是差不多的，因此雪碧图应控制在200KB以内

书中推荐了两个CSS Sprite工具：[CSS Sprite Generator](http://css.spritegen.com/) 和 [Sprite Cow](http://www.spritecow.com/)

## Icon Font图标

使用字体文件实现小图标效果，从而减少图片的使用。

推荐的Icon Font网站：http://www.iconfont.cn/

网站上就有[使用教程](http://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.d0091c141&helptype=code)

# 重要概念

## 包含块

containing block，决定一个元素大小和定位的元素。

时视觉格式化模型中的一个重要概念，与CSS盒子模型类似。其作用主要是为其内部的后代元素提供一个参考。

- 根元素（HTML元素）没有父元素，它存在的包含块被称为初始包含块
- 定位为fixed的元素的包含块为浏览器窗口
- 定位为是static和relative的元素的包含块是它最近的块级（block、inline-block或table-cell）祖先元素创建的
- 定位为absolute的元素的包含块是它最近的定位不是static的祖先元素，可以是块元素也可以是行内元素

## 层叠上下文

![层叠级别图](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1537245373&di=c7455fbd6881b1a07ae8ac81a79f474e&imgtype=jpg&er=1&src=http%3A%2F%2Fwww.w3cplus.com%2Fsites%2Fdefault%2Ffiles%2Fblogs%2F2018%2F1808%2Fz-index-15.png)

一个元素在z轴上的堆叠顺序：

- 层叠级别越大越靠上
- 同等层叠级别，后边的堆叠在前边的上边（后来者居上）
- 不同的层叠上下文比较的是父级层叠上下文，与自身无关

## BFC和IFC

BFC： block formatting context， 块级格式上下文

IFC： inline formatting context， 行级格式上下文

若一个元素具备以下任何一个条件，则会创造创造一个新的BFC：

- 根元素
- float属性不是none
- position属性是absolute或fixed
- overflow属性值不是visible
- display属性为inline-block、table-caption、table-cell

W3C描述BFC的特点为：

- 在一个BFC中，盒子从顶端开始垂直一个接着一个地排列。两个相邻盒子之间的垂直间距有margin决定。**同一个BFC中**，两个相邻块盒子之间**垂直方向上的外边距**会叠加。
- 在一个BFC中，每一个盒子的左外边界（margin-left）会紧贴着容器的border-left，右边同理，即使存在浮动元素也是如此。

可以得到结论：

1. 在一个BFC内部，盒子会在垂直方向上一个接着一个地排列
2. 在一个BFC内部，相邻的margin-top、margin-bottom会叠加
3. 在一个BFC内部，每一个盒子的左外边界（margin-left）会紧贴着容器（包含盒子）的border-left，即使存在浮动元素也是如此
4. 在一个BFC内部，如果存在内部元素是一个新的BFC，并且存在内部元素是浮动元素，则这个新的BFC的区域不会与浮动元素的区域重叠
5. BFC就是页面上一个隔离的盒子，该盒子内部的子元素不会影响到外边的元素
6. 计算一个BFC的高度时，其内部浮动元素的高度也会计算其中

BFC的用途：

- 创建BFC来避免垂直外边距叠加（例如使用div将一个盒子包起来并给这个div添加overflow属性）
- 创建BFC来清除浮动（为父元素添加`overflow: hidden`，利用结论第六条）
- 创建BFC来实现[自适应布局](https://blog.csdn.net/michael8512/article/details/76473835)



---

好了到这里，这本书就看完了，一些细节的东西了解到了很多，下面开始看html5+css3。

\- 2018 - 09 - 11 -