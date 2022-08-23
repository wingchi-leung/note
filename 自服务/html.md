## Html

万维网： www  （World wide web ） 

html (Hyper Text Markup Language) ： 超文本标记语言，描述网页的语言。 

html标签： 由尖括号包围的关键词，如<html>



```html
<!DOCTYPE html> // 声明： 告诉浏览器： 我是一个html文档 

<html>      //html标签的开始标签  

  <head>    //HTML 标题 

​    <title>我只是个AI</title>

  </head>

  <body>

​    <h1>嘿嘿。。。</h1> //标题 

        <p>陈永棋还是一如既往的傻逼</p> //html段落

  </body>

</html>   //html结束标签 由/开头。 


```

### 常用标签

#### head 

< head > 标签用于定义文档的头部，它是所有头部元素的容器。   

< head > 中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等等。 

#### h1--h6

 是定义标题 ， h1 最大， h6最小 

h就是修饰标题的， 不要用来增大字体！ 因为python也会读，不是只有人会读！ 

#### p

是一个段落

#### body

定义的是文档的内容 ，图像，超链接都在里面  

#### img

有两个重要的属性：src指定图片的地址，alt 指定图像的代替文本

属性之间用空格空开就好了

img标签是没有结束标签的，是一个空标签

`        <img src ="D:/新建文件夹/image.jpg" alt ="她好可爱"/> `

##### 图片处理

```css
background-image: url("../images/autumn.jpg") ;
background-repeat:no-repeat; 不重复
background-size:100% 100%; 大小
```



#### a

href属性

`<p> 在这里找到你想要的 <a href ="(URL)"> 去吧！</a> </p> `

#### meta 

> <meta> 标签用于描述页面内容，关键词，作者，最新修订时间以及其它元信息。

1. 告诉浏览器他的编码方式
2. 实现网页跳转
3. 写给爬虫看

 meta 标签用于描述页面内容，关键词，作者，最新修订时间以及其它元信息 

meta标签的内容不会显示在页面中，但是经常被机器解析（如爬虫）

元信息可以被浏览器（指导它如何加载和显示页面）所使用，也可以有利于搜索引擎收录页面（指定关键词）。

注意： <meta> 标签永远位于<head> 标签的内部 

 必须的属性 ： content 

 可选属性： `http-equiv( 值： content-type | refresh | set-cookie) ` ,	`name(author | description | keywords | ..) (描述：把content的属性关联到一个名称)` 



##### 示例

1. 为了让网页尺寸自适应，可以加上这行代码(通过 `meta` 标签引入一个新的方法)

`<meta name ="viewport" content ="width=device-width ,initial-scale =1.0"> ` 

2. 

```php+HTML
<!DOCTYPE html> 
<html>
    <head>
        <title>第一个程序在这里</title>
        <meta charset = "utf-8"> 
        <meta name ="viewoirt" content ="width=device-width ,initial-scale =1.0 "> 
        <meta name ="keyword" content=" 沙雕麻花班 来学web吧！">
        <meta name ="description" content ="《快来学习》" >
        <meta name ="author" content ="甲鱼和月亮">
        <meta http-equiv ="refresh" content ="3;https://www.bilibili.com/video/BV1QW411N762?p=2" >  
    </head>
    <body>
        <img src ="D:/新建文件夹/image.jpg"height =200  width =200 alt ="她好可爱"/> 
        <p> 在这里找到你想要的 <a href ="https://man.ilovefishc.com/pageHTML5/a.html" > 去吧！</a> </p>
        <h1>Hello world</h1> 
        <p>I love myself as a bastard </p>
        <p>I get nervous in here </p> 
    </body>
</html>

```



#### sytle 

用来写css的。 

Css规则： 选择器，属性，值

  <style> 标签用于为 HTML 文档定义样式信息。  

 style 元素可以出现在 HTML 文档中的各个部分，一个文档可以包含多个 style 元素。  

media 属性： 给不同的场景下让页面做出相应改变， 如： 打印时，将背景扣掉。

```html
 <style>
 h1选择器{ 
      属性-值 ： color: rgb(0, 183, 255 );
      background :black;
}
     
        </style>
```



#### link

链接一个外部样式表

 link 元素是一个空元素，它仅包含属性。==此元素只能存在于 head 部分，不过它可出现任何次数==。

在 HTML 中，<link> 标签没有结束标签。 

 link 元素定义了 6 个属性，其中 rel 属性是必选的，它说明了当前文档与被链接资源之间的关系。

属性      值                                                  备注

href    URL                                                指定被链接资源的URL 

rel      stylesheet, icon,canonical..   指定当前文档与被链接资源之间的关系。 

type     MIME_type                      规定被链接文档的MIME类型





stylesheet ：调用外部样式表

<link rel ="stylesheet"  href ="c.css"  type ="text/css"> 

#### base

用于设置相对url的解析基准。

用来设置一个基准url，让html文档中的相对连接在此基础上进行解析。 

> 标签必须位于 <head>标签内部，并尽量靠前，以便随后的元素中的相对 URL 可以用上其设置的基准 URL。 

base 有两个属性： href 和target 

target是指定在何处打开超链接。 

_blank ： 在新窗口中打开

_parent:在当前父窗口中打开。

_self 当前窗口打开（默认） 

#### span

<span> 标签被用来组合文档中的行内元素。以便通过样式来格式化它们。 

 span 没有固定的格式表现。当对它应用样式时，它才会产生视觉上的变化。 

示例：

```html
    <style>
        #autumn {color : yellow}
        #spring {color:aquamarine}
    </style>
</head>    
<body>
    <!--一是说明p是块级元素 a是行内元素 二是体现span的作用-->
    <a href="http://bbs.fishc.com">论坛</a>
    <p>叶子在<span id="autumn">秋天</span>掉落</p>
    <p>在<span id="spring">春天</span>发芽</p> 
```

 



#### 格式化

</br> , <strong>用来强调</strong> ,<em></em> 

`  <p>Hi im pargraph of<br/> text </p>`

`<p>im a <strong> strong  </strong> <em>haha </em>text </p>`





#### 插入音频

html的audio元素

<audio> 元素是一个 HTML5 元素，在 HTML 4 中是非法的，但在所有浏览器中都有效。

```html
<body>  
    <audio src="wav\陈鸿宇 - 船子.mp3" controls="controls" autoplay ="autoplay" ></audio>
</body>
```

autoplay ：自动播放

controls： 如果出现该属性，则向用户显示控件，比如播放按钮。 

loop： 顺换播放

src： 音频地址 

#### 列表类标签

ul+li 创建无序列表（更常用） 

ol+li 创建有序列表

```html
<ol> 
      <li>i'am an item</li>
      <li>i'am an item</li>
</ol>
```

把ol改成ul即可 

#### 块级元素 

HTML（超文本标记语言）中元素大多数都是“块级”元素。块级元素占据其父元素（容器）的整个空间，因此创建了一个“块”。

通常浏览器会在块级元素前后另起一个新行 

和行内元素区别

块级大多为结构性标记  行内元素大多为描述性标记 

 块级元素

`h1-h6,p,ul,ol,dl,table,form,divaddress,center`

 1.总是从新的一行开始

 2.高度、宽度都是可控的

 3.宽度没有设置时，默认为100%

 4.块级元素中可以包含块级元素和行内元素

·行内元素

`span,a,br,b,strong,img,sup,sub,i,em,del,u,input,textarea,select`

 1.和其他元素都在一行

 2.高度、宽度以及内边距都是不可控的

 3.宽高就是内容的高度，不可以改变

 4.行内元素只能行内元素，不能包含块级元素



给行内元素换行： br元素 

#### pre

用于定义预格式化文本效果

#### html字符实体

在 HTML 中，某些字符是预留的。

不能使用小于号（<）和大于号（>），这是因为浏览器会误认为它们是标签。

如果希望正确地显示预留字符，我们必须在 HTML 源代码中使用字符实体（character entities）。

#### 引用

<q> 标签用于简短的行内引用。如果需要从周围内容分离出来比较长的部分（通常显示为缩进的块）用 <blockquote> 标签。 

 <q> 标签在本质上是一样的。不同之处在于它们的显示和应用。



#### cite元素

<cite> 标签通常表示它所包含的文本对某个参考文献的引用，比如书籍或者杂志的标题。

按照惯例，引用的文本将以斜体显示。

#### bdo 

bdo 元素可覆盖默认的文本方向。


 属性：`dir (ltr|rtl|) (定义文字的方向)`

 

#### 表格

<table> 标签定义 HTML 表格。

简单的 HTML 表格由 table 元素以及一个或多个 tr、th 或 td 元素组成。

tr(table row) 元素定义表格行，th (table head cell) 元素定义表头，td (table data cell)元素定义表格单元。 

说明：  border-collapse :collapse; 的效果是不要双杆线 

```html
    <style>
        table{
            border : 1px solid black;
            border-collapse :collapse;
        }
        td,th{
            border : 1px solid black;
        }
       
    </style>
</head>    
<body>
   <table>
       <tr>
           <th>姓名</th>
           <th>年龄</th>
           <th>座右铭</th>
       </tr>
       <tr>
           <td>小甲鱼</td>
           <td>18</td>
           <td>哈哈哈哈哈哈哈哈</td>
       </tr>
       <tr>
           <td>不二如是</td>
           <td>45</td>
           <td>年龄45,心理还是18岁哦</td>
       </tr>
   </table>
```

#### 表单form

`<form>`标签用于给用户输入创建HTML表单
表单能够包含input,menus,textarea,fieldset,legend,label元素 用于向服务器传输数据

   

```html
<form action="from_action.asp" method = "get">
</form>
```

form属性 

- action：规定当提交表单时，向何处发送表单数据
- method： 规定如何发送表单数据到action指定的页面 （可以以 get或者post来发送 

##### 示例

```html
<body>
<h1>Hello index</h1>
<form action="/user" method = "get">
    <input value="REST-GET 提交" type="submit"/>
</form>

<form action="/user" method="post">
    <input value="REST-POST 保存" type="submit" />
</form>
<form action="/user" method = "post">
    <input name="_method" type="hidden" value="DELETE"/>
    <input value="REST-DELETE 删除" type="submit"/>
</form>
<form action="/user" method = "post">
    <input name="_method" type="hidden" value="PUT"/>
    <input value="REST-PUT" type="submit"/>
</form>
</body>
```



#### 自动填充

from的属性之一： 规定是否启用表单自动填充功能，默认是on， 可以手动设置为off 

```html
 <form autocomplete="off">
        你叫什么名字:<input tpye="text" name="name"><br><br>
        你是不是基佬:<input tpye="text" name="gay"><br><br>
        <button type="submit">提交</button>
    </form>
```





##### 设置默认值

input的属性value

`    你是不是基佬:<input tpye="text" value="是" name="gay">`

##### 自动聚焦

input 

`    你叫什么名字:<input tpye="text" autofocus name="name">`

##### 废除

​    你是不是基佬:<input tpye="text" value="是" disabled name="gay">

##### 只读

​    你是不是基佬:<input tpye="text" value="是" readonly name="gay"><br><br>

#### label 

<label> 标签为 input 元素定义标注（标记）。

label 元素不会向用户呈现任何特殊效果。不过它为鼠标用户改进了可用性。当用户选择该标签时，浏览器就会自动将焦点转到和标签相关的表单控件上。

<label> 标签的 for 属性应当与相关元素的 id 属性相同。

#### fieldset 

ieldset 元素可将表单内的相关元素分组。

<fieldset> 标签将表单内容的一部分打包，生成一组相关表单的字段。

当一组表单元素放到 <fieldset> 标签内时，浏览器会以特殊方式来显示它们，它们可能有特殊的边界、3D 效果，或者甚至可创建一个子表单来处理这些元素。

#### option

和select 一起用 ，定义一个下拉列表的选项

浏览器将 <option> 标签中的内容作为 <select> 标签的菜单或是滚动列表中的一个元素显示。

option 元素位于 select 元素内部。

注释：<option> 标签通常需要使用 value 属性，此属性会指示出被送往服务器的内容。

```html
<label for="sex">性别:
 <select name="sex">
 	<option value="male">男</option>
	<option value="female">女</option>
 </select>
</label><br></br>
```

#### optgroup  



#### input

<input> 标签用于搜集用户信息。

根据不同的 type 属性值，输入字段拥有很多种形式。输入字段可以是文本字段、复选框、掩码后的文本控件、单选按钮、按钮等等。

属性type ： 指定input元素的类型

| 值       | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| button   | 定义可点击按钮（多数情况下，用于通过 JavaScript 启动脚本）。 |
| checkbox | 定义此input元素在首次加载时应当被选中。                      |
| file     | 定义输入字段和 "浏览"按钮，供文件上传。                      |
| hidden   | 定义隐藏的输入字段。                                         |
| image    | 定义图像形式的提交按钮。                                     |
| password | 定义密码字段。该字段中的字符被掩码。                         |
| radio    | 定义单选按钮。                                               |
| reset    | 定义重置按钮。重置按钮会清除表单中的所有数据。               |
| submit   | 定义提交按钮。提交按钮会把表单数据发送到服务器。             |
| text     | 定义单行的输入字段，用户可在其中输入文本。默认宽度为 20 个字符。 |

属性：name 定义input元素的名称 

属性：value 规定input元素的值 对于不同的输入类型，value的属性用法不同 

type="button"/"reset"/"submit" 定义的是按钮上显示的文本

type="text"/"password"/"hidden" 

#### map

定义一个客户端图像映射。图像映射（image-map）指带有可点击区域的一幅图像。

必须属性： id ，为map标签定义唯一的名称

可选属性：name 描述image-map规定名称 

##### area元素

<area> 标签定义图像映射中的区域（注：图像映射指得是带有可点击区域的图像）。

area 元素总是嵌套在 <map> 标签中。

必须属性： alt值  text 描述：定义此区域的替换文本 

可选属性：

|  属性  |            值             |                描述                 |
| :----: | :-----------------------: | :---------------------------------: |
| coords |          坐标值           |        定义可点击区域的坐标         |
| shape  |  default,rect,circ,poly   |           定义区域的形状            |
| target | _blank,_parent,_self,_top | 规定在何处打开href属性指定的目标url |

实例：

```html

<img src="images/springNight.png" usemap="#girl">
   <map name="girl">
      <area shape="circle" coords="784,241,163" ,alt="A girl" href="https://movie.douban.com/subject/26935251/" target ="_blank"> 
  </map>
```



<img>标签中的usemap属性和map中的name属性相关联 



#### pitcure/source

picture通过包含source元素来为不同的显示/设备场景提供图像版本。浏览器会选择最匹配的子  元素

如果没有匹配的，就选择  元素src属性中的URL。然后，所选图像呈现在<img>元素占据的空间中。

```html
  <picture>
        <source media="(min-width:1024px)" srcset="images/autumn.jpg">
        <source media="(min-width:512px)" srcset="images/springNight.png">
        <img src="images/forest.png" alt="forest" style="width:auto;">
    </picture>
```

####  video

viedo标签用来放视频 

| autoplay | autoplay           | 如果出现该属性，则视频在就绪后马上播放。                     |
| -------- | ------------------ | ------------------------------------------------------------ |
| controls | controls           | 如果出现该属性，则向用户显示控件，比如播放按钮。             |
| height   | *pixels*           | 设置视频播放器的高度。                                       |
| loop     | loop               | 如果出现该属性，则当媒介文件完成播放后再次开始播放。         |
| muted    | muted              | 如果出现该属性，视频的音频输出为静音。                       |
| poster   | *URL*              | 规定视频正在下载时显示的图像，直到用户点击播放按钮。         |
| preload  | auto metadata none | 如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。 |
| src      | *URL*              | 要播放的视频的 URL。                                         |
| width    | *pixels*           | 设置视频播放器的宽度。                                       |

## html容器

 ![img](https://camo.githubusercontent.com/1a74f2ff8954c4dcd42f16a78081664186c379a4a6f3152b4105deef1c7ce047/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303230365f313535302e706e67) 

1. header 定义简洁形式的内容
2. div 是一个无语义的容器也很常用
3. nav 定义页面主导航
4. main定义主内容
5. article 定义独立的文章内容
6. section 定义文档中的节 
7. aside 定义侧边栏
8. footer 定义页脚部分
9. details 定义额外的细节
10. summary 定义details 元素的标题

### section 

表示一个包含在HTML文档中的独立部分，它没有更具体的语义元素来表示，一般来说会有包含一个标题。

- 一般通过是否包含一个标题 element) 作为子节点 来 辨识每一个<section>。

###  footer

 表示最近一个章节内容或者根节点(sectioning root ）元素的页脚。一个页脚通常包含该章节作者、版权数据或者与文档相关的链接等信息。 



### div

HTML <div> 元素 (或 HTML 文档分区元素) 是一个通用型的流内容容器，在不使用CSS的情况下，其对内容或布局没有任何影响。

div的css布局

`display：inline `（内联） 此时不能自己设置宽度，由内容决定。 

`display : flex ` flex要为div的父级元素设置 

### flex布局

flexible BOx 模型， 是一种一维的布局模型（  一次只能处理一个维度上的元素布局，一行或者一列 ）

使用 flex 布局时，首先想到的是两根轴线 — 主轴和交叉轴。主轴由 `flex-direction`定义，另一根轴垂直于它。我们使用 flexbox 的所有属性都跟这两根轴线有关。 

主轴

由 `flex-direction`定义，可以取4个值

`row` ,`row-reverse`,`column` ,`column-reverse` 

交叉轴： 

**交叉轴垂直主轴**。   主轴)设成了 `row` 或者 `row-reverse` 的话，交叉轴的方向就是沿着列向下的。 



**Flex容器**

采用flexbox的区域就叫做flex容器 . 为了创建 flex 容器， 我们把一个容器的 `display` 属性值改为 `flex` 或者 `inline-flex。` 容器中的直系子元素就会变为 **flex 元素**。所有CSS属性都会有一个初始值.

```css
 <section>
                <div>a</div>
                <div>b</div>
                <div>c</div>
</section>

 div{
     margin : auto; 
     width: 100px;
     background: rgb(230, 78, 78);
}
 section{
     color:gray;
     background:white;
     padding : 20px;
     display : flex; 
     flex-direction :row;
}
```

## CSS基础

cascading style sheets 层叠样式表 

**css的单位　：**　html中的单位只有一种，那就是像素px，所以单位是可以省略的，但是在CSS中不一样。 **CSS中的单位是必须要写的**，因为它没有默认单位。

绝对单位：

1 `in`=2.54`cm`=25.4`mm`=72`pt`=6`pc`。

`px`：像素　

 `em`：印刷单位相当于12个点 `%`：百分比，相对周围的文字的大小



### 字体属性

太多了，要用自己来查更好　

[ｃｓｓ字体和文本属性]: https://github.com/qianguyihao/Web/blob/master/02-CSS%E5%9F%BA%E7%A1%80/01-CSS%E5%B1%9E%E6%80%A7%EF%BC%9A%E5%AD%97%E4%BD%93%E5%B1%9E%E6%80%A7%E5%92%8C%E6%96%87%E6%9C%AC%E5%B1%9E%E6%80%A7.md	"ｃｓｓ"



下划线： 

text-decoration: underline ;





```ｃｓｓ
p{
	font-size: 50px; 		/*字体大小*/
	line-height: 30px;      /*行高*/
	font-family: 幼圆,黑体; 	/*字体类型：如果没有幼圆就显示黑体，没有黑体就显示默认*/
	font-style: italic ;		/*italic表示斜体，normal表示不倾斜*/
	font-weight: bold;	/*粗体*/
	font-variant: small-caps;  /*小写变大写*/
}
```





### 鼠标属性

鼠标的ｃｕｒｓｏｒ有属性值：

- `auto`：默认值。浏览器根据当前情况自动确定鼠标光标类型。
- `pointer`：IE6.0，竖起一只手指的手形光标。就像通常用户将光标移到超链接上时那样。
- `hand`：和`pointer`的作用一样：竖起一只手指的手形光标。就像通常用户将光标移到超链接上时那样



```ｃｓｓ
   #div1 {
           overflow: auto; 
           cursor : pointer;
       }
```

### 背景　

- `background-color:#ff99ff;` 设置元素的背景颜色。
- `background-image:url(images/2.gif);` 将图像设置为背景。
- `background-repeat: no-repeat;` 设置背景图片是否重复及如何重复，默认平铺满。（重要）
  - `no-repeat`不要平铺；
  - `repeat-x`横向平铺；
  - `repeat-y`纵向平铺。
- `background-position:center top;` 设置背景图片在当前容器中的位置。
- `background-attachment:scroll;` 设置背景图片是否跟着滚动条一起移动。 属性值可以是：`scroll`（与fixed属性相反，默认属性）、`fixed`（背景就会被固定住，不会被滚动条滚走）。

#### 通栏banner

banner图（网站最上方的全屏大图叫做通栏banner，这种图要求的横向宽度很大，如果直接将其作为img插入网页中，会出现问题 

### css选择器

1. 基本选择器
   1. 标签选择器：针对一类标签
   2. 针对某一个特定标签使用
   3. 针对你想要的所有的标签使用
2. 复合选择器
3. 伪类选择器
4. 伪元素选择器
5. 属性选择器

css布局，本质上都是用<div>来进行布局，在引用中分成标题，区域等等，所以会有语义化的div类的标签来帮助阅读代码。



```html
  body{
        color: white;
        background : rgb(58,58,58) ;
        font-family:Helvetica Neue,Helvetica,sans-serif;
        padding :0;
        margin:0;
}   
header{
        background: black;
        padding:20px;
}
section{
        color:gray;
        background:white;
        padding : 20px;
}
footer{
        background: black;
        padding : 10px 20px;
}
<body>
    <header>header</header>
    <section>triple-section</section>
    <section>lower-section</section>
    <footer>footer</footer>
</body>

```



### id和class选择器

1. id 选择器可以为标有特定 id 的 HTML 元素指定特定的样式。

    **id 选择器以 "#" 来定义。**

    

2. class 选择器用于描述一组元素的样式，class 选择器有别于id选择器，class可以在多个元素中使用。

   **类选择器以一个点"."号显示：**
   
   .one {width: 888px; }
   
3. **js用id ， css用class，一般认为用id的标签会有动态效果**





### 交集选择器

相交的部分就是要设置属性的标签

![image-20210322150616302](D:\Typora\自服务\html.assets\image-20210322150616302.png)

格式：`选择器1.选择器2....`

注意：

1. 选择器间没有连接符号

2. 选择器可以是标签名称，也可以是id，class名称

```
        h3.speical {
            color : red ; 
        }
        h3.express{
            color: green;
        }

    </style>
</head>
<body>
    <h3 class="speical zhongyao">标签1</h3>
    <h3 class=speical>标签2</h3>
    <h3 class="express"> 标签3</h3>
</body>
</html>
```



### 并集选择器

`格式：选择器1,选择器2....`

​	注意： 

1. **选择器间用,连接**
2. 选择器可以是标签名称，也可以是id，class名称

```html
    <style type="text/css">
     p,h2,#express,.one  {
         color: red;
     }

    </style>
</head>
<body>
    <p>Too young too simple</p>
    <h3 class="one">标签1</h3>
    <h2 class=speical>标签2</h2>
    <h3 id="express"> 标签3</h3>
</body>
</html>
```



### 伪元素选择器 

用于某些选择器设置特殊效果

语法：`selecotr :pseudo-element {property:value;}`



#### ::first-line

用于向文本的首行设置特殊样式

```html
 <style>
        ::first-line{
            background-color:rgb(238, 238, 82);
            color:aliceblue;
        }
    </style>

<body>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Nobis excepturi cumque mollitia eveniet eius. </p>
    <div>Lorem ipsum dolor sit amet consectetur adipisicing elit. Praesentium quisquam magni tempore cumque amet expedita quae quia cum dolore</div>
</body>
```

注意：==first-line伪元素只能用于块级元素== ，如a标签是行内元素，无法使用first-line 



lorem 随机生成文字：

#### ::before

在元素的内容后插入新的内容,需要一个属性： content 定义插入的内容，可以是图片，也可以是文本

```html
 <style>
        p::before{
            content:url("images/birds.jpg");
        }
    </style>
<body>
    <p>Lorem ipsum dolor sit s vel sint impedit, officia maiores modi!</p>
```

#### ::after

同before 

**通过这两个属性添加的伪元素是行内元素，需要通过转换成块级元素才能设置**

 

#### ::selection选择器

被用户选中后的样式

```html
 <style>
        p::selection{
            background-color:black;
            color:white;
        }
    </style>
<body>
    <p>Lorem ipsum dolor sit s vel sint impedit, officia maiores modi!</p>
```

 ![img](https://camo.githubusercontent.com/8752082e571ed6d59d05287521051704d8d9ae80a6e194cd4ce6f968b059a22b/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303230375f313530332e706e67) 

### 伪类选择器

同一个标签，根据其不同的状态显示不同样式，用冒号来表示

 

#### 静态伪类

只能用于超链接样式，如下：

`:link 被点击之前`

`:visited 连接被访问过后`



#### 动态伪类

```
:hover 悬停，鼠标放到标签上
:active 激活 鼠标点击标签但没松手
:focus 某个标签获得焦点 
```





#### (a)

a标签的不同状态可以不同的方式显示，包括：活动状态，已被访问状态，未被访问状态，和鼠标悬停状态。



提示：

1. a:hover 必须被置于 a:link 和 a:visited 之后，才有效。
2. a:active 必须被置于 a:hover 之后，才有效。
3. 总结就是 ： l(link)ov(visited)e-and-h(hover)a(active)te
4. 伪类名称对大小写不敏感。

```html
    <style>
        <!--连接未被访问之前-->
        a:link{
            color:pink;
        }
        <!--连接被访问后-->
        a:visited{
            color:red;
        }
        <!--当光标移动到上面的时候-->
        a:hover{
            color:yellow;
        }
        <!--当链接被按下时-->
        a:active{
            color:blue;
        }
        <!--先后顺序：love&&hate-->
    </style>
<body>
    <a href="https://www.w3school.com.cn/css/css_pseudo_classes.asp" alt="链接失效啦~">前往梦的地方</a>
</body>
```

hover对div

```html
    <style>
      div:hover{
        background-color:black;
        color:white;
      }
    </style>
<body>
   <div>Lorem ipsum dolor sit amet consectetur adipisicing elit. Esse beatae ducimus, sequi, ad ipsa maiores</div>
</body>
```

#### 练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style type="text/css">
        div{
            float : left ;
            padding: 8px;
            margin-top:100px;
            background-color: purple;
            给它增加边框
          border-top: 1.5px solid red; 
            border-bottom: 1.5px solid red;
        }
        div:hover{
            background-color: orange;
        }
        a {
            color : rgb(219, 211, 211); 
        }
        a:link{
            text-decoration: none;
            padding:6px;
            color:whitesmoke;
        }
        a:visited{
            color : pink;
        }
        a:active{
            color:orange;
        }
        a:hover{
            color: whitesmoke;
        }
        
      

    </style>
</head>
<body>
    <div><a href="" alt="梦开始的地方" >网站导航</a> </div>
    <div><a href="" alt="梦开始的地方">网站导航</a> </div>
    <div><a href="" alt="梦开始的地方">网站导航</a> </div>
    <div><a href="" alt="梦开始的地方">网站导航</a> </div>
    <div><a href="" alt="梦开始的地方">网站导航</a> </div>
    <div><a href="" alt="梦开始的地方">网站导航</a> </div>
    <div><a href="" alt="梦开始的地方">网站导航</a> </div>
    <div><a href="" alt="梦开始的地方">网站导航</a> </div>
   
</body>
</html>
```

#### 结构伪类选择器

**格式：（第一部分）**

- `E:first-child` 匹配父元素的第一个子元素E。
- `E:last-child` 匹配父元素的最后一个子元素E。
- `E:nth-child(n)` 匹配父元素的第n个子元素E。**注意**，盒子的编号是从`1`开始算起，不是从`0`开始算起。
- `E:nth-child(odd)` 匹配奇数
- `E:nth-child(even)` 匹配偶数
- `E:nth-last-child(n)` 匹配父元素的倒数第n个子元素E。

**理解： 父元素的含义指的是以E元素的父元素为参考** 

以上选择器中所选到的元素的类型，必须指定是类型E，如果选不中则无效





 https://github.com/qianguyihao/Web/blob/master/02-CSS%E5%9F%BA%E7%A1%80/10-CSS3%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%A6%E8%A7%A3.md![img](https://camo.githubusercontent.com/78a25af08d15402694a49f6a4fc10761a0f44d9d6699b6bc2e6654e6444922c7/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303230375f313530322e706e67)

### 选择器定位详解

定位到某个标签内的属性 

属性选择器： `[]` 

匹配含义：

1.  `^` ：开头 
2. `$` : 结尾
3. `*` : 包含

格式：

1. E[title]选中页面的E元素 ，并且存在title 属性即可 
2. E[title="abc"] 选中页面E元素，并且E需要带有title 属性，且属性值完全等于abc 
3. E[attr~=val] 选则具有att属性且属性值 用空格分隔的列表，其中一个等于val的E元素



 ![img](https://camo.githubusercontent.com/7f8ea4b665b7d0afcc8b93afa93bdd3304ffcd047ead6af8c2be99105bf84538/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303230375f313530302e706e67) 



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style type="text/css">
        body{
            margin:0px;
            padding:0px;
            background-color:#f7f7f7;
        }
        .wrapper{
            width:1024px;
            margin:0 auto;
        }
        .wrapper > header{
            line-height: 2; 
            text-align: center;
        }
        .wrapper >section {
            min-height: 300px;
            margin-bottom: 30px;
            box-shadow: 1px 1px 4px #DDD;
            background-color: #FFF;
        }
        .wrapper section > header{
            text-align: center; 
            line-height: 1 ;
            padding: 10px;
            font-size: 22px;
            color: #333;
            background-color: #EEE;
        }
        .wrapper .wrap-box {
            padding: 20px;
        }
        form {
            width: 300px;
            height: 300px;
            /* 让内容水平居中 */
            margin : 0 auto;  
        }
        ul{
            list-style: none;
        }
        form input [type="text"] {
            width: 200px;
            height: 30px;
        }
         form input [type="passwrod"] {
            width: 200px;
            height: 30px;
        }

        a{
            color: rgb(0, 0, 0);
        }
        .attr1 a[class~="haha"] 
        {
            color: green ;
        }
        .attr2 a[class$="heihei"] {
            color:rgb(243, 187, 113);
        }
        .attr3 a[class|="xixixi"] {
            color:rgb(194, 2, 178);
        }
        .attr4 a[class*=whk]{
            color:rgb(57, 152, 216);
        }

        .attr5 a[class^="whr"] {
            color: rgb(151, 149, 149);
        }

        .attr6 a[class="download"] {
            color:cadetblue;
        }
        p::selection{
            background-color: #333;
            color: wheat;
        }
    </style>
</head>
<body>
    <p>asjdflajdsifajfelanfjenagiojblnf</p>
    <div class="wrapper">
        <header>CSS3 属性选择器</header>
        <section>
            <header>简介</header>
       
        <div class="wrap-box">
            <form action="">
                <ul>
                    <li>
                        姓名：<input type="text"> 
                    </li>
                    <li>
                        密码：<input type="password">
                    </li>
                    <li>
                        性别：<input type="radio"> 男
                                <input type="radio" >女
                    </li>
                    <li>
                        兴趣：<input type="checkbox" name= "" id=""> 写代码  
                    </li>
                    <li>
                        提交：<input type="submit" value="提交">
                    </li>
                </ul>
            </form>
        </div>
    </div>
    </section>

    <section class="attr1">
        <header>E[attr~=val] 表示有空格隔开的单词列表中有val值就匹配</header>
        <div class="wrap-box">
            <a href="../images/dog.png" class="download haha">我要变色</a>
            <a href="../images/dog.png" class="downloadhaha">点我下载</a>
        </div>
    </section>

    <section class="attr2">
        <header>E[attr$=val]以val值结尾的</header>
        <div class="wrap-box">
            <a href="../images/少女.png" class="downloadheihei">点我下载</a>
        </div>
    </section>

    <section class="attr3">
        <header>E[attr|=val] 要么是单个val值，要么是以-分割</header>
        <div class="wrap-box">
            <a href="../images/北极熊.png" class="download-xixixi">点我下载</a>
        </div>
    </section>

    <section class="attr4">
        <header>E[attr*=val] 其中包含任何位置有val的</header>
        <div class="wrap-box">
            <a href="../images/dog6.png" class="dowhkwnload">点我下载</a>
        </div>
    </section>

    <section class="attr5">
        <header>E[attr^=val] 开头有val的</header>
        <div class="wrap-box">
            <a href="../images/s.png" class="whrdownload">点我下载</a>
        </div>
    </section>

    <section class="attr6">
        <header>E[attr=val] </header>
        <div class="wrap-box">
            <a href="../images/banner20200827.jpg" class="download">点我下载</a>
        </div>
    </section>
</body>
</html>
```



### 继承性

关于css的继承性：
1. 关于文字样式的属性,都有继承性，这些属性包括：color,text-xxx,line-xxx,font-xxx
2. 关于盒子,定位,布局的属性,都不能继承

### 层叠性

**计算权重**

**就是css处理冲突的能力**，所有的权重计算，没有任何兼容问题 



```html
    <style type="text/css">
        p{
            color:red ;
        }
        .haha {
            color: green ;
        }
        #heihei{
            color:blue;
        }
      

    </style>
</head>
<body>
    
   <div>
       <p class="has" id="heihei">猜猜我是什么颜色</p>
   </div>
```



![image-20210322160831058](D:\Typora\自服务\html.assets\image-20210322160831058.png)

 **id的数量，类的数量，标签的数量** 

因为对于相同方式的样式表，其选择器排序的优先级为：

**ID选择器 > 类选择器 > 标签选择器**



#### 懵圈例子

```html
    <style type="text/css">
    /*  # 1个id 1个类选择器 .    一个标签 111   */
        #box1 .hh2 p {
            color : red ;
        }
        /*  1个id  0 个类 3个标签  103 */
        div div #box3 p{
            color: green ;
        }
        /* 0个id 3个类 4个标签  034 */
        div.hh1 div.hh2 div.hh3 p{
            color:pink ;
        }

    </style>
</head>
<body>
    
   <div class="hh1" id="box1">
       <div class="hh2" id ="box2">
            <div class="hh3" id="box3">
                <p>猜猜我是什么颜色</p>
            </div>
       </div>
   </div>
```

#### 实战例子

```html
 <style type="text/css">
        .box ul li {
            color:pink;
        } // 0 1 2 
        .box ul li.test {
            color : red;
        } // 1 1 1 
    </style>
</head>
<body>
    
    <div class = "box">
        <ul>
            <li class="tesu">哈哈</li>
            <li>哈哈</li>
            <li>哈哈</li>
            <li>哈哈</li>
            <li>哈哈</li>
            <li>哈哈</li>
            <li>哈哈</li>
        </ul>
```

https://www.cnblogs.com/qianguyihao/p/7253929.html 

没懂



`section div` 表示在section内部的div（有嵌套的） 

`section ,a `表示要同时对section和a （独立的）

选择器类名

解决不同容器有不同的颜色，给容器加类名可以让css精准定位

```css
 <section class="feature-box sales">
        	  <div>Section c</div>
</section>
<section class="feature-box closeout">
            <div>Section e</div>
</section> 
.feature-box{
         display :flex;
        }
        .feature-box div{
            margin:10px;
            width:100%;
        }
        .feature-box.sales div{
            background:grey;
        }
        .feature-box.closeout div{
            background:pink;    
        }
1. 可以省略选择器名称 ，直接.类名
2. 相同的部分如 padding， margin， 另外开一个，代码复用
```



## 盒子模型

 在 CSS 中我们广泛地使用两种“盒子” —— **块级盒子 (block box) 和 内联盒子 (inline box)**。这两种盒子会在页面流（page flow）和元素之间的关系方面表现出不同的行为 

**盒子的属性；**

1. width，height，padding，border，margin

2. width和height ： 内容的宽度，高度（不是盒子的宽度和高度）
3. padding : 内边距  （内容到border的距离） 
4. border： 边框
5. margin： 外边距

![image-20210323193858962](D:\Typora\自服务\html.assets\image-20210323193858962.png)





 ![img](https://camo.githubusercontent.com/9354ddb1b4fd57a640f7499c8ab225e112fa85b923bfd2c56e35e69b076e2a45/687474703a2f2f696d672e736d79687661652e636f6d2f323031352d31302d30332d6373732d32372e6a7067) 

真实占有宽度 = 左border + 左padding + width + 右padding + 右border



**如果想保持一个盒子的真实占有宽度不变，那么加width的时候就要减padding。加padding的时候就要减width**。因为盒子变胖了是灾难性的，这会把别的盒子挤下去。



 

### 块级block盒子 

1. 盒子在内联方向扩展并占据父容器在该方向上的所有可用空间。
2. 每一个盒子都会换行
3. **width和height属性可以发挥作用**
4.  内边距（padding）, 外边距（margin） 和 边框（border） 会将其他元素从当前盒子周围“推开” 
5. 如  div、hr、p、h1~h6、ul、ol、dl、form、table

### 内敛inline盒子

1. 不会换行
2. **width和height不起作用**
3.  垂直方向的内边距、外边距以及边框会被应用但是不会把其他处于 `inline` 状态的盒子推开。 
4.  水平方向的内边距、外边距以及边框会被应用而且也会把其他处于 `inline` 状态的盒子推开。
5. ​     **span、a、i、em**   



### 相互转换

通过display属性将块级元素和行内元素进行转换

`display : inline ; // 块级转行内`

`display: block ; // 行内变块级 `

行内边转块级才可以设置它的width和height

### 脱离标准流

> 标准流里面的限制非常多，导致很多页面效果无法实现。如果我们现在就要并排、并且就要设置宽高，那该怎么办呢？办法是：移民！**脱离标准流**！

css中3中方法是一个元素脱离标准文档流：

1. 浮动
2. 绝对定位
3. 固定定位

### 浮动

几个性质

1. **脱离标准流（可以并排也可以设置行高了，无论原来是span还是div 浮动之后就不区分行内和块级了**
2. **浮动元素互相贴靠**
3. **字围效果： 即标准流的文字不会被浮动的盒子遮挡住**
4. **一个浮动元素如果没有设置width，那么将自动收缩为内容宽度**



关于浮动我们要强调一点，浮动这个东西，为避免混乱，我们在初期一定要遵循一个原则：**永远不是一个东西单独浮动，浮动都是一起浮动，要浮动，大家都浮动。**

### 清除浮动

> 这里所说的清除浮动，指的是清除浮动与浮动之间的影响。
>
> 下面介绍几个方法

#### 1.祖先增加元素

1. **给浮动元素的祖先元素增加高度**（一个元素要浮动 其祖先元素一定要有高度 只有在一个有高度的盒子中，这个浮动才不会影响到后面的浮动元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
        li {
            float: left;
            width: 150px;
            height: 40px;
            background-color: pink;
        }
        div{
            height: 50px;
        }
    </style>
</head>
<body>
    <div class="box1">
        <ul>
            <li>生命壹号1</li>
            <li>生命壹号2</li>
            <li>生命壹号3</li>
            <li>生命壹号4</li>
        </ul>
    </div>
    <div class="box2">
        <ul>
            <li>许嵩1</li>
            <li>许嵩2</li>
            <li>许嵩3</li>
            <li>许嵩4</li>
        </ul>
    </div>
</body>
</html>
```

**可以试试去掉div的高度，发现会粘在一起**

#### clear:both

1. clear就是清除，both就是左右都要清楚，不允许左侧和右侧有浮动对象



#### 隔墙法

 为了防止第2个div贴到第一个div，可以在两个div中间用一个新的div隔开，给新的div设置clear：both 然后设置height达到margin的效果 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
        li {
            float: left;
            width: 150px;
            height: 40px;
            background-color: pink;
        }
        
        .clear{
            clear:both;
            height: 10px;
            background-color:aqua
        }
       
    </style>
</head>
<body>
    <div class="box1">
        <ul>
            <li>生命壹号1</li>
            <li>生命壹号2</li>
            <li>生命壹号3</li>
            <li>生命壹号4</li>
        </ul>
    </div>
    <div class="clear h2"></div>
    <div class="box2">
        <ul>
            <li>许嵩1</li>
            <li>许嵩2</li>
            <li>许嵩3</li>
            <li>许嵩4</li>
        </ul>
    </div>
</body>
</html>
```

#### 内墙法

**一句话:一个父亲是不能被浮动的儿子撑出高度的**

```html
    <style type="text/css">
        div{
            background-color: aqua;
        }
        p{
            /* 1.当p设置为浮动之后，发现div没有了高度 */
            float:left;
            width:100px;
            height:100px;
            background-color: royalblue;
        }
        .cl{
            clear:both;
        }
    </style>
</head>
<body>
    <!-- 0.在一个div内放一个p -->
    <div class="box1">
        <p></p>
        <!-- 2.在div中再建一墙 div作为内墙解决问题 -->
         <div class="cl"></div>
    </div>
```

#### overflow:hidden

 本意是清除溢出到盒子外面的文字。但是，前端开发工程师发现了，它能做偏方。如下：

一个父亲不能被自己浮动的儿子，撑出高度。但是，只要给父亲加上`overflow:hidden`; 那么，父亲就能被儿子撑出高了。这是一个偏方。

#### margin相关

**margin塌陷/margin重叠**

**标准文档流中，竖直方向的margin不叠加，取**较大的值**作为margin(水平方向的margin是可以叠加的，即水平方向没有塌陷现象)。如下图所示

如果不在标准流，则没有塌陷现象

盒子居中`margin:0 auto;`

- 只有标准流的盒子，才能使用`margin:0 auto;`居中。也就是说，当一个盒子浮动了、绝对定位了、固定定位了，都不能使用margin:0 auto;
- 使用`margin:0 auto;`的盒子，必须有width，有明确的width。（可以这样理解，如果没有明确的witdh，那么它的witdh就是霸占整行，没有意义）
- `margin:0 auto;`是让盒子居中，不是让盒子里的文本居中。文本的居中，要使用`text-align:center;`



**善于使用父亲的padding，而不是儿子的margin**

 ![img](http://img.smyhvae.com/20170806_1537.png) 

如果父亲没有border，那么儿子的margin实际上踹的是“流”，踹的是这“行”。所以，父亲整体也掉下来了。

**margin这个属性，本质上描述的是兄弟和兄弟之间的距离； 最好不要用这个marign表达父子之间的距离。**

所以，如果要表达父子之间的距离，我们一定要善于使用父亲的padding，而不是儿子的margin。

**左右用margin ，上下用padding**



### padding

padding属性设置一个元素的内边距，padding 区域指一个元素的内容和其边界之间的空间，该属性不能为负值。

```
padding : 30px  20px 40px 100px ; 
// 对应 上右下左 顺时针方向 用空格隔开 

// 如果写了3个值: 则为上 右 下 

如果写了2个值： 
padding : 30px  40px ;
即为 ： 30px  40px  30px  40px ; 

```

**自带padding的元素**

如ul标签自带40px的padding 的元素

做网页时为了方便控制总清楚这个默认的padding 



**padding会影响元素的尺寸（通常情况下是通过增加/挤压内容区域）**

### margin

为给定元素设置所有四个（）方向的外边距属性。 

```css
margin: style                  /*单值语法  所有边缘 */  举例： margin: 1em; 
margin: vertical horizontal    /*二值语法  纵向 横向 */  举例： margin: 5% auto; 
margin: top horizontal bottom  /*三值语法 上 横向 下*/  举例： margin: 1em auto 2em; 
margin: top right bottom left  /*四值语法 上 右 下 左*/  举例： margin: 2px 1em 0 auto; 


```



### border

要素： 粗细，线型，颜色

挺多东西的-就不抄了

https://github.com/qianguyihao/Web/blob/master/02-CSS%E5%9F%BA%E7%A1%80/06-CSS%E7%9B%92%E6%A8%A1%E5%9E%8B%E8%AF%A6%E8%A7%A3.md



### 定位

> 元素其实是使用 top、bottom、left 和 right 属性定位的。但是，除非首先设置了 position 属性，否则这些属性将不起作用。根据不同的 position 值，它们的工作方式也不同。

CSS的定位属性

有三种，分别是绝对定位、相对定位、固定定位。

```css
	position: absolute;  <!-- 绝对定位 -->

	position: relative;  <!-- 相对定位 -->

	position: fixed;     <!-- 固定定位 -->
```

#### 相对定位

 让元素相对于自己原来的位置，进行位置调整（可用于盒子的位置微调）。 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
       *{
           margin:0px;
          
       }
        .div1{
           width:200px;
           height:200px;
            border  : 2px solid red ;
            background-color: aquamarine;
        }
        .div2{
            position: relative;  /*   相对自己原来位置 */
            /* 横坐标： 正值表示右移， 负值表示左移 */
             left:50px;
             /* 正值表示下移  负值表示上移  */
            top:50px;

            width:100px ;
            height: 100px;
            background-color: pink;
            margin-top:50px;
             border  : 2px solid red ; 
        }

    </style>
</head>
<body>
   <div class="div1">有生之年</div>
    <div class="div2">狭路相逢</div>
</body>
</html>
```

**相对定位不脱标**

**相对定位**：不脱标，老家留坑，**别人不会把它的位置挤走**。

也就是说，相对定位的真实位置还在老家，只不过影子出去了，可以到处飘。

**相对定位的用途**

如果想做“压盖”效果（把一个div放到另一个div之上），我们一般**不用**相对定位来做。相对定位，就两个作用：

- （1）微调元素
- （2）做绝对定位的参考，子绝父相

##### 压盖案例

```html 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
       *{
           margin:0px;
        }
       
        .box{
            position: relative ; /*父相*/
            margin:100px; 
            width:308px;
            height: 307px;
            border :1px  solid #FF7E00 ;
        }
        /* 内敛盒子 */
        .box .dtc{
            /* 转成块级才能设置高度 */
            display : block ;  
            background-image : url(http://img.smyhvae.com/20180116_1115.png);
            position : absolute;
            width : 52px;
            height: 28px;
            top: -10px;
            left : 13px;
        }
        .box .image  img{
            width:308px;
            height  :196px;
        }
        .box h4{
            background-color: black ;
            color: white ;
            width: 308px;
            height: 40px;
            line-height: 40px;
            position: absolute;
            top:156px;
            font-size: 15px;
        }

    </style>
</head>
<body>
    <div class="box">
        <span class="dtc"></span>
        <div class="image">
            <img src="../images/English.jpg">
        </div>
        <h4>不要迷茫了下定决心抓住良机吧</h4>
    </div>
</body>
</html>
```



#### 绝对定位

定义横纵坐标，原点在父容器的左上角或者左下角

**绝对定位脱标：脱离了标准文档流**

绝对定位后标签之后不区分行内元素，块级元素，不需要display:block就可以设置宽，高

- 元素以block显示 

#### 高度欺骗

absolute出现层级关系，父元素看不见设置为absoulte 的子元素





**绝对定位的参考点**

1. 如果只指定了absoulte而未指定left/right/top/bottom ，其在所处的层级中的定位点就是正常文档流中该元素的定位点。 

2. 让absolute元素没有position:static以外的父元素。此时absolute所处的层是铺满全屏的，即铺满body。会根据用户指定位置的在body上进行定位。

   只指定left时，元素的左上角定位点的left值会变成用户指定值。但top值仍旧是该元素在正常文档流中的top值：

3. 



1. 如果用top描述，参考点是页面的左上角，而不是浏览器的左上角
2. 如果用bottom描述，那么参考点就是浏览器的首屏窗口尺寸 对应的页面左下角



以盒子作为参考点

> 一个绝对定位的元素，如果父辈元素（根据最近上一级已经定位的元素）中也出现了已定位（无论是绝对定位、相对定位，还是固定定位）的元素，那么将以父辈这个元素，为参考点。

 ![img](https://camo.githubusercontent.com/028908dcca2ed6a500d5204a0ee951e8d0ba84277bd4dd556c9fa8cfad2ac738/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303131355f323231302e706e67) 

上图： 子绝夫相 

> 子绝父绝、**子绝父相**、子绝父固，都是可以给儿子定位的。但是在工程上，如果子绝、父绝，没有一个盒子在标准流里面了，所以页面就不稳固，没有任何实战用途。



#### 子绝父相： 

子级绝对定位，不会占有位置，可以放到父盒子的任何一个地方





##### 让盒子居中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
    /* 脱标(absolute)元素 1.让左边线居中 left：50%  2.margin-left：负的宽度的一半   公式*/
        div{
            left: 50%;
            height: 100px;
            width:600px;
            position: absolute;
            top:0;
            background-color: aquamarine;
            margin-left: -300px;
        }
        /* 在标准流之中设置 margin：0 auto */
        /* div{
            width :600px;
            height: 100px;
            background-color: aquamarine;
            margin: 0 auto;
        } */
    </style>
</head>
<body>
    <div>

    </div>
</body>
</html>
```



#### 固定定位

**固定定位**：就是相对浏览器窗口进行定位。无论页面如何滚动，这个盒子显示的位置不变。

跟父元素没有任何关系

不随滚动条滚动

不占有原先位置 （脱标，

**用途1**：网页右下角的“返回到顶部”

比如我们经常看到的网页右下角显示的“返回到顶部”，就可以固定定位。 



### z-index

表示谁压着谁，数值大的盖着数值小的

有如下特性：

1. 属性值大的位于上层，属性值小的位于下层
2. z-index值没有单位，就是一个正整数。默认的z-index值是0。
3. 如果大家都没有z-index值，或者z-index值一样，那么在HTML代码里写在后面，谁就在上面能压住别人。定位了的元素，永远能够压住没有定位的元素。
4. 只有定位了的元素，才能有z-index值。也就是说，不管相对定位、绝对定位、固定定位，都可以使用z-index值。**而浮动的元素不能用**。
5. 从父现象：父亲怂了，儿子再牛逼也没用。意思是，如果父亲1比父亲2大，那么，即使儿子1比儿子2小，儿子1也能在最上层。

**z-index属性的应用还是很广泛的。当好几个已定位的标签出现覆盖的现象时，我们可以用这个z-index属性决定，谁处于最上方。也就是层级的应用。**



### 定位的扩展





### 博雅互动

![image-20210414145248772](D:\Typora\自服务\html.assets\image-20210414145248772.png)

# css动画



在css中使用动画效果可以通过设置多个节点来精确控制一个或者一组动画，常用来实现复杂的动画效果

关键帧:`@keyframes` 规则通过在动画序列中定义关键帧的样式来控制CSS动画

动画定义的步骤：

1. 通过@keyframes定义动画
2. 将这段动画通过百分比分割成多个节点，然后各个节点中分别定义各属性
3. 在指定元素中，通过animation属性调用动画

```
    定义动画：
        @keyframes 动画名{
            from{ 初始状态 }
            to{ 结束状态 }
        }

     调用：
      animation: 动画名称 持续时间；
```

其中animation属性的格式如下：

```
animation：定义的动画名称，持续时间，执行次数，是否反向，运动曲线，延迟执行 （infinite表示无限次）
```

1. animation-name ： 为@keyframes 动画规定的一个名称 

2. animation-duration：4s 持续时间

上面两个属性为必须项，且顺序固定

3. animation-iteration-count : 1 执行次数

4. animation-direction：alternate 动画方向

   alternate反向， normal正向

5. animation-timing-function：ease-in ;

   属性值可以是： linear ,ease-in-out ,steps()

   如果是steps() 则表示动画不是连续执行，而间断地分成几步执行





### 示例1 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Css动画</title>
    <style>
        .box{
            width:100px;
            height:100px;
            margin:100px;
            background-color:cornflowerblue;
            /* animation: move 3s alternate linear 3 ; */
            animation: round 4s alternate linear infinite ;
        }
        @keyframes move{
            from{
                transform:translateX(0px) rotate(0deg) ;
            }
            to{
                transform:translateX(500px) rotate(360deg);
            }
        }
        @keyframes round{
            0% {
                transform: translateX(0px) translateY(0px) ;
                color: cornflowerblue;
            }
            25%{
                transform:  translateX(500px) translateY(0px);
                color: chartreuse;  
            }
            50%{
                /*  因为X轴的500px是相对最开始的原点来说的。可以理解成此时的 translateX 是保存了之前的位移 */
                transform: translateX(500px) translateY(200px);
                background-color: coral;
                border-radius: 50%;
            }
            75%{
                transform: translateX(0px) translateY(200px);
            }
            100%{
                transform: translateX(0px) translateY(0px);
                background-color: cornflowerblue;
                 border-radius: 0%;
            }
        }
    </style>
</head>
<body>
    <div class="box">
    </div>
</body>
</html>
```

###  案例2 step()

钟 我们通过一个黑色的长条div，旋转360度，耗时60s，分成60步完成。即可实现。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Css动画</title>
    <style>
        div{
            width: 3px;
            height:200px;
            background-color: #000; 
            margin: 100px auto; 
            transform-origin: center bottom;/* 旋转的中心点是底部 */
            animation: myClock 60s steps(60) infinite ;
        }
        @keyframes myClock{
            0%{
                transform: rotate(0deg) ;
            }
            100%{
                transform: rotate(360deg);
            }
        }
    </style>
</head>
<body>
    <div class="box">
    </div>
</body>
</html>
```



## 过渡

transition 实现不同状态间的平滑过渡（补间动画）

- 补间动画：自动完成从起始状态到终止状态的的过渡。不用管中间的状态。
- 帧动画：通过一帧一帧的画面按照固定顺序和速度播放。如电影胶片

**transition 包括以下属性：**

- `transition-property: all;` 如果希望所有的属性都发生过渡，就使用all。
- `transition-duration: 1s;` 过渡的持续时间。
- `transition-timing-function: linear;` 运动曲线。属性值可以是：
  - `linear` 线性
  - `ease` 减速
  - `ease-in` 加速
  - `ease-out` 减速
  - `ease-in-out` 先加速后减速
- `transition-delay: 1s;` 过渡延迟。多长时间后再执行这个过渡动画。

上面的四个属性也可以写成**综合属性**：

```
	transition: 让哪些属性进行过度 过渡的持续时间 运动曲线 延迟时间;

	transition: all 3s linear 0s;
```

#### 过渡案例



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Css动画</title>
    <style>
       body{
           margin: 0 px;
           padding: 0px;
           background-color: #eeeeee;
       }
       .content{
           width: 800px;
           height:320px;
           padding-left: 30px;
           margin : 80px auto;
       }
       .content .play{
            width:230px;
            height: 300px;
            text-align: center; 
            margin-right: 20px;
            background-color: #fff;
            position: relative;
            top:0;
            float: left;
            overflow: hidden;
            transition: all .5s; 
       }
       .play img{
           margin-top:30px;
       }
       .play .desc{
           position: absolute;
           left:0px;
           bottom: -80px;
           width: 100%;
           height: 80px;
           background-color: #aaaaaa;
           transition: all .5s;
       }

       .play:hover {
            top: -5px;
            box-shadow: 0 0 15px #aaa;
       }
       .play:hover .desc{
           bottom: 0;
       }
    </style>
</head>
<body>
    <div class="content"> 
        <div class="play">
            <img src="../image_play/天空.jpg" alt="">
        </div>
        <div class="play">
            <img src="../image_play/月亮.jpg" alt="">
            <span class="desc"></span>
        </div>
        <div class="play">
            <img src="../image_play/街道.jpg" alt="">
            <span class="desc"></span>
        </div>
    </div>
</body>
</html>
```

## 2D转换

**转换**是 CSS3 中具有颠覆性的一个特征，可以实现元素的**位移、旋转、变形、缩放**，甚至支持矩阵方式。

在 CSS3 当中，通过 `transform` 转换来实现 2D 转换或者 3D 转换。

- 2D转换包括：缩放、移动、旋转。

### 缩放scale

格式：` transofrm : scale(x,y);` 

x表示水平方向的缩放倍数，y表示垂直方向的缩放倍数，只写一个值就是等比例缩放

取值： 大于1就是放大，小于1是缩小，不能为百分比

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Css动画</title>
    <style>
     body{
            margin: 0;
            padding: 0;
            background-color: #eeeeee; 
        }
        .content{
            width: 800px ;
           
            margin: 100px auto ;
        }
        .content div  {
            background-color:rgb(243, 183, 104);
            float: left;
            width: 230px;
            height: 250px;
            margin-right: 20px;
            color:#eeeeee;
            text-align: center;
            font : 400 30px/150px ;
        }
        .content .box2 {
            background-color: green;
            transition: all 1s;
        }
        .content .box2:hover{
            background-color: yellowgreen;
            transform: scale(2,0.5); 
        }
     
    </style>
</head>
<body>
    <div class="content"> 
       <div class="box1"></div>
       <div class="box2"></div>
       <div class="box3"></div>
    </div>
</body>
</html>
```

box2变化不会将其兄弟挤走



### 位移translate 

```
transform: translate(x，y);
transform: translate(-50%, -50%);
```

参数为百分比，相对于自身移动

x 出现正值时，表示水平方向向右移动，负值表示向左移动

y正值： 向下移动， 负值：向上移动



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Css动画</title>
    <style>
     body{
            margin: 0;
            padding: 0;
            background-color: #eeeeee; 
        }
        .content{
            width: 800px ;
           
            margin: 100px auto ;
        }
        .content > div  {
            background-color:rgb(243, 183, 104);
            float: left;
            width: 230px;
            height: 250px;
            margin-right: 20px;
            color:#eeeeee;
            text-align: center;
            font : 400 30px/150px ;
        }
        div:nth-child(2){
            background-color: green;
            transition: all 1s;
        }
        div:nth-child(2):hover{
            transform: translate(-50%,-50%);
        }
    </style>
</head>
<body>
    <div class="content"> 
       <div class="box1"></div>
       <div class="box2"></div>
       <div class="box3"></div>
    </div>
</body>
</html>
```

**应用：让绝对定位中的盒子在父亲中居中**

如果一个标准流的盒子在父亲里水平居中，可以设置其margin属性：`margin ： 0 auto `

如果盒子是绝对定位的，此时已经脱标了，如果还想让其居中，可以 

```css 
.content{
            background-color: rgb(231, 140, 20);
            width: 600px ;
            height: 60px;
            position: absolute;
            left: 50%; /*首先这里的50%指的是左边线位于中间的位置*/
            top: 0;
            margin-left: -300px;  /*然后向左移动宽度（600）的一半*/
        }
      
```

还可以用偏移translate来做

```css
  div{
           width: 600px;
           height: 60px;
           background-color: aquamarine;
           position: absolute;
           /* 中线50% */
           left: 50%;
           top:0;
           /* 往左走自己宽度的一半 */
           transform: translate(-50%); 
       }
```

### 旋转rotate

transform：rotate（角度）

transform: rotate(45deg)  正值顺时针，负值逆时针



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Css动画</title>
    <style>
     body{
            margin: 0;
            padding: 0;
            background-color: #eeeeee; 
        }
        .content{
            background-color: rgb(231, 140, 20);
            width: 300px ;
            height: 200px;
            position: absolute;
            left: 50%;
            margin-top: 30px;
            margin-left: -300px;
            transition: all 2s; 
        }
        .content:hover{
            transform: rotate(-405deg);
        }
    </style>
</head>
<body>
    <div class="content"> </div>
</body>
</html>
```



### 案例：小火箭

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Css动画</title>
    <style>
        html, body{
         height: 100%;
        }
        body{
            background-color: #DE8910;
        }
        .rocket{
            position: absolute;
            left: 100px;
            top:600px;
            height: 120px;
            /* 开始： 向左向下移动 顺时针45度 */
            transform: translate(-200px,200px) rotate(45deg);
            transition: all 1s ease-in; 
        }
        body:hover .rocket{
            /* 悬浮时：向右向上移动，顺时针45 */
            transform:translate(500px,-500px) rotate(45deg);
        }
    </style>
</head>
<body>
   <img class="rocket" src="../image_play/20180208-rocket.png">
</body>
</html>
```

## 3d转换

旋转 ： rotateX ， rotateY，rotateZ 

3D坐标系左手坐标系

 ![img](https://camo.githubusercontent.com/6302cc87daea5f17e7a16a5775f654de3738231ec3c3aa6cd7793beb5812a8da/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303230385f323031302e706e67) 





### 百度钱包

让百度钱包水平翻转的效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Css动画</title>
    <style>
       body{
           background-color: cornflowerblue;
       }
       .box{
           width: 300px;
           height: 300px;
           margin: 50px auto;
           position: relative;
           border-radius: 50%;
           background-color: pink;
       }
       .box > div{
           width: 100%;
           height: 100%;
           position: absolute;
           border-radius: 50%;
           transition: all 2s ;
           backface-visibility: hidden;
       }
       .box1{
           background: url(../images/百度钱包.png) left 0 no-repeat;
       }
       .box2{
           background: url(../images/百度钱包.png) right 0 no-repeat;
           /*让图片的右半边默认时，旋转180度，就可以暂时隐藏起来*/
           transform: rotateY(180deg); 
       }

       .box:hover .box1{ 
           /* 右半部分消失 */
            transform: rotateY(180deg); 
       }
       .box:hover .box2{
           /* 左半部分转出现 */
           transform: rotate(0deg); 
       }
    </style>
</head>
<body>
    <div class="box">
        <div class="box1"></div>
        <div class="box2"></div>
    </div>
</body>
</html>
```





### 透视perspective

电脑显示屏是一个2d平面，图像之所以有立体感，其实只是一种视觉呈现，通过透视可以实现这个目的

格式：

作为一个属性，设置给其父元素，作用域所有3d转换的子元素

作为transform属性的一个值，作用域元素自身

`perspective:500px;`

### translateZ案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Css动画</title>
    <style>
       body{
           background-color: cornflowerblue;
       }
       .box{
           width: 300px;
           height: 300px;
           margin: 50px auto;
           position: relative;
           border-radius: 50%;
           background-color: pink;
       }
       .box > div{
           width: 100%;
           height: 100%;
           position: absolute;
           border-radius: 50%;
           transition: all 2s ;
           backface-visibility: hidden;
       }
       .box1{
           background: url(../images/百度钱包.png) left 0 no-repeat;
       }
       .box2{
           background: url(../images/百度钱包.png) right 0 no-repeat;
           /*让图片的右半边默认时，旋转180度，就可以暂时隐藏起来*/
           transform: rotateY(180deg); 
       }

       .box:hover .box1{ 
           /* 右半部分消失 */
            transform: rotateY(180deg); 
       }
       .box:hover .box2{
           /* 左半部分转出现 */
           transform: rotate(0deg); 
       }
    </style>
</head>
<body>
    <div class="box">
        <div class="box1"></div>
        <div class="box2"></div>
    </div>
</body>
</html>
```

### 3d呈现

（transfrom-style）

3D元素构建是指某个图形是由多个元素构成的，可以给这些元素的**父元素**设置`transform-style: preserve-3d`来使其变成一个真正的3D图形。属性值可以如下：

```css
transfrom-style:preserve-3d;/*让子盒子位于三维空间内*/
transfrom-style:flat; /*让子盒子位于此元素所在的平面内子盒子被扁平化*/
```



#### 案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Css动画</title>
    <style>
        .box{
            width: 250px;
            height: 250px;
            border: 1px dashed red;
            margin:100px  auto ;
            position: relative;
            border-radius: 50%;
            transform-style: preserve-3d;
            animation: gun 8s linear infinite ;
        }
        .box >div {
            width: 100%;
            height: 100%;
            position: absolute; 
            text-align: center ;
            line-height: 250px;
            font-size: 60px;
            color: #daa520;
        }
        .left{
            background-color:  rgba(255, 0, 0, 0.3);
            transform-origin:left;
            transform: rotateY(90deg) translate(-125px);
        }

         .right{
            background-color:  rgba(0, 0, 255, 0.3);
            transform-origin:right;
            transform: rotateY(90deg) translate(125px);
        }
          .forward {
            background: rgba(255, 255, 0, 0.3);
            transform: translateZ(125px);
        }
           .back {
            background: rgba(0, 255, 255, 0.3);
            transform: translateZ(-125px);
        }

         .up {
            background: rgba(255, 0, 255, 0.3);
            transform: rotateX(90deg) translateZ(125px);
        }

        .down {
            background: rgba(99, 66, 33, 0.3);
            transform: rotateX(-90deg) translateZ(125px);
        }
        @keyframes gun{
            0% {
                transform: rotateX(0deg) rotateY(0deg) ;
            }
            100% {
                transform: rotateX(360deg) rotateY(360deg);
            }
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="up">南</div>
        <div class="dowm">北</div>
        <div class="left">西</div>
        <div class="right">动</div>
        <div class="forward">中</div>
        <div class="back">后</div>
    </div>
</body>
</html>
```















