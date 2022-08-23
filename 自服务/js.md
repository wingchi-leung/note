js简介

是一门动态语言

在网页中运行： 

一 ： <script>...</script> 包含的代码就是js的代码

二： 在html中通过<script src="..."></script>将js文件引入进来

## 数据类型

js中的变量可以在运行时改变。字符串类型的通过赋值可以改变成number型的（动态语言的特点） 

js的变量分为两大类： 原始类型和引用类型

原始类型包括： 

String、Number、Boolean、undefined、null 五种

Symbol. 

`let firstName = undefined ;` 

可以用typeof关键字查看变量的类型 例如 ： 

`typeof  firstName ：  undefined `

也就是说，undefined既是一种类型，也是一个值。

变量是互相独立的: 

```javascript
let x = 10 ;
let y=x ;
x=20 ;
// x=20 ,y=10 
```

引用

```javascript
let x ={value:10} ;
let y= x ;
x.value=20 ; // y.value==20 
```

当其为一个对象的时候，变量x和y实际上是保存了对象的地址， 修改x或y指向的对象的值，实际上是对同一个变量进行操作。 



### Number 

js不区分整数和浮点数，统一用Number表示。

应用数据类型有Object,Array,Function 

### Object

```javascript 
 person ={
    name :'mosh',
    age : 30 ,
    class:  2 
};
//键值对 
person['name'] = 'jonh' ; 
console.log (person) ;
```

两种方式访问其属性：

person['name'] ='john' ;

person.name='john' ;

加点比较常用

括号法： 当不知道要用哪个属性时，需要用户选择，就可以用到括号法。

```javascript
 person ={
    name :'mosh',
    age : 30 ,
    class:  2 
};
let selection ='name' ;
person[selection]='mary' ;
```



### String

字符串的对象包装类型

每个实例都有一个length属性

**字符访问**

```js
var str ="hello,world"
如果超出了length访问，则返回空字符串
str.charAt(1) ; //e  

str.charCodeAt(1) //101 ,得到的是字符编码ASCII而不是字符
str[1] // e 
```

**字符串操作**

```
var str ="hello,"
//将一个或多个字符串拼接起来
var res = str.concat("world") ; 
```



### Arrays 数组





数组的长度可以变，存储的类型可以不同。

所有数组实例都会从Array.prototype继承属性和方法，修改Array的原型会影响到所有数组实例。 

#### 创建数组

两个方法：

1. 字面量方式： var arr =[1,2] 

2. var arr = new Array(参数);

   参数为空：表示创建一个空数组； 参数是一个数： 表示数组长度； 参数是多个数：表示数组中的元素

检测是不是数组对象 ：

1. instanceof  

2. Array.isArray() 

#### 增、删



| 方法      | 描述                                                         | 备注           |
| --------- | ------------------------------------------------------------ | -------------- |
| push()    | 向数组的**最后面**插入一个或多个元素，返回结果为新数组的**长度** | 会改变原数组   |
| pop()     | 删除数组中的**最后一个**元素，返回结果为**被删除的元素**     | 会改变原数组   |
| unshift() | 在数组**最前面**插入一个或多个元素，返回结果为新数组的**长度** | 会改变原数组   |
| shift()   | 删除数组中的**第一个**元素，返回结果为**被删除的元素**       | 会改变原数组   |
|           |                                                              |                |
| slice()   | 从数组中**提取**指定的一个或多个元素，返回结果为**新的数组** | 不会改变原数组 |
| splice()  | 从数组中**删除**指定的一个或多个元素，返回结果为**被删除元素组成的新数组** | 会改变原数组   |
|           |                                                              |                |
| fill()    | 填充数组：用固定的值填充数组，返回结果为**新的数组**         | 不会改变原数组 |

在数组的开头，末尾，中间增加元素

```javascript
const numbers=[3,4] ;
let number= [] ; 
//add at End 
numbers.push(5,6) ; //3,4,5,6

//add at Beginning 
numbers.unshift(1,2);//1,2,3,4,5,6

//at anywhere 

number.splice(1,0,'a','b');
//1,'a','b',3,4,5,6 
```

`splice(startnumber , deletecount, ... )` 

startnumber： 删除或添加的下标

deletecount： 要删除的个数

后面的参数： 要添加的元素 







```javascript
const numbers =[1,2,3,4] ;
//End 
const last  = numbers.pop() ;
console.log(last ) ; //返回被删除的最后一个元素

const first = numbers.shift(0) ;
console.log(first) ;//返回被删除的第一个元素

//delete 关键字
delete numbers[0] ;

//sec Arg： 要删掉元素的个数
const mid = numbers.slice(0,2);
console.log(mid) ;//返回一个数组保存被删除的所有元素
```



#### 查找

查找元素要看它的类型是值还是引用。 

`indexOf(index,fromIndex) ;`
`lastIndexOf(index ,fromIndex );`//没有就返回-1 

第二个参数： 是从那个下标开始找，可以不传。 

判断是否存在
`includes` 

`numbers.includes(1) //retrun boolean  `

查找引用类型

回调函数

>  A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action. 

find()方法返回数组中满足提供的测试函数的第一个元素值，否则返回undefined 

语法：`arr.find(callback[,thisArg])` 

`callback` 在数组每一项执行的函数，接收3个参数（回调函数）

element 当前遍历到的元素

`index   [可选] `  当前遍历到的索引

`array   [可选]   `     数组本身

`thisArg [可选]  ` 执行回调时用作this的对象

>  `find`方法对数组中的每一项元素执行一次 `callback` 函数，直至有一个 callback 返回 `true`。当找到了这样一个元素后，该方法会立即返回这个元素的值，否则返回 `undefined`

```javascript
const courses =[
    {id:1 ,name :'a'} ,
    {id:2 ,name :'b'} ,
];
function  foudIt(course){
    return course.name==='a';
}
const course = courses.find( foudIt ) ; 
//注意这里是传入一个函数,不是要一个函数的返回值不要加()
console.log(course) ;

//第二种写法
const res = courses.find(function (course) {
    return course.name === 'a' ;
}) ;

```

箭头函数

用箭头函数语法简化代码

`const res = courses.find( course=>course.name === 'a') ;`

删掉function，`=>`分开函数参数和函数体

如果回调函数只有一个参数，如上面只有一个course参数，可以去掉括号 

如果回调函数只有一个语句，如上，可以去掉大括号和return



#### 清空数组

```javascript
let  numbers =[1,2,3,4] ;
let  another =numbers ; 
// Solution 1 有两多引用的时候不好用 
numbers=[] ; 
console.log(numbers); 
console.log(another); //还在

// Solution 2 强制清空
numbers.length=0 ;
console.log(numbers); 
console.log(another); //清空

//Solution 3 
numbers.splice(0,numbers.length) ;
console.log(numbers); 
console.log(another); //清空

//Solution 4 
while(numbers.length>0) 
numbers.pop() ;//花哨大 
//推荐1，2 
```

#### 切割组合



| 方法     | 描述                                                 | 备注             |
| -------- | ---------------------------------------------------- | ---------------- |
| concat() | 合并数组：连接两个或多个数组，返回结果为**新的数组** | 不会改变原数组   |
| join()   | 将数组转换为字符串，返回结果为**转换后的字符串**     | 不会改变原数组   |
| split()  | 将字符串按照指定的分隔符，组装为数组                 | 不会改变原字符串 |

```javascript
const first = [1,2,3] ;
const second =[4,5,6] ;//concat函数连接
const combined =first.concat(second) ; //[1,2,3,4,5,6]
const slice1 = combined.slice(2,4) ; [3,4]
const slice2 = combined.slice(2) ;[3,4,5,6]
const slice3 = combined.slice() ;相当于复制 
```

如果数组元素是值类型，直接复制值

如果是引用类型，复制的是对象的引用不是对象，所以复制出来的引用和原引用指向同一个对象。

join()和split()方法 

```javascript
const message = 'This is my first message'; 
const part = message.split(' ') ;//返回数组
 // ["This", "is", "my", "first", "message"]
parts=part.join('-');
console.log(parts); //This-is-my-first-message 
```

用来解决url问题

#### 拆分操作符

`...`  使用拆分操作符的时候，每一个元素都被单独返回。 

```java
const first = [1,2,3] ;
const second =[4,5,6] ;
const combined =[...first,'a',...second] ; //组合
const copy = [...combined] ; //克隆
```

#### 排序

1. sort函数和reverse函数

两个方法都会改变原来的数组，返回新数组

```javascript
const numbers = [2,3,1] ;
numbers.sort() ;
numbers.reverse() ;//翻转得到逆序
```

2. 比较对象

用回调函数

```javascript
const courses =[
    {id:1 , name : 'Node.js'} ,
    {id:2,  name : 'Javascript'},
] ;

//@a:第一个比较的元素
//@b：第二个比较的元素
courses.sort(function(a,b){
    //a<b  return -1 
    //a>b  return 1 
    //a==b return 0 
    if (a.name<b.name) return -1 ;
    if (a.name>b.name) return 1 ;
    return 0 ;
} )

console.log(courses);
```

#### 其他方法 

every函数和some函数

```javascript
const numbers = [1,2,3,-1];
const atLeastOnePositive = numbers.some(function(value){
    return value>=0 ; 
}) ; 
console.log(atLeastOnePositive) ;

//erery:检测全部符合条件,检测到一个不符合条件的就返回
//some:检测至少有一个符合条件,检测到一个符合条件的就返回
```

`Array.prototype.filter()`

将所有在过滤函数中返回 `true` 的数组元素放进一个新数组中并返回。

`Array.prototype.map()`

返回一个由回调函数的返回值组成的新数组。

map方法创建一个==新数组==， 其结果是该数组中的每个元素是调用一次提供的函数后的返回值。

```javascript
const numbers =[1,4,9,16] ;
const mapping = numbers.map(x=>x+1) ;
//2,5,10,17
```

```javascript
const items = numbers
             .filter(v=>v>=0)
             .map(v=>({value:v}))  //映射成一个对象
             .filter(x=>x.value>1) 
             .map(obj=>obj.value) ;
        
console.log(items);
```

reduce

方法签名： `Array.prototype.reduce(callback(accumulator,currentValue[,index[,arrya]]) [,initialValue]) `

`reduce()` 方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。 

```javascript
let sum = numbers.reduce((accumulator,currentValue)=>accumulator+currentValue ,b )  ;
console.log(sum);
```

callback函数4个参数： 

@accumulator:累加器 

@currentValue:当前值     

理解累加器和当前值：

==用户的reducer函数的返回值赋值给累加器，该返回值在每次迭代中都被记住，最终成为单个结果值==

`@?index `：数组当前处理元素的索引，如果提供了initialValue，则起始索引号为0 ，否则为1 

`@array`：调用reduce方法的数组



### Map

>  　　JavaScript 中的对象（Object），本质上是键值对的集合，但是只能用字符串来做键名。这给它的使用带来了很大的限制。

>  ES6 提供了Map数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。



#### 创建Map

Map本身就是构造函数，在使用构造函数时，通过传参进行数据初始化

`let map= new Map()；`

`let map2 =ne Map([['name' , 'zhangsan' ], ['age','12']])`

#### 属性

size： 返回map实例的成员总数

#### 操作

```javascript
set(key, value)　　 添加或修改数据。设置 key 所对应的键值，并返回 Map 结构本身

get(key)　 获取数据。读取 key 对应的键值，如果找不到 key，返回 undefined

has(key)　　　查看是否存在某个数据，返回一个布尔值。

delete(key) 删除数据。删除成功返回 true

clear()　 清除所有数据，没有返回值
```

#### 遍历

```javascript 
Map 提供了三个遍历器生成函数和一个遍历方法:

keys()　　　　 返回一个键名的遍历器

values()　　　 返回一个键值的遍历器

entries()　　　 返回一个键值对的遍历器

forEach()　　   使用回调函数遍历每个成员
```

#### 与其他数据结构的转换

**map转换成数组：使用扩展运算符 ...** 

```javascript
let myMap = new Map();
myMap
 .set(true, "真")
 .set(false, "假");//因为每次会返回新Map，可以连着写
console.log(myMap); // {true => "真", false => "假"}
let newMap = [...myMap];
console.log(newMap); // [[true, "真"], [false, "假"]]
```



**数组转换map**

```
　将数组传入 Map 构造函数中，就可以转为 Map。

 
let arr = [[true, "真"], [false, "假"]];
let map = new Map(arr);
console.log(map); // {true => "真", false => "假"}
```



### 操作符

`let x =29 ;	`

`let y =2; `

`x**y ; //意思为 x的y次方 `

全等于： ===  严格相等，必须为相同类型和相同的值 

抽象相等 ==   相同的值即可，如果类型不同，会将右边的类型转换成左边的。 

`console.log(1=='1')`  //true 

`console.log(1==='1')   ` //false 类型不同。 



#### 三元操作符

```javascript
let points=90 ;
let type = points>100? 'gold' :'sliver' ; 
console.log(type) ; 
```

#### 逻辑运算符

` && ， || ， ！ `

 ==用非布尔数据进行逻辑操作==

得到的结果不一定是布尔型数据 ,js引擎在解析时，会查看每一个参数，如果不是一个布尔数据，就会将其转换成类真量或类假量。 

#### 类假量： 

`undefined ， null， 0 ， false， ‘ ’， NaN  ` 

NaN： 就是不是一个数字的意思 。 （Not a Number ） 

除了 类假量，都是类真量 。

示例：

```javascript 
  false || 'Mosh'//结果"Mosh" 
  false || 1    // 1 
```

如果客户不选择，就默认为蓝色bule， 如果客户选择了如 userColor ='red' , 输出 red 

```javascript 
let userColor = undefined ; 
let defaultColor ='bule' ;
let currentColor =userColor||defaultColor ;
console.log(currentColor) ;   //输出 bule 
```

小程序： 

检查一个数组，输出其不是假量或类假量的值得个数

```javascript
function countTruthy(array) {
    let count =0 ;
    for(let i of array){
        if(i) count++ ;
    }
    console.log(count) ;
}

const array=[0,null, 3 ,' ' , 2 ,3] ;  
countTruthy (array)  ; 
```

这里是js的引擎直接会将不是布尔型的值转换成类真量和类假量，直接试就好了，不用一个一个比较。 

### 循环

for，while，do..while

#### for-in

**多用来遍历对象里的属性，数组多用for-of 循环** 

```javascript
person{
    name : 'Mosh' ;
    age  : 30 
};
for(let key in person ){
   	console.log(key,person[key]) ; 
    //在这里不能用. key的值为name，age 但是如果为： person.key 是错的，因为里面没有key属性。 
}
const colors=['red' ,'green' ,'blue'] ;
for(let index in colors){
    console.log(index ,colors[index]) ; 
}
```

#### for-of 

**多用在数组中。** 

```javascript
const colors = ['red' , 'green' ,'ulue'] ; 
for(let color of colors) 
    console.log(color) ;  //color直接代表一个元素。 
```

#### 遍历对象

**对象是不可以被枚举的，==for-of只能用于可枚举的类型。 所以不能使用for-of循环进行遍历==。** 

1. 使用 Object.keys() 遍历

   返回一个数组，包括对象自身的（不包括继承的）所有可枚举属性 （不含Symbol属性） 

```javascript
for(let key of Object.keys(circle))
    console.log(key , circle[key]) ; 
```

2. Object.entries()方法 ： 及那个对象作为参数，把这个对象可枚举属性[key, value]对作为数组的元素，然后返回这个字符串数组。 

```javascript
for(let entry of Object.entries(circle))
    console.log(entry) ;
```



### 内存情况&对象

基本数据类型都是直接保存在栈内存里，值与值互相独立

对象是保存在堆内存中的，每创建一个新的对象，就会在堆内存中开辟一个新的空间，变量保存的是对象的内存地址（对象的引用） 

**对象的值是保存在堆内存的，对象的引用（变量）是保存在栈内存中的，如果两个变量保存的是同一个对象的引用，那么当通过其中一个变量修改属性时，另一个也会有影响**



对于引用类型的数据，赋值相当于地址拷贝，a、b指向了同一个堆内存地址。所以改了b，a也会变；本质上a、b就是一个东西。

```javascript
let a = 1;

let b = a;// 将 a 赋值给 b

b = 2; // 修改 b 的值
修改 b 的值之后，a 的值并不会发生改变。
```

```javascript
var obj1 = new Object();
obj1.name = "孙悟空";

var obj2 = obj1; // 将 obj1 的地址赋值给 obj2。从此， obj1 和 obj2 指向了同一个堆内存空间

//修改obj2的name属性
obj2.name = "猪八戒";

修改 obj2 的name属性后，会发现，obj1 的 name 属性也会被修改。因为obj1和obj2指向的是堆内存中的同一个地址。
```





如果你打算把引用类型 A 的值赋值给 B，让A和B相互不受影响的话，可以通过 Object.assign() 来复制对象。效果如下：

```javascript
var obj1 = {name: '孙悟空'};

// 复制对象：把 obj1 赋值给 obj3。两者之间互不影响
var obj3 = Object.assign({}, obj1);
```

## 对象 

#### 工厂模式

工厂模式应该是最广为人知的一种设计模式了，它抽象了创建对象的具体过程，解决了重复代码的问题。由于ECMAScript中没有类的概念，所以只能使用函数来封装创建对象的细节

语法： `function createObject()` 

```javascript
function createObj (name ,age ) {
    //创建一个新对象
    let obj =new obj ;
    //向对象中添加属性
    obj.name =name ;
    obj.age =age ;
    obj.sayName=function(){
       	alert(this.name); 
    };
    return obj ;
}
//这种方式创建的对象的类型都是Object
var obj1= createObj('a',32) ;
obj1 instanceof Object     // true 
obj1 instance of createObj //false 
```



```javascript
function createCircle (radius ){
    Circle ={
        radius, // 键和值相同,可以省略:let radius : radius ; 
        draw() {
            console.log('draw') ;
        } // draw : function(){}
    }; 
    return Circle ;
}
```



构造函数 

函数用帕斯卡命名标记（每一个单词首字母都大写） func

```javascript
function Circle(radius) {
	this.radius=radius;
	this.draw=function(){
		console.function('draw') ;
	}
    //retrun this ; // 不需要 
}

const circle =new Circle(1) ;
cicle.draw() ; 
```

(如果不写new 就是一个普通的函数)

说明： new 操作符经历了4个步骤：

1. 创建一个新对象（在内存中开辟了一个空闲的空间） 
2. 将this指向这个新对象
3. 执行构造函数中的代码
4. 返回新对象 

#### 增/删

js是一门动态语言：对象可以添加属性，方法，也可以用delete关键字删除属性和方法

```javascript
//续上
circle.color='yellow' ;
circle.method=function(){} 
delete circle.radius ;
delete circle.method ;
```

说明： **circle定义为 const circle 指的是circle不能再指向其他对象，但是依然可以添加，删除它的属性和方法**。 

每一个对象都有构造器属性，其应用的是创建这个对象的构造函数。 



#### 私有属性

当希望对象里的一些值不被外界调用，直接定义成本地变量，不设置位属性

```javascript
//此对象的coler,defaultLocation,和computeLoc都是私有的。 
function Circle(radius){
    let color='red' ; 
    this.radius = radius; 
    let defaultLocation={
        x:1,
        y:1
    }; 
    let computeLoc =function(factor){
        return defaultLocation.x*factor;
    }
   
    this.draw=function(){
        computeOptimunLocation(0.1); 
        console.log(draw) ;
    };

}

```

#### 内置对象

1. Math.PI

   Math.floor() 

   Math.random() 返回是 [0,1) 小数 左闭右开 

2. String :既是一个原始类型，也可以是一个对象

```javascript
const message = 'hello' ;
const s       =  message.substr(0,3) ; //javascript引擎会把它转换成一个对象
const str =new String('hello') ;
//typeof message = string , typeof str=object ;
```

##### Date

 Date 对象是一个**构造函数** ，需要**先实例化**后才能使用。 

```javascript
const date1=new Date('May 11 2029 11:54') ;

//没有设置的参数默认为0 
const date2 =new Date(2028,3,11,12);
               //year/month/day/hour/minute/seconds

//没有参数：系统当前时间来设置
const now =new Date() ;

console.log(now) ;

console.log(now.toTimeString()) ;

//标准ISO格式，通常用于网络服务
console.log(now.toISOString()) ; 
```

#### 时间戳

指的是从格林威治标准时间的`1970年1月1日，0时0分0秒`到当前日期所花费的**毫秒数**（1秒 = 1000毫秒）。

计算机底层在保存时间时，使用的都是时间戳。时间戳的存在，就是为了**统一**时间的单位。

getTime() 获取时间戳

获取date对象的时间戳

```javascript
 <script>
        const timestamp1 = +new Date() ;
        console.log(timestamp1);

        const timestamp2 = new Date().getTime() ; 
        console.log(timestamp2) ;

        console.log(Date.now()); 
    </script>
```



练习1：模拟日历

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>时间戳</title>
    <style>
    </style>
</head>
<body>
    <div class="content"> </div>
    <div></div>
    <script>
        function getCurrentDate(){
            const date=new Date() ;
            const year =date.getFullYear();
            const month = date.getMonth() ;
            const day = date.getDate() ;
            const week = date.getDay();
            console.log(year + " " + month + " " + day +" " + week);
            const arr= ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
            return '今天是 ：' + year +'年'+ (month+1)+'月' + day+'日' + arr[week];
        }
        const div = document.getElementsByTagName('div')[1];
        div.innerText = getCurrentDate();
    </script>
</body>
</html>
```





### this

1. 在对象方法中，this指的是此方法的“拥有者” 

```javascript
fullName : function()
return this.firstName ; 
```

2. 单独的this 

单独使用时，拥有者是全局对象，因此this指的是全局对象

在浏览器窗口中，全局对象是`object Window `  

3. 在函数中的this（默认） 

默认是全局对象`object  Window`    

4. 函数中的this（严格模式）

严格模式是undefined ;

显示函数绑定

call() 和 apply() 是预定义方法。 

它们可以用于将另一个对象作为参数调用对象的方法。

``` javascript
var person1 ={
	fullName :function() {
		retrun this.firstName+this.lastName;
	}
}
var person2 ={
	firstName : 'bill' ;
	lastName : 'Gates' ;
}
person1.fullName.call(person2)
```

判断对象是否含有某个key ：使用in 操作符

`if('radius in circle') console.log('yes') ；`













### 转义字符

转义字符： 

js中的转义符 ： \ ' ,  \n 来解决字符串中出现 '' 和换行的问题，但是这样编码非常糟心和难看。 

```javascript
const message = 'I\'am \"ok\" ';
```

### 模板语法

Template literals 用反引号`` 表示多行字符。 

用了反引号``来定义字符串，可以不使用转义字符，直接书写字符串

```javascript
const another =`hi ,this is 'big R`; 
```

占位符： 

使用${} 代表一个占位符 placeholders 

```javascript
const name = 'John' ;
const message =`Hi , bro, this is ${name}`; 
console.log(message) ;
```





```javascript
//计算数组中每个元素出现的次数 
function countOccurences(array,searchElement){
      return array.reduce((acc,curr)=>{
            let occurrence = (curr===searchElement?1:0);
            console.log('频率：'+occurrence+ ' 累加器：'+acc+ '当前值 :'+curr) ;
            return acc+occurrence ;
     } ,0);  
}
//每一次调用回调函数频率要么是0要么是1,都会返回给acc 
//acc不是真正的累加每次返回值,两者可以理解为返回值将赋值给acc 

function join(numbers) {
    return numbers.reduce( (acc, current)=>{
        return acc.concat(current) ;
    },[]);
}//initalValue给出,acc初始化值就是initialValue的值
```

### 函数

#### 函数声明

1. 用function关键字

```javascript
walk() ; //可以
function walk() {
    console.log('walk') ;
}
```

- 可以在声明函数的前面就调用此函数 

原因： hoisting--javascript引擎在解析的时候会将所有函数声明提到代码的顶端。

2. 立即执行函数
3. 函数表达式（匿名函数） 

```javascript
sayHello();
// sayHello.call();
function sayHello() {
    alert("欢迎");
}

//匿名函数
let fun = function() {
    alert("我是匿名函数") ;
};

fun.call(); 


// 即刻执行函数
(function(){
    alert('我是及时执行函数');
})() ;
```

- ！使用函数表达式创建的函数不会被声明提前，所以不能在声明之前调用

#### 绑定事件函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<style>
    div{
        background-color: aliceblue;
        width:4%
    }
</style>
<body>
    <div id="btn">你好</div>
    <button id="btn2">点击一下</button>
</body>
<script src="front.js"></script>

</html>
```

```javascript
var btn=document.getElementById('btn') ; 
var btn2=document.getElementById('btn2');
btn.onclick=function(){
    alert('点击按钮，会出现奇迹');
}
btn2.onclick=function(){
    alert('点击真正的按钮,会出现更大的奇迹');
}
```



#### 参数

在函数调用中传入的参数和声明中的参数个数不同的时候，不会报错

```javascript
function sum(a,b ){
return a+b ;
}
sum(1); //1+undefined=NaN 
sum(1,2,3)//1+2=3 
```

在JS中，形参的默认值是undefined

`arguments`对象是一个传递给函数的参数的Array-like对象

>  **Note:** “Array-like” means that `arguments` has a `length`property and properties indexed from zero, but it doesn't have `Array's`  built-in methods like `forEach()`and `map()`

```javascript
//遍历求和 
function sum(){
    let total=0 ;
    for(let i of arguments){
        total+=i ; //有一个迭代器，可以使用for-of
    }
    console.log(total);
    console.log(arguments);
}
sum(1,2,3,4,5) ; 
```



#### 函数名和函数体

函数名等于整个函数

函数加载问题：js加载的时候，只加载函数名，不加载函数体，所以如果想使用内部的成员变量，需要调用函数

#### fn() 和 fn 的区别

- `fn()`：调用函数。调用之后，还获取了函数的返回值。
- `fn`：函数对象。相当于直接获取了整个函数对象。



#### 类数组arguments

在调用函数时，浏览器每次都会传递进两个隐含的参数

1. 函数的上下文对象：this
2. 封装实参的对象：arguments 

```javascript

function foo() {
    alert(arguments);//[object Arguments]
    alert(typeof arguments);//object
}
foo();
```

arguments 是一个类数组对象，它可以通过索引来操作数据，也可以获取长度。

**arguments 代表的是实参**。在调用函数时，我们所传递的实参都会在 arguments 中保存。arguments**只在函数中使用**。

1. 返回函数实参的个数  `arguments.length`
2.  返回正在执行的函数 ： `arguments.callee`  
3. 修改元素

arguments的展示形式是一个**伪数组**。伪数组具有以下特点：

- 可以进行遍历；具有数组的 length 属性。
- 按索引方式存储数据。
- 不具有数组的 push()、pop() 等方法。



#### 余下操作符

rest operator :     `...` 

将一个不定数量的参数表示为一个数组 

 和拆分操作符区别，当用在函数的参数时就是余下参数

==只能是函数的参数列表中的最后一个==

`function sum(a,...args,c) 是错的`

```javascript
function sum(...args){ //求和第二种方法 
    return args.reduce((a,b)=>a+b) ;
}
console.log(sum(1,2,3,4,5)) ; 
```

函数默认参数

```javascript
//计算利息
//需求：如果没有传rate和years使用默认（设置默认参数） 
function interest(principal,rate,years) {
    rate =rate||3.5; 
    years=years||5; 
    return principal*rate/100*years;
}
console.log(interest(1000)) ;
//新方法，在声明的时候就设置
function interest(principal,rate=3.5,years=5) {
   return principal*rate/100*years;
}
```

注意：如果在设置的时候只设置了rate没有years，在调用也没有传入或只传了两个参数，years依然是undefined 

所以最好保证设置默认参数的时候将其放在参数列表的最后一个，或者保证其后所有参数都有默认值。

### getter/setter

keyword : `get` & `set` 

```javascript
//获取一个人的全名 
const person={
    firstName :'Mosh' ,
    lastName:'hamdani',
    get fullName(){
        return `${person.firstName}${person.lastName}`; 
    }
    set fullName(fullName){
        const parts =fullName.split(' ') ;
        this.firstName=parts[0] ;
        this.lastName=parts[1] ;
    }
};
person.fullName='John smith' ;
console.log(person.fullName) ; //可以去掉括号像属性一样调用

```

try-catch 

#### var和let 

```javascript
function start () {
    for(var i =0 ;i<5;i++) 
        console.log(i) ;
    console.log(i) ;
}
start();
```

1. 使用var定义变量，它的作用域是它所属的函数的作用域

   使用let定义，它的作用域是for代码块 

2. 用var定义的变量会被添加到window对象的属性种

   ps： 定义的函数也会被添加到window对象种 

3. window对象是一个核心对象 

### this 

>  在绝大多数情况下，函数的调用方式决定了`this`的值。`this`不能在执行期间被赋值，并且在每次函数被调用时`this`的值也可能会不同。ES5引入了bind方法来设置函数的`this`值，而不用考虑函数如何被调用的，ES2015 引入了支持`this`词法解析的箭头函数（它在闭合的执行环境内设置`this`的值）。 





根据函数的调用方式的不同，this 会指向不同的对象：

1. 以函数的形式（包括普通函数、定时器函数、立即执行函数）调用时，this 的指向永远都是 window。比如`fun();`相当于`window.fun();`
2. 以方法的形式调用时，this 指向调用方法的那个对象
3. 以构造函数的形式调用时，this 指向实例对象
4. 以事件绑定函数的形式调用时，this 指向**绑定事件的对象**
5. 使用 call 和 apply 调用时，this 指向指定的那个对象



#### 改变this的指向

JS 专门为我们提供了一些方法来改变函数内部的 this 指向。常见的方法有 call()、apply()、bind() 方法。



当一个函数在其主体中使用this关键字时，可以通过使用函数继承自`Function.prototype`的call或apply方法将this值绑定到调用中特定的对象 

```javascript
    function add(c,d) {
        return this.a+this.b+c+d; 
    }
    let o={a:1,b:3} ;

    add.call(o,5,7) ; 
//1,3,5,7 ->16 

    add.apply(o, [10, 20]); 
 // 1 + 3 + 10 + 20 = 34
```

call方法的语法：

`fun1.call(想要将this指向哪里,函数实参1,函数实参2);`  

call可以实现继承

------

apply方法需要传递数组

`fun2.apply(对象,[函数实参1,函数实参2]);`

如果传递给this的值不是对象，javascript会尝试用ToObject操作将其转换成对象。 



#### bind方法 

bind方法不会调用函数，但可以改变函数内部的this指向

 调用`f.bind(someObject)`会创建一个与`f`具有相同函数体和作用域的函数，但是在这个新函数中，`this`将永久地被绑定到了`bind`的第一个参数，无论这个函数是如何被调用的。 

```javascript
    function f(){
      return this.a;
    }

    var g = f.bind({a:"azerty"}); //返回的是一个新函数
    console.log(g()); // azerty

    var h = g.bind({a:'yoo'}); // bind只生效一次！
    console.log(h()); // azerty
```



有些方法，本身可以传入一个this，如forEach  

## 浏览器

js可以获取浏览器提供的很多对象，并进行操作。

### 作用域Scope

- **概念**：通俗来讲，作用域是一个变量或函数的作用范围。作用域在**函数定义**时，就已经确定了。
- **目的**：为了提高程序的可靠性，同时减少命名冲突。

**分类：**

在 JS 中，一共有两种作用域：（ES6 之前）

- 全局作用域：作用于整个 script 标签内部，或者作用域一个独立的 JS 文件。
- 函数作用域（局部作用域）：作用于函数内的代码环境。

当在函数作用域中操作一个变量时，它会先在自身作用域中寻找，如果有就直接使用（就近原则），如果没有则向上一级作用域寻找，直到全局也没有找到则报错ReferenceError 

### 变量提升

使用var关键字声明的变量（ 比如 `var a = 1`），**会在所有的代码执行之前被声明**（但是不会赋值），但是如果声明变量时不是用var关键字（比如直接写`a = 1`），则变量不会被声明提前。

```javascript
    console.log(b);  // undefined

    console.log(a); // 报错相当于window.a 
    c=110
    console.log(c) ; //110 ; 相当于window.c

    var b=111;  //相当于window.b
    let a=123 ;

```

### 函数的声明提升

使用`函数声明`的形式创建的函数`function fun(){}`，**会被声明提前**。

也就是说，整个函数会在所有的代码执行之前就被**创建完成**。所以，在代码顺序里，我们可以先调用函数，再定义函数。



使用`函数表达式`创建的函数`var foo = function(){}`，**不会被声明提前**，所以不能在声明前调用。

很好理解，因为此时foo被声明了（这里只是变量声明），且为undefined，并没有把 `function(){}` 赋值给 foo。

### 函数作用域

在函数作用域中，也有声明提前的特性

1. 在函数中用var关键字声明的变量会在函数中所有的代码执行之前被声明
2. 函数中没有var声明的变量都是全局变量，而且不会提前声明

```javascript
   <script>
        var a =1 ; 
        function foo(){
            console.log(a) ; //1 
            a=2; 
        }
        foo() ;
        console.log(a);//打印2
  
    </script>
```
//去掉第一行var a=1 会使得函数打印错误

定义形参就相当于在函数作用域中声明了变量 

```javascript
	    function foo(e){
            console.log(e) ;
            
        }
        foo(2) ;//2 
        foo() ; //undefine 
```
### 作用域链
引入：
1. 只要是代码，就至少有一个作用域
2. 写在函数内部的局部作用域
3. 如果函数内还有函数，那么这个作用域中又诞生了一个作用域

**作用域链：内部函数访问外部函数的变量，采用的是链式查找的方式来决定取哪个值，这种结构称之为作用域链。查找时，采用的是就近原则。**

```javascript
  var num=10 ;
    function fn() {
        var num=20 ;
        function fun(){
            console.log(num);//20	 
        }
        fun() ; 
    }
    fn() ; 
```



### 预编译

javascript运行三部曲：语法分析、预编译、解释执行

**预编译两个规律：** 

1. 任何变量未申明就赋值，此变量属于window的属性，而且不做变量提升
2. 一切声明的全局变量，都是window的属性

```javascript
function a(){
    var a =10 ;  //声明了但在函数体内为全局变量
    b=10 ; // 没有声明，归window所有
}
var c=10;// 全局上的变量，归window所有
```



```javascript

function fun(){
    var a=b=100; 
}
fun() ;
console.log(window.b) ;
console.log(b) ;

console.log(window.a) ;
console.log(a);
---------------------------------
100
100
undefined
Uncaught ReferenceError: a is not defined
// 100赋值给b,然后再声明变量a,把b的值赋值给a
// b为未经声明的变量,由规律1 b属于window.b 
// 而a的作用域仅限于fun()内部
```



#### 函数预编译步骤：

1. 创建AO对象，AO即Activation Object 活跃对象，就是执行器上下文
2. 找**形参和变量**声明，将形参和变量作为AO的属性名，值为undefined
3. 将实参值和形参统一，**实参值赋给形参**
4. **查找函数声明，函数名作为AO对象的属性名，值为整个函数体**

#### 示例1

```javascript
        var a=1; 
        console.log(a) ;
        function fun(a){
            console.log(a) ; 
            var a=123; 
            console.log(a) ; 
            function a() {}
            console.log(a) ; 
            var b= function(){} ;
            console.log(b) ;
            function c() {}  
        }
        var d = function() {
            console.log("i am d fun");
        }
        console.log(d) ;
        
        fun(2) ;
```

```javascript
                   打印结果
            1
            ƒ () {
                console.log("i am d fun");
            }
            ƒ a() {}
            123
            123
            ƒ (){}

         /**
                 *  1.创建AO活动对象（Active Object）；
                 *  2.查找形参和变量声明，值赋予undefined；
                 * AO ={
                 * a:undefined ,
                 * b:undefined ,
                 * }
                 * 3.实参值赋给形参；
                 *   AO = {
                 *  a:2,
                 *  b:undefined,}
                 *4. 查找函数声明，值赋予函数体；
                     AO={
                         a:function a() {} 
                         b :undefined 
                         c: function c() {}
                     }
                    开始执行函数
                    AO={
                        a:function a() {} 
                        b:undefined 
                        c: function c() {}
                    }-->
                    AO={
                        a=123 ;
                        b :undefined
                         c: function c() {}
                    }-->
                    AO={
                        a:123;
                        b :function b(){}
                         c: function c() {}
                    }
                 */
```

#### 示例2

```

    a=100 ; 
    function demo(e) {
        function e(){} 
        console.log('e=  '+e) ;  
        arguments[0]=2; 

        console.log('e=  ' +e) ; 
        console.log('a        = ' + a);  
        console.log('window.a = '+window.a);
            if(a) {
                var b=123;
                function c(){} 
            }
        var c ;
        a=10 ; 
        var a; 
        console.log('b='+b); 
        f=123;
        console.log('c='+c); 
        console.log('a='+a);  
    }

    var a ;
    console.log('a='+a);  
    demo(1) ;
    console.log(a);   
    console.log(f);
```

![image-20210301202105486](D:\Typora\自服务\js.assets\image-20210301202105486.png)

https://segmentfault.com/a/1190000018001871



### this的指向

当函数执行时，（函数发生预编译的前一刻） 会创建一个执行器上下文的内部对象，一个执行期的上下文定义了一个函数执行时的环境

每次调用函数，就会创建一个新的上下文对象，它们是相互独立的，函数执行完毕则被销毁

**this：**解析器在调用函数每次都会向函数内部传递进一个隐含的参数，这个隐含的参数就是 this，this 指向的是一个对象，这个对象我们称为函数执行的 上下文对象。

### 函数内的this指向

函数的调用有**六种**形式。

根据函数的调用方式的不同，this 会指向不同的对象：

1. 以函数的形式（包括普通函数、定时器函数、立即执行函数）调用时，this 的指向永远都是 window。比如`fun();`相当于`window.fun();`

2. 以方法的形式调用时，this 指向调用方法的那个对象
3. 以构造函数的形式调用时，this 指向实例对象
4. 以事件绑定函数的形式调用时，this 指向**绑定事件的对象**
5. 使用 call 和 apply 调用时，this 指向指定的那个对象

6. 箭头函数中this的指向：  ES6 中的箭头函数并不会使用上面的准则，而是会继承外层函数调用的 this 绑定（无论 this 绑定到什么）。













### 高阶函数

当 函数 A 接收函数 B 作为**参数**，或者把函数 C 作为**返回值**输出时，我们称 函数 A 为高阶函数。

通俗来说，高阶函数是 对其他函数进行操作 的函数。

```javascript
    function fn1(a,b,callback){
        console.log(a+b) ;
        callback && callback() ; 
    }


    fn1(10, 20, function () {
        console.log('我是最后执行的函数');
    });
```

```javascript
function fn2()
{
    let a=20 ;
    return function(){
        console.log('a='+a) ;
    };
}
const foo=fn2(); 
foo();
```



### 闭包

我们知道，变量根据作用域的不同分为两种：全局变量和局部变量。

- 函数内部可以访问全局变量和局部变量。
- 函数外部只能访问全局变量，不能访问局部变量。
- 当函数执行完毕，本作用域内的局部变量会销毁。

![image-20210301210408402](D:\Typora\自服务\js.assets\image-20210301210408402.png)



```javascript
function fn1()
{
    let a=20 ;
    function fn2(){
        console.log(a) ;
    }
    fn2(); 
}
fn1(); 
// 函数fn2的作用域访问了fn1的局部变量,那么此时在fn1中产生了闭包,fn1称为闭包函数
```

闭包的作用：延伸变量的作用范围

```javascript
function fn1()
{
    let a=20 ;
    function fn2(){
        console.log(a) ;
    }
    return fn2; 
}
const foo = fn1(); 
foo();
```

![image-20210301212354897](D:\Typora\自服务\js.assets\image-20210301212354897.png)





### Josn

JOSN：JavaScript Object Notation  

JSON 和对象字面量的区别：JSON 的属性必须用双引号引号引起来，对象字面量可以省略。







