# JS

引入jquery

<script src="http://code.jquery.com/jquery-latest.js"></script>
### js输出

用 window.alert() 弹出警告栏

用document.wirte()方法将内容写到html文档中

innerHTML写入到html元素

### 进阶前言

- **DOM**：文档对象模型（Document object Model），操作**网页上的元素**的API。比如让盒子移动、变色、轮播图等。
- **BOM**：浏览器对象模型（Browser Object Model），操作**浏览器部分功能**的API。比如让浏览器自动滚动。

- **节点**（Node）：构成 HTML 网页的最基本单元。网页中的每一个部分都可以称为是一个节点，比如：html标签、属性、文本、注释、整个文档等都是一个节点。



### DOM

DOM 为文档提供了结构化表示，并定义了如何通过脚本来访问文档结构。目的其实就是为了能让js操作html元素而制定的一个规范。

DOM就是由节点组成的。

**解析过程**： HTML加载完毕，渲染引擎会在内存中把HTML文档，生成一个DOM树，getElementById是获取内中DOM上的元素节点。然后操作的时候修改的是该元素的**属性**。

**DOM树**：（一切都是节点）

 ![img](https://camo.githubusercontent.com/135c3f3257c19175bba869b78d3b02cace51fc97b2404b30ded4ff9a9dbe0f6a/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303132365f323130352e706e67) 



**DOM可以做什么**

- 找对象（元素节点）
- 设置元素的属性值
- 设置元素的样式
- 动态创建和删除元素
- 事件的触发响应：事件源、事件、事件的驱动程序

![](D:\Typora\前端JS.assets\操作元素.png)

**获取元素节点**

 DOM节点的获取方式其实就是**获取事件源的方式**。 

3种方式： 

1. 根据id 

```html
<body>
    <div id="btn">你好</div>
    <button id="btn2">点击一下</button>
</body>
```

```javascript
var btn=document.getElementById('btn') ; 
var btn2=document.getElementById('btn2');
```

2. 根据名称：

```javascript
var arr1 = document.getElementsByTagName("div"); //方式二：通过 标签名 获取 元素节点数组，所以有s
```

3. 根据类名

   ```javascript
   var arr2 = document.getElementsByClassName("hehe"); //方式三：通过 类名 获取 元素节点数组，所以有s
   ```

   

#### DOM访问关系

DOM的节点并不是孤立的，因此可以通过DOM节点之间的相对关系对它们进行访问。如下：

 ![img](https://camo.githubusercontent.com/872e06fadfce617f199da912ceda5d0e255cb1a7aa82ccce633af86463c4b902/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303132365f323134302e706e67) 

节点的访问关系，是以**属性**的方式存在的。

JS中的**父子兄**访问关系：

 ![img](https://camo.githubusercontent.com/6f86de4696b3e5732a659861b4e4c40fe08eed6986a7a67079fbb8e9433bae96/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303132365f323134352e706e67) 



**parentNode和children**

**获取父节点**

调用者就是节点， 一个节点只有一个父节点，调用方法就是  ``节点.parentNode `

 **获取兄弟节点**

1. 下一个节点 nextSibling：(火狐，google，等是下一个节点(包括标签，空文档，或者换行节点)) 

    或 `nextElementSibling`：(指的是下一个元素节点(标签))

2.  前一个节点，前一个元素节点 `previousSibling` 或者 `previousElementSibling`
3. 任意一个兄弟节点 ： `节点自己.parentNode.children[index]`
4. 获取第一个子节点 firstchild, firstElementChild 
5. 最后一个子节点  lastChild ,lastElementChild 
6. 获取所有子节点 childNodes 

### 节点操作

**创建节点**

格式：`新的标签 = document.createElement("标签名") `

```javascript
   var a1 = document.createElement("li");
    var b1 = document.createElement("abcd");
```

**插入节点**

1. `父节点.appendChild(新的子节点) `  // 在父节点的最后插入新的子节点
2. `父节点.insert Before(新的子节点,作为参考的子节点) ` 在参考节点前插入一个新节点，如果参考节点是null，就插入父节点的到最后

**复制节点**

```
要复制的节点.cloneNode();
// 默认为false,只复制节点,不复制子节点
要复制的节点.cloneNode(true) ;
// 复制节点和其子节点
```

**获取节点属性值**

```
元素节点.属性名; 

元素节点[属性名]

元素节点.getAttribute("属性名称"); 
```



```html
<img src="blueman.jpg" class="image-box" title="蓝色男人" alt="好看的" id="man"> 
<script type="text/javascript">
    var buleman = document.getElementById("man") ;
    console.log(buleman.src) ;
    console.log(myNode["src"]);
    
    console.log(buleman.className) ;
    console.log(buleman.title) ;  
</script>
```



```    javascript
    console.log(myNode.getAttribute("src"));
    console.log(myNode.getAttribute("class"));   //注意是class，不是className
    console.log(myNode.getAttribute("title"));
```

**设置节点的属性值**

​	元素节点.setAttribute("属性名", "新的属性值");

```javascript
    var buleman = document.getElementById("man") ;
    buleman.src= "girl.jpg" ;
    buleman.className = "girl" ; 

    buleman.title = "bulefamily" ;
buleman.setAttribute("src", "images/3.png");
    alert(buleman.title);
```

**删除节点的属性**

格式:

​	`元素节点.removeAttribute(属性名);`

```javascript
    buleman.removeAttribute("class") ;
    buleman.removeAttribute("id") ;

    console.log(buleman.id) ;
    console.log(buleman.className) ; 
```

**小结**

 **如果是节点的“原始属性”**（比如 普通标签的`class/className`属性**方式1和方式2是等价的**，可以混用。



如果是节点的"非原始标签",在使用两种方式时,区别如下:

1. 方式1绑定的属性值不会出现在标签上
2. 方式二get/set/remove 绑定的属性值会出现在标签上
3. 这两种方式不能交换使用,get和set必须同时使用同一种方式 



```html
<body>
<div id="box" title="主体" class="asdfasdfadsfd">我爱你中国</div>
<script>

    var div = document.getElementById("box");

    //采用方式一进行set
    div.aaaa = "1111";
    console.log(div.aaaa);    //打印结果：1111。可以打印出来，但是不会出现在标签上

    //采用方式二进行set
    div.setAttribute("bbbb","2222");    //bbbb作为新增的属性，会出现在标签上

    console.log(div.getAttribute("aaaa"));   //打印结果：null。因为方式一的set，无法采用方式二进行get。
    console.log(div.bbbb);                   //打印结果：undefined。因为方式二的set，无法采用方式一进行get。

</script>
</body>
```



### 补充

1. innerHTML : 双闭合标签的内容(包括标签)

2. innerText :  双闭合标签的内容 (包含标签)

```html
<body>
    <input id="input1" type="text" value="input_value"/>
    <div id="box1">
        box1 
        <div id="box1.1">
            box1.1
        </div>
    </div> 
    <div id="box2">
        box2 
        <div id="box2.1">box2.1</div>
    </div>
    <script>
        console.log(document.getElementById("input1").value) ;
        console.log(document.getElementById("box1").innerHTML) ;
        console.log(document.getElementById("box2").innerText) ;
    </script>
</body>
```





3. innerHTML: 会修改标签本身,而innerText则不会

```html
document.getElementById("box1").innerHTML = "<h1>我变成h1啦哈哈哈</h1>";
document.getElementById("box2").innerText =  "<h1>我也变成h1啦哈哈哈</h1>";
```



#### nodeType

nodeType属性表示的是该节点的类型,返回的是数值

1表示的是元素节点(标签)  ,2 表示的是属性节点 , 3表示的是文字节点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DOM
    </title>
</head>
<body>
    <!-- 三种标签：元素节点（标签）， 属性节点 ， 文本节点 -->
    <div id="text-box" value="text">吴杭肯</div>
    <script>
         
        var element = document.getElementById("text-box");  // 获取标签 
        var attribute = element.getAttributeNode("id");   // 获取标签的属性节点 
        var txt = element.firstChild;   // 获取节点的第一个子节点 
        var value=element.getAttribute("id"); // 获取节点的属性值
    
        
        console.log(element) ;
        console.log("--------------") ;
        console.log(attribute) ;
        console.log("--------------");
        console.log(txt) ;
        console.log("--------------");
        console.log(value);

        // 获取nodeType
        console.log(element.nodeType) ;
        console.log(attribute.nodeType) ;
        console.log(txt.nodeType);

        // 获取nodeName
        console.log(element.nodeName) ;
        console.log(attribute.nodeName) ;
        console.log(txt.nodeName) ;
    </script>
</body>
</html>
```





### 设置style

在DOM中想要设置样式有两种形式:

1. className (针对内嵌样式)
2. style (针对行内样式)，优先级高 

style是一个对象,只能获取行内样式,不能获取内嵌的样式和外链样式



> 行内样式就是直接把CSS代码添加到HTML的标记中，即作为HTML标记的属性标记存在。通过这种方法，可以很简单地对某个元素单独定义样式。



**通过js读取元素的样式有两个方法:** 语法: 

1. 元素.style.样式名称 (都是行内样式)
2. 元素.style["属性"]   box.style["width"] 可以给属性传递参数

通过js设置元素样式:

语法: 元素.style.样式名=样式值

e.g. 

`box1.style.width="300px" `

`box1.style.backgroundColor="red"`

style属性注意事项：

1. 样式少的时候使用。
2. style是对象。我们在上方已经打印出来，typeof的结果是Object。
3. 值是字符串，没有设置值是“”。
4. 命名规则，驼峰命名。
5. 只能获取行内样式，和内嵌和外链无关。
6. box.style.cssText = “字符串形式的样式”。 `cssText`这个属性，其实就是把行内样式里面的值当做字符串来对待。 



style的常用属性包括：

- backgroundColor
- backgroundImage
- color
- width
- height
- border
- opacity 设置透明度 (IE8以前是filter: alpha(opacity=xx))

注意DOM对象style的属性和标签中style内的值不一样，因为在JS中，`-`不能作为标识符。比如：

- DOM中：backgroundColor
- CSS中：background-color

#### 用className

直接改变元素的类名，会覆盖原先类名

```javascript
<script>
    	div1= docoument.getElementsByTagsName("div")[0] ;
	 div1.className="change" ;
</srcipt>
```



#### 举例1

改变div的大小和透明度

```html
<body>
    <div  style="width:200px ;height:100px ;background-color:pink;"></div>
    <script>
       var div1 =document.getElementsByTagName("div")[0] ;
       div1.onmouseover =function(){
           div1.style.width="300px" ;
           div1.style.height="300px";
           div1.style.backgroundColor="black" ;
           div1.style.opacity="0.2";
           div1.style.filter="alpha(opacity=20)" 
       }
    </script>
</body>
```



#### 举例2

```html
<body>
    <ul>
        <input type="text/">
        <input type="text/">
        <input type="text/">
        <input type="text/">
        <input type="text/">
    </ul>
    <script>
        var inpArr = document.getElementsByTagName("input")
        for(var i=0;i<inpArr.length ;i++){
            inpArr[i].onfocus=function(){
                this.style.border= "2px solid red" ;
                this.style.backgroundColor = "pink";
            }

            inpArr[i].onblur=function(){
                this.style.border="";
                this.style.backgroundColor="";
            }
        }
    </script>
</body>
```



**通过js获取元素当前显示的样式**

有些元素的样式是内嵌样式或外链样式，获取方法为：

1. window.getComputedStyle("要获取样式的元素", "伪元素")
2. obj.currentStyle 

如果当前元素没有设置该样式，则获取它的默认值

该方法会返回一个对象，对象中封装了当前元素对应的样式，可以通过对象.样式名来读取具体的某一个样式

通过currentStyle和getComputedStyle() 读取到的样式都是只读的， 不能修改，如果要修改必须通过style



## 待整合

### 添加监听事件

语法：`element.addEventListener(event,function,useCapture)`

event :必须，字符串，指定事件名

function ：必须，指定要事件触发时执行的函数 

### onclick 

onclick事件会在元素被点击时发生 

```html
  <button onclick ="playPause()">播放暂停</button>
    <button onclick ="makeBig()">放大</button>
      <script>
        function playPause(){
            if(video.paused){
                video.play();
            }
            else video.pause();
        }
        function makeBig(){
            video.width=1080;
        }     
    </script>
```

`document.getElementById(id)`

如果要查找的元素没有ID ，用querySelector()   

`Document.querySelectorAll`  

用法： 

`elementList = parentNode.querySelectorAll(selectors);`

selectors必须是一个合法的css selector 

###  监听事件

js的几个方法

`document.getElementById(idname) ;`

`document.getElementByTagName(TagName)`

获得网页中的元素 

`addEventListener(Event , callBackFunction)` 

Event ： 事件

## js动画

js动画的主要内容如下：

1. 三大家族和一个事件对象
   1. 三大家族：offset、scroll、client
   2. 事件对象 event
2. 动画（闪现、匀速、缓动
3. 冒泡，兼容封装



#### offset

**offsetWidth 和 offsetHeight** ： 获取元素的**宽高 + padding + border**，不包括margin 

- offsetWidth = width + padding + border
- offsetHeight = Height + padding + border

```javascript
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        div {
            width: 100px;
            height: 100px;
            padding: 10px;
            border: 10px solid #000;
            margin: 100px;
            background-color: pink;
        }
    </style>
</head>
<body>

<div class="box"></div>
<script>
    var div1 = document.getElementsByTagName("div")[0];

    console.log(div1.offsetHeight);          //打印结果：140（100+20+20）
    console.log(typeof div1.offsetHeight);   //打印结果：number

</script>
</body>
</html>
```



**offsetParent： 获取当前元素的定位父元素**

- 如果当前元素的父元素，**有CSS定位**（position为absolute、relative、fixed），那么 `offsetParent` 获取的是**最近的**那个父元素。
- 如果当前元素的父元素，**没有CSS定位**（position为absolute、relative、fixed），那么`offsetParent` 获取的是**body**。





**offsetLeft和offsetTop**

offsetLeft： 当前元素相对于其定位父元素的水平偏移量

offsetTop：当前元素相对于其定位父元素的垂直偏移量



offsetLeft 和 style.left的区别

1. offsetLeft可以返回无定位父元素的偏移量，如果父元素都没有定位，以body为准

2. style.left只能获取行内样式，如果父元素都没有定位，则返回""

3. offsetTop返回的是数字，而style.top返回的是字符串，而且带单位字符串

4. offsetLeft 和 offsetTop **只读**，而 style.left 和 style.top 可读写（只读是获取值，可写是修改值）

   总结：我们一般的做法是：**用offsetLeft 和 offsetTop 获取值，用style.left 和 style.top 赋值**（比较方便）。理由如下：

   - style.left：只能获取行内式，获取的值可能为空，容易出现NaN。
   - offsetLeft：获取值特别方便，而且是现成的number，方便计算。它是只读的，不能赋值。

```html
    <style>
        .box1{
            width:300px;
            height: 300px;
            padding:100px;
            margin: 100px;
            position: relative;
            border: 100px solid #000;
            background-color: palegoldenrod;
        }
        .box2{
             width:100px;
            height: 100px;
             background-color: rgb(245, 231, 74);
        }
    </style>
</head>
<body>
    <div class="box1">
        <div class="box2" style="left: 10px"></div>
    </div>
    <script>

        var box2 = document.getElementsByClassName("box2")[0];

        //offsetTop和offsetLeft
        console.log(box2.offsetLeft);  //100
        console.log(box2.style.left);  //10px
    </script>
</body>
```



### 动画的种类

#### setInterval

1. setInterval() 方法可按照指定的周期（以毫秒计）来调用函数或计算表达式。
2. setInterval() 方法会不停地调用函数，直到 [clearInterval()](https://www.runoob.com/jsref/met-win-clearinterval.html) 被调用或窗口被关闭。由 setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。

```
语法：
setInterval(code, milliseconds);
setInterval(function, milliseconds, param1, param2, ...)
```



1. 闪现 
2. 匀速
3. 缓动



## 表单实验

#### 阻止表单提交

onsubmit 时间会在表单中确认按钮被点击时发生  **用在form标签内！**

#### $的作用 

https://www.cnblogs.com/jokerjason/p/7404649.html



#### setInterval()

setInterval(code,millisec[,"lang"]) 方法可按照指定的周期（以毫秒计）来调用函数或计算表达式。

> setInterval() 方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。由 setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。
>
> 返回值：一个可以传递给 Window.clearInterval() 从而取消对 code 的周期性执行的值。





# Jquery

jQuery 是 js 的一个库，封装了我们开发过程中常用的一些功能，方便我们调用，提高开发效率。

js库是把我们常用的功能放到一个单独的文件中，我们用的时候，直接引用到页面里即可。



### jquery的两大特点

1. 链式编程，如.show() 和 .html 可以写成: .show().html() 

   原理:return this

2. 隐式迭代:  在方法的内部会为匹配到的所有元素进行循环遍历，执行相应的方法；而不用我们再进行循环，简化我们的操作，方便我们调用。 

### 开发步骤

1. 引包 
2. 入口函数 
3. 功能实现代码(事件处理)

 ![img](https://camo.githubusercontent.com/d18e6608c5e28733749e28ad36424c29163d6d2dd08ff8cbe8d332eb8461371b/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303230345f313934302e706e67) 



### 入口函数和$号

原生 js 的入口函数指的是：`window.onload = function() {};` 如下：

```javascript
        //原生 js 的入口函数。页面上所有内容加载完毕，才执行。
        //不仅要等文本加载完毕，而且要等图片也要加载完毕，才执行函数。
       window.onload = function () {
           alert(1);
       }
```

而 jQuery的入口函数，有以下几种写法：

写法一：

```
       //1.文档加载完毕，图片不加载的时候，就可以执行这个函数。
       $(document).ready(function () {
           alert(1);
       })
```

写法二：（写法一的简洁版）

```javascript
       //2.文档加载完毕，图片不加载的时候，就可以执行这个函数。
       $(function () {
           alert(1);
       });
```



写法三：

```javascript
       //3.文档加载完毕，图片也加载完毕的时候，在执行这个函数。
       $(window).ready(function () {
           alert(1);
       })
```



**$号:**

jQuery 使用 `$` 符号原因：书写简洁、相对于其他字符与众不同、容易被记住。

jQuery占用了我们两个变量：`$` 和 jQuery。当我们在代码中打印它们俩的时候：

```javascript
    <script src="jquery-1.11.1.js"></script>
    <script>

        console.log($);
        console.log(jQuery);
        console.log($===jQuery);  //true 

    </script>
```

$实际上是一个函数名,如下:



```javascript
$ () //调用我们自定义的函数 $ 
$(document).ready(function(){}); //调用入口函数
$(function(){}) ; //
$("#btnShow") // 获取id属性为btnShow的元素
$("div")  // 获取所有div标签元素
```



### js的DOM对象和JQuery对象

二者区别:

通过JQuery获取的元素是一个数组,数组里面包含着原生JS的DOM对象

将DOM对象转为JQuery对象

`$(js对象)`   eg:`jqBox1 = $(myBOx)`

将Jquery对象转换成DOM对象

`jquery对象[index] ` 推荐

`jquery对象.get(index)`

### jquery选择器

 ![img](https://camo.githubusercontent.com/c4162b92dea956f77aec81467afd5ec025b5bebe5afc921dab777fd865b1f735/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303230345f323132352e706e67) 

 



#### 层级选择器

 ![img](https://camo.githubusercontent.com/4cf4924a9b478a37a9a07655a2495cbd14d85ed0054370b1a59f5866b16af067/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303230345f323133392e706e67) 

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="http://code.jquery.com/jquery-latest.js"></script>
    <script>
       $(function(){
        //    所有后代
           var jqli=$("ul li") ;
           jqli.css("margin",5) ;
           jqli.css("background","pink") ;
            // 直接儿子
           var jqSon=$("ul>li");
           jqSon.css("background","red");
       });
    </script>
</head>
<body>
    <ul>
        <li>111</li>
        <li>222</li>
        <li>333</li>
        <ol>
            <li>aaa</li>
            <li>bbb</li>
            <li>ccc</li>
        </ol>
    </ul>
</body>
</html>
```

#### 基本过滤选择器

 ![img](https://camo.githubusercontent.com/c522744faac7cbaea4de89f3f1e6b42db986d0f14ea6e46454be0457b66da4bd/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303230345f323135302e706e67) 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="http://code.jquery.com/jquery-latest.js"></script>
    <script>
       $(function(){
            // 奇数行的
            $("li:odd").css("background","red");
            // 偶数行的
            $("li:even").css("background", "pink");

            $("li:eq(3)").css("font-size", "40px");
            // 小于index的，序号从0开始
            $("li:lt(1)").css("font-size", "10px");
           // 大于index的，序号从0开始
            $("li:gt(4)").css("font-size", "40px");
            //第一个
            $("li:first").css("background","yellow");
            $("li:last").css("background","green");
       });
    </script>
</head>
<body>
    <ul>
        <li>111</li>
        <li>222</li>
        <li>333</li>
        <ol>
            <li>aaa</li>
            <li>bbb</li>
            <li>ccc</li>
        </ol>
    </ul>
</body>
</html>
```

#### 筛选选择器

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="jquery-1.11.1.js"></script>
    <script>
        jQuery(function () {
            var jqul = $("ul");

            //find(selector); 从jquery对象的后代中查找
            //必须制定参数，如果不指定获取不到元素。length === 0
            jqul.find("li").css("background", "pink");
            console.log(jqul.find());

            //chidlren(selector); 从jquery对象的子代中查找
            //不写参数代表获取所有子元素。
            jqul.children("li").css("background", "green");

            //eq(索引值); 从jquery对象的子代中查找该索引值的元素
            //要写该数组中的第几个。
            jqul.children().eq(0).css("background", "red");

            //next(); 该元素的下一个兄弟元素
            jqul.children().eq(0).next().css("background", "yellow");

            //siblings(selector); 该元素的所有兄弟元素
            jqul.children().eq(0).next().siblings().css("border", "1px solid blue");

            //parent(); 该元素的父元素（和定位没有关系）
            console.log(jqul.children().eq(0).parent());
        });
    </script>
</head>
<body>

<ul>
    <li>生命壹号，永不止步</li>
    <li class="box">生命壹号，永不止步</li>
    <span>生命壹号，永不止步</span>
    <li class="box">生命壹号，永不止步</li>
    <i>生命壹号，永不止步</i>
    <li>生命壹号，永不止步</li>
    <a id="box" href="#">生命壹号，永不止步</a>
    <ol>
        <li>我是ol中的li</li>
        <li>我是ol中的li</li>
        <li>我是ol中的li</li>
        <li>我是ol中的li</li>
    </ol>
</ul>

</body>
</html>

```

