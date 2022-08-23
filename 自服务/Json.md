## json

json实际上就是js的一个子集，在json中，一共就几种数据类型：

- number
- boolean
- string
- null
- array ：就是js的Array `[]`
- `object`，就是`js`的`{...}`表示

json规定了字符集必须是utf-8，并且字符串一定是""，Object的键也必须是`""`

把任何js对象变成JSON，就是把这个对象序列化成一个JSON格式的字符串，这样才能通过网络传递给其他计算机。

接收到一个JSON格式的字符串，只要反序列化成一个JS对象，就可以在JS中直接使用这个对象了。



JSON语法就是JS对象表示语法的子集

`key:value` 





### json数组

在`[]`中书写，数组可以包含多个对象，下面的site 

```javascript
var xiaoming={
    name:"小明",
    age:14,
    gender:true,
    height:1.65,
    grade:null,
    
    'middle-school': '\"W3C\" Middle School',
    skills:["javascript","java","python","list"],
    site:[
        { "name": "工程师", "url":"https://www.runoob.com/json/json-syntax.html"},
        {"name":"google" ,"url":"https://google.com"},
        {"name":"bing", "url":"https://bing.com"} 
    ]
}

var s= JSON.stringify(xiaoming,null,' ') 
console.log(s) 
```







