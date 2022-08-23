

### Jupyter Notebook

交互式笔记本，支持40多种编程语言

**安装**

有两种安装方式：pip 或者 Anaconda

（我用了pip安装： pip install jupyter)

下载、安装参考：

https://www.zeketian.com/2020/03/07/Anaconda-jupyter-notebook-install/

https://blog.csdn.net/Shallowmm/article/details/113600843

### Anaconda

conda是一款开源的软件包管理系统和环境管理系统，用于安装多个版本的软件包及其依赖关系，并在它们之间轻松切换，Conda支持语言包括python，R，Ruby，Lua等等 。**作用是进行包管理和环境管理**

Anaconda是一个开源的python版本，其中包含了conda，Python等180个科学包以及依赖项，可以通过Anaconda对不同的py环境进行管理，切换，从而避免一些环境冲突。



## 基础

### 内存

python变量保存的都是值引用，即：变量是对内存及其地址的抽象

在python中，一切皆对象

变量的存储采用了引用语义的方式，存储的只是一个变量的值所在的内存地址，而不是变量的本身 

采用这种方式变量所需的存储空间大小一致，因为变量只是一个引用，也称为对象语义和指针语义。变量每一次初始化，都开辟了一个新空间，将新内容的地址赋值给变量

值语义：有些语言采用的不是这种方式，如C语言。

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190214231236717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FzNDgwMTMzOTM3,size_16,color_FFFFFF,t_70) 

`id函数`：可以通过id函数查看对象的身份，就是对象的内存地址

我们把不同的值赋给变量的时候，变量指向的地址将发生变化，但相同的值地址不发生变化

<img src="D:\Typora\自服务\Python.assets\image-20211004205152693.png" alt="image-20211004205152693" style="zoom: 50%;" />

对于复杂的数据类型，如列表，元组，字典等的修改与赋值

如果只是修改某一项元素的值，或者添加元素，不会改变其本身的地址，只会改变其内部元素的地址引用，但如果对其进行重新赋值操作，就会给列表重新赋值一个地址，来覆盖之前的地址。



切片复制和引用复制：

<img src="D:\Typora\自服务\Python.assets\image-20211004205458110.png" alt="image-20211004205458110" style="zoom:67%;" />

- 用"="进行复制时，只会给现存的对象添加一个新的引用，不会在内存中生成新对象
- 用切片复制时，会创建一个新的对象，和copy（）函数一样

更多知识：https://blog.csdn.net/as480133937/article/details/87305247



Python的每个对象都分为可变和不可变，主要的核心类型中，**数字、字符串、元组是不可变的，列表、字典是可变的。**

对不可变类型的变量重新赋值，实际上是创建了一个不可变类型的对象，并将原来的变量重新指向新创建的对象（如果没有其他变量引用圆有变量的话，原有对象就会背回收）

###  输入

函数： input() 

返回的数据是字符串类型的，如果是整型需要转换

```python
s=input('birth:')
birth=(int)(s)
```

**用input输入多个变量**

```python
a,b,c=input().split(" ")
# 要用空格分开
```



### 输出 

print() 

print() 也可以接收多个字符串，用，隔开可以连成一串输出，print会依次打印每个字符，遇到一个，就会输出一个空格

Python的语法比较简单，采用缩进方式，写出来的代码就像下面的样子：

```python
# print absolute value of an integer:
a = 100
if a >= 0:
    print(a)
else:
    print(-a)
```

以`#`开头的语句是注释，注释是给人看的，可以是任意内容，解释器会忽略掉注释。其他每一行都是一个语句，当语句以冒号`:`结尾时，缩进的语句视为代码块。

**-f表达式**

f表达式（f-string）用花括号`{}`包裹替换字段的字符串文字。

`f'{name} is {age} yeas old'` 

其中{}包裹的是替换字段，相当于占位符

f表达式里的替换字段直接在花括号里面进行变量替换

```python
print(f"序号为{i}的同学拍手") 
```

#### format格式化

字符串对象的内置函数，提供了比百分号格式化更强大的功能，例如调整参数顺序，支持字典关键字等。

该函数把字符串当作一个模板，通过传入参数进行格式化，并使用大括号作为特殊字符代替“%”

①、位置匹配

```python
# 不指定位置，按默认顺序
print('{}{}'.format('hello','world'))
# 指定位置
print('{0}{1}'.format('hello','world'))
# 指定位置
print('{0}{1}{0}'.format('hello', 'world'))
# 通过名称指定参数
print('{wd}{ho}'.format(wd='JOHN', ho='hello'))
print('{wd}{ho}'.format(ho='hello',wd='world'))

#helloworld
#helloworld
#helloworldhello
#JOHNhello
```



```python

# 通过字典设置参数
man={"name": "John", "age": "25"}
print("name: {name}, age: {age}".format(**man))

class Point:
    def __init__(self, x, y):
        self.x, self.y=x, y

    # 通过对象属性匹配
    def __str__(self):
        return 'Point({self.x}, {self.y})'.format(self=self)
print(Point(4,5))

#name: John, age: 25
#Point(4, 5)
```

②、数字格式

### 类型

布尔值可以用`and`，` or`，`not`运算

空值用None表示，None 不能理解为0；0是有意义的，而None是一个特殊的空值 

### 除法

在Python中，有两种除法，一种除法是`/`：

```
>>> 10 / 3
3.3333333333333335
```

**`/`除法计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数：**

```
>>> 9 / 3
3.0
```

还有一种除法是`//`，称为地板除，两个整数的除法仍然是整数：

```python
>>> 10 // 3
3
```

### *

*代表乘法

**代表乘方

### 流程控制

```python
        bi = input ('age:')
        age =  int (bi)
        if age <= 20:
            #记得后面加:
            print('your aget is ', age)
        elif(age > 20 and age < 60):
            print('adult')
        else:
            print('old')

```

### 循环

while语句

while 判断条件（condition）：

​	执行语句（statements

for语句用来遍历任何序列的items，如一个列表或一个字符串

for iter_var in sequence:

​	statements(s) 



### 内置函数

#### eval函数

eval函数用来执行一个字符串表达式，并返回表达式的值。

eval(expression[, globals, [, locals]]) 

expression-- 表达式

globals-- 变量作用域，全局命名空间，（可选）但必须是一个字典对象

locals--变量作用域，局部命名空间，如果提供，可以是任何映射对象。 



```python
while True:
    a = input() 
    guess = eval(a)
    print(type(a)) 
    print(type(guess)) 
    if guess == 0x452//2:
        break
print("hello")
```



#### zip

用于将可迭代的对象作为参数，将对象中对应的元素打包成一个一个元组，然后返回由这些元组组成的列表。 

如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同，利用 ***** 号操作符，可以将元组解压为列表。





## 字符串


https://www.liujiangblog.com/course/python/21
python不支持单字符类型（即c语言中的char）， 单字符也是字符串类型

字符串是不可变类型，**即无法直接修改字符串某个索引对应的字符，**需要转换成列表处理，可以认为字符串是特殊的元组类型

`str [ idx] ='s' ` 类似这样的语句是错误的 

**由下标控制长度，没有结束标识**

两个字符串相等的充要条件是：长度相等，并且对应各个位置上的字符也相等



①、定义字符串：可以使用双引号、单引号、三引号等

`var1="hello "`  `var2='hello'`

#### 访问、查找

访问字符串中的值：

- 可以用[]来截取字符串（切片）：`变量[头下标：尾下标]`

  切片下标超过最大索引或者起始索引大于终止索引返回空字符串

  支持指定步长：[start:stop:step] 前两个索引和普通切片一样

  **切片返回的是新的字符串**

```python
str = "1234567"
# 用于翻转字符串
str2 = str[::-1]
print(str2) 
print(str2==str) 
print(str[0::2]) 
# 从-1（最后一个）开始，倒序2个取
print(str[-1::-2])

7654321
False
1357
7531
```

- 可以通过负数下标访问字符串尾部，下标从-1开始向前递减

`str0="python" ; str0[-1]=='n'`

**字符串查找**

①、使用in 和 not in 判断子串是否存在

```python
str="abceeg" 
print("s" in str) 
print("e" in str) 
print("eee" not in str)
```

②、使用find函数

S.find(sub, [,start,[,end]])->int

find()在[start,end]索引范围内查找sub字符串，如果存在则返回第一个索引，否则返回-1

```python
str="abceeg" 
print(str.find("ee",0,-1))
```

③、使用index函数

`Str.index(sub,[,start,[,end]]) -> int`

index和find函数非常类似，但找不到会抛出异常

④、使用rindex函数



#### 复制

通过切片实现

```python
str0 = ["01234567"]
str1=str0[:]
```

#### 拼接

```python
name ='AI' 
age=4000
print('Hello,my name is %s, I am %s + years old.'%(name,age))
```

```python
name ='AI' 
age=4000
print(f'My name is {name}. Next Year I will be {age+1} ')
```

```python
s='apple'
print(3*s) 
#appleappleapple
```



#### 字符串分割

字符串切片：

`S.split([sep [,maxsplit]]) -> list of strings `

通过指定分割符对字符串进行切片，默认使用所有空白符，返回的是一个字符串列表

```python
str="abceeg\n 1235 \n xyz" 
print(str.split())
print(str.split(' ',1))
# 如果没有该字符，返回的是原字符串
print(str.split('A'))
# 用的是‘12’作为整体去分割
print(str.split('12')) 

['abceeg', '1235', 'xyz']
['abceeg\n', '1235 \n xyz']
['abceeg\n 1235 \n xyz']
['abceeg\n ', '35 \n xyz']
```



#### 互相转换

其他类型转换成字符串：使用str()函数

转换成列表：可以通过list() 方法转为列表

```python
str1="sldk"
list1=list(str1) 
```

此时通过索引就可以更新每一个字符，然后再通过join()函数转会字符串 

```python 
str0 = ''.join(list1)
```

#### 切片

**使用切片更新字符串**

切片操作支持指定步长，格式为 [start:stop:step]，前两个索引和普通切片一样。

```python
str="abceeg" 
str2=str[:3]+"d"+str[-2:]
print(str2)
```



切片操作的步长可以为负数，常用于翻转字符串，此时如果不提供默认值 start 和 stop 则默认为尾部和头部索引：

```python
str1="0123456789"
print(str1[0:1])  # 从0开始拿1个
print(str1[1::2])  # 以2为步长
print(str1[3:])  # 3到end


print(str1[3:-1])  # 下标为3到-1  
print(str1[::-1])
print(str1[7::-2]) #从下标8开始，逆序隔2个元素取值

0
13579
3456789
345678
9876543210
7531
```



#### 编码和格式化

字符串以unicode编码，对应当个字符的编码，python提供了ord函数() 获取字符的整数表示，chr()函数把编码转换成对应的字符

```python
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```



python的字符串类型是`str`，在内存以Unicode表示，一个字符对应若干个字节，如果要在网络上传输或者保存到磁盘上，就需要把str变为字节为单位的`bytes `

byte类型的表示：（每个字符只占用一个字节）

`x=b'ABC' 或 x=b"ABC"`



反过来，如果我们从网络或者磁盘读取了字节流，那么读到的数据就是bytes，要把bytes变为str，就要用decode()方法

 `bytes.decode(encoding, errors) `

```python
        b'ABC'.decode('ascii');
        b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
```



> 在操作字符串时，我们常遇到str和bytes的相互转换， 为了避免乱码问题，应当始终坚持使用UTF-8编码对`str`和`bytes`进行转换。 
>
> 由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：
>
> ```Python
> #!/usr/bin/env python3
> 第一行告诉linux/OS X系统，这个是一个py可执行程序，window会忽略 
> # -*- coding: utf-8 -*-
> 第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。
> 
> 
> ```



原生字符：

使用raw原生字符，r或R可以免除大量转义，直接输出

```python
str0 = r"123\"\'456" 
str1 =R "123\"\'456" 
>>>
123\"\'456
123\"\'456
```



转义和多行显示：

如果字符串内部有很多换行，用`\n`写在一行里不好阅读，为了简化，Python允许用`'''...'''`的格式表示多行内容 

```python
>>> print('''line1
... line2
... line3''')
line1
line2
line3
```





## 数据结构

### list 

内置的数据类型：`list`;一种有序的集合，可以随时添加和删除其中的元素,每个位置存放的都是对象的指针。

列表的数据项不需要具有相同的类型。

列表中的内置函数：

| 方法               | 作用                                                         |
| ------------------ | ------------------------------------------------------------ |
| append(obj)        | 在列表末尾添加新的对象                                       |
| count(obj)         | 统计某个元素在列表中出现的次数                               |
| extend(seq)        | 在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表） |
| **index(obj)**     | **从列表中找出某个值第一个匹配项的索引位置**                 |
| insert(index, obj) | 将对象插入列表，插入的值的下标就是index（挤后面的）          |
| pop(obj=list[-1])  | 移除列表中的一个元素（默认最后一个元素），并且返回该元素的值 |
| remove(obj)        | 移除列表中某个值的第一个匹配项                               |
| reverse()          | 反向列表中元素                                               |
| sort([func])       | 对原列表进行排序                                             |
| copy()             | 复制列表                                                     |
| clear()            | 清空列表，等于del lis[:]                                     |

**零、列表赋值**

```python
list1=['a','b','c']
list2=list1
list1.append('de') 
print(list2) 
# ['a', 'b', 'c', 'de']
```

**list的无穷递归**：

https://cloud.tencent.com/developer/ask/28286

```python 
list1=['a','b','c']
list1.append(list1) 
print(list1) 

# ['a', 'b', 'c', [...]]
```

**①、访问列表中的值**

```python
#下标索引来访问列表中的值
list1=[1,3,4,5] 
print(list1[1]) 
```

 **②、要删除list末尾的元素**

用`pop()`方法： 

 要删除指定位置的元素，用`pop(i)`方法，其中`i`是索引位置： 

**③、`len()`函数获取list元素的个数**

```python
len(list1) 
```

**④、增加元素**

```python
list1.append(obj) #在列表末尾添加
list1.insert(index,obj)  # 在指定位置添加
```



要把某个元素替换成别的元素，可以直接给对应的索引位置：

```python
classmate =['Michael','bob','Tracy']
classmate.append('adam')
classmate.insert(1,'jack')
classmate.pop(2)

classmate[1]=2  // 可以是其他类型的数据
print(classmate) 
```

**负下标：-1 代表最后一个元素，-2代表倒数第二个元素** 

**⑤、遍历列表**

```python
        for i in list:
            #i 表示的是值，不是对应的下标,这种方式只是迭代，无法改变列表的值
            print(i) 
```



```python
        for i in range(len(list)) :
            print(list[i]) 

```

enumerate() 函数 ，返回的是一个enumerate对象

对于一个可迭代的对象，enumerate将其组成一个索引序列，利用它可以同时获得索引和值

```python 
s = [39, 28, 17, 43, 22, 11, 55, 16]
l = len(s)
maxNum = max(s)
minNum = min(s)
average = 0
for idx, num in enumerate(s):
    average += num
average /= l

print("列表的最大值为%d，最小值为%d，平均值为%f" % (maxNum, minNum, average))
```



**⑥、统计元素**

最大值、最小值：

max() 和 min() 函数



**⑦、堆栈、队列**

列表很适合作为一个堆栈来使用把列表的头部作为栈底，表尾作为栈顶，就形成了一个堆栈。 用列表的append()方法可以将元素添加到栈顶，用pop方法可以把一个元素从栈顶释放出来。 

效率都是O（1）



列表做队列的效率不高，因为从列表头部弹出第一个元素的速度不快。 通常使用

`queue.Queue` 作为单向队列，用`collection.deque`作为双向队列。





### range() 

range() 函数返回的是一个可迭代对象，list()函数是一个对象迭代器

`range(stop)`

`range(start,stop[,step])`

start：计数从start开始，默认是从0开始

stop： 计数到stop结束，但不包括stop

step：步长，默认为1 ， 即 range(0,5) = range(0,5,1) 



用法：

①、提供数字

```python
for i  in range(10)
	print(i ) 
>>> 0 1 2 3 4 5   6 7 8 9
提供一个数字会遍历从0到参数-1的数字 ，默认从0开始
```

②、指定区间

```python
for i range(1,4):
	print(i) 
>>> 1,2,3
```

③、指定步长,如果要指定负数步长，区间就要从大到小

```python
for i range(1,12,2):
    print(i)
    >>> 1 3  5 7 9 11 
```

④、结合len函数，遍历一个列表

```python
a= ["google","bing","xiaomi","huawei"]
for i in range(len(a)):
    print(i,a[i]) 
```



#### 列表推导式

迭代语句可以对单个列表语句进行处理得到新列表，称为列表推导。 

 列表推导式可以利用 range 区间、元组、列表、字典和集合等数据类型，快速生成一个满足指定需求的列表。

格式：

`表达式 for i  in 可迭代对象 [if 条件表达式]`

if部分可以省略



```python
a_range = range(10)
# 对a_range执行for表达式
a_list = [x * x for x in a_range]
# a_list集合包含10个元素
print(a_list)
```

>  上面代码的第 3 行会对 a_range 执行迭代，由于 a_range 相当于包含 10 个元素，因此程序生成的 a_list 同样包含 10 个元素，且每个元素都是 a_range 中每个元素的平方（由表达式 x*x 控制）。
>
> [0 , 1 , 4 , 9 , 16 , 25 , 36 , 49 , 64, 81]

**List 的初始化方法**

```python
        # 使用列表推导法 
        arr = [0 for i in range(1000)] 

        # 使用*运算符，[object] * n ,其中n是数组元素的数目 
        arr = [0] * 1000 
```



### tuple

另一种有序列表叫元组：tuple。tuple和list非常类似，但是**tuple一旦初始化就不能修改，它也没有append()，insert()这样的方法。**其他获取元素的方法和list是一样的，你可以正常地使用`classmates[0]`，`classmates[-1]`，但不能赋值成另外的元素。

不可变的tuple有什么意义？**因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple。**

**tuple的陷阱：当你定义一个tuple时，在定义的时候，tuple的元素就必须被确定下来，**



括号`()`既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是`1`。

**所以，只有1个元素的tuple定义时也必须加一个逗号`,`，来消除歧义：**

```python
>>> t = (1,)
>>> t
(1,)
```



元组排序：

```
sorted(Tuple) ;
```



### 切片

取出一个list或tuple的部分元素，比如：

```
>>> L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
```

python提供了slice操作符

`L[0:3]`  表示从0开始取取到3为止，即0，1，2，正好是三个元素，如果第一个索引为0，还可以省略： `L[:3]` 

字符串也可以： 

```python
>>> 'ABCDEFG'[:3]
'ABC'
>>> 'ABCDEFG'[::2]
'ACEG'
#表示所有数，每两个取一个

>>> L[:10:2]: 表示前10个数，每两个取一个 
```



 





### dict 字典

相当于哈希表；

可以存储任意类型对象

通常任何不可变的对象均可以作为索引，比如数字，元组和字符串，**列表可以修改，不可以做为键** 



创建字典：

```python 
dict = {'Michael': 97 ,'bob':88 ,'Tray':22}  # 定义一
dict2 = {Michael= 97 ,bob=88 ,Tray=22}  #定义2
print (dict==dict2)  
# true 
# 创建空字典
dict3 = {} 


# 包含键值对的kv序列，可以用dict进行类型转换
list0 = [ ('key0',1) , ('key2',None), (3,['1','2'])] 
dict0 = dict(list0) 
print(dict0) 

# 通过参数对序列，关键字必须是字符串
dict1= dict(key0=1,key1="abc")
print(dict1) 

{'key0': 1, 'key2': None, 3: ['1', '2']}
{'key0': 1, 'key1': 'abc'}

dict3 = dict(([2, 3], [4, 5]))
print(dict3)
# {2: 3, 4: 5}
```

zip合并

zip()方法可以将两个列表合并成一个zip对象，dict()方法将它转换成字典

```python
dict0 = dict(zip(['one','two','three'],[1,2,3]))
print(dict0) 
# {'one': 1, 'two': 2, 'three': 3}
```

由列表生成定值字典

```python
list1= ['one','tow','three']
list2= [1,2,3]
print(list(zip(list1,list2))) 
#[('one', 1), ('tow', 2), ('three', 3)]
```



**访问字典**

1. 用索引
2. D.get()方法可以在键不存在时返回指定值，如果不指定则返回None

```python
# in 关键字 
'Amy' in dict # 判断在还是不在

#访问字典 ，如果该值不存在，则抛出异常
print(dict['bob'])  #查找
print(dict['winghi'])#异常


print(dict.get('Amys'))  #如果没有就返回None 

print(dict.get('Armys',-1))#还可以定义如果没有返回什么 

```

**键值更新和添加**

```python 
dict['Adam'] = 33   //添加 
print('Adams score' , dict['Adam']) 

 # 键值不存在时更新
D.setdefault(k [,d]) -> D.get(k,d) , also set D[k]=d if k not in D

dict0 = dict(zip(['one','two','three'],[1,2,3]))
dict0['one']=100
print(dict0)

dict0.setdefault('two',300)
print(dict0)

dict0.setdefault('four',400) 
print(dict0)
{'one': 100, 'two': 2, 'three': 3}
{'one': 100, 'two': 2, 'three': 3}
{'one': 100, 'two': 2, 'three': 3, 'four': 400
 
 
 # 更新
 D.update() 
 dict0 = dict(zip(['one','two','three'],[1,2,3]))
dict0.update([('one',10)])
print(dict0)
# {'one': 10, 'two': 2, 'three': 3}
```

dict迭代：

```python
>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for key in d:
...     print(key)
...
a
c
b
```

默认情况下dict迭代的是key，可以用`for value in d.values()` 迭代value

 如果要同时迭代alue和key，可以用`for k,v in d.items()`





字符串也是可以迭代的对象

如：

```python
for ch in 'abcdef'
	print(ch) 
```

`collections.abc`模块的Iteralbe判断是否为可迭代对象。

```python
from collections.abc import Iterable
isinstance('abc',Iterable) ;
isinstance([1,2,3],Iterable);
```



如果要对list实现类似java那样的下标循环，可以使用python内置的enumerate函数将list变成索引-元素对。

```python
for i,value in enumerate(['a','b','c'])
	print(i,value) 
```



### set

```java
#要创建一个set 需要提供一个list作为输入集合 
s1=set([1,2,3]) 
s1.add(4) 
s1.remove(3) 

s2=set([3,4,5]) 

#求交集
print(s1&s2) 
#求并集
print(s1|s2) 

```

### 不可变vs可变对象

list是可变对象

str 字符串是不可变对象

**存进哈希表的key都应该是不可变对象： 所以list不能作为key**



### 生成器

列表作为一个容器，其占的内存和元素个数大小成正比， 也就是每增加一个元素，内存就要分配一块区域来存储它。 

> 如果我们要处理更多元素，那么所占内存就呈线性增大，所以受到内存限制，列表容量是有限的。通常我们并不会一次处理所有元素，而只是集中在其中的某些相邻的元素上。所以如果列表元素可以用某种算法用已知量推导出来，就不必一次创建所有的元素。这种边循环边计算的机制，称为生成器（generator），生成器是用时间换空间的典型实例。

生成器通常由两种方式生成，用小括号()标识生成器表达式和生成器函数

```python
list0 = [x*x for x in range(5)] 
print(list0) 

list_generator0 = (x*x for x in range(5)) 
print(list_generator0) 

list_generator1 = (x*x for x in range(5000000))
print(sys.getsizeof(list_generator0))
print(sys.getsizeof(list_generator1))
#[0, 1, 4, 9, 16]
#<generator object <genexpr> at 0x0000029948275C10>
#112
#112
```

可以看到生成器对象的大小不会因为生成元素的上限个数而增大，也不能够像列表一样被打印出来。

通过next()方法获取生成器的每一个元素 

```python 
list_generator0 = (x*x for x in range(3)) 
print(next(list_generator0))
print(next(list_generator0))
print(next(list_generator0))
print(next(list_generator0))
```

相当于java的迭代next  

```python
list_generator0 = (x*x for x in range(50000)) 
for i in list_generator0:
    print(i) 
```



**生成器函数**

如果表达式比较复杂， 比如斐波那契数列就难以用表达式写出来，这时需要用到生成器函数

```python 
def fib_generator(n):
    i,j=0,1 
    while(i<n) :
        #print(i)
        yield i
        i,j=j,i+j 
print(type(fib_generator))
print(type(fib_generator(5))) 
# 生成函数不是生成器，它返回的对象才是生成器
generator = fib_generator(6) 
# 每调用一次next，生成器就从上次yield的地方继续执行返回。 
print(next(generator)) 
print(next(generator)) 
print(next(generator)) 
```

如果一个函数定义中包含了yield表达式，那它就是一个生成器函数，yield语句类似return会返回一个值，但它会记住这个返回位置，下次next()迭代时就从这个位置的下一行继续执行。 （return语句一次性返回整个结果对象列表，而yield一次返回一个结果。）

## 函数

 在Python中，定义一个函数要使用`def`语句，依次写出函数名、括号、括号中的参数和冒号`:`，然后，在缩进块中编写函数体，函数的返回值用`return`语句返回。 

### 函数参数

#### 位置参数

一个萝卜一个坑

#### 默认参数

当有一个参数可以用，也可以不用时，可以给它设置一个默认参数； 这样的好处是能降低调用函数的难度。

规则： 

- 必选参数在前，默认参数在后，否则Python的解释器会报错

```java
def power(x,n=2) {
    s=1 
    while(n>0)
        n--
    	s*=x
    return s 
}
```

python函数在定义的时候，默认参数L值被计算出来了，即列表 ，因为L也是一个变量，它指向对象`[]` ,每次调用该函数，如果改变了L的内容，则下一次调用时，默认参数的内容就变了，不再是函数定义时的[]了 

```python
def add_end(L=[]): 
    L.append('END')
    return L 
print(add_end([1,2,3])) 
print(add_end()) 
print(add_end()) 

```

==定义的默认参数必须指向不变对象== 



改： 可以用None这个不变对象来实现： 

```
def add_end(L=None)
	if L is None:
		L=[]
	L.append('End')
	return L 
```

#### 可变参数

传入的参数个数不确定 ， 用前缀* 标识

**可变参数在函数调用时自动组装为一个tuple；** 

```python
# 前面加一个*号
def cala(*numbers) :
    sum=0
    for n in numbers: 
        sum +=n 
    return sum 

print(cala(1,2,3,4,5))
print(cala(4,5,6)) 

# 如果已经有一个list或者tuple， py允许在list或者tuple前加一个*号 
# 把元素变成可变参数传入 
nums=[1,2,3] 
print(cala(*nums)) 
```



####  关键字参数 

关键字参数位于可变参数之后，因为位置不明确，**只能指定关键字**，以键值对的方式传递它，

```python
def keyword_only_args(name="default" , *args, age) :
    print("name=%s ,age=%d" %(name,age)) 
    print(args)


keyword_only_args("John",  "Teacher", {"Level": 1},age=23)
#name=John ,age=43
#('Teacher', {'Level': 1})
```

#### 可变关键字参数

可变关键字参数前缀通过 ** 来申明，这种参数类型可以接收0个或多个**键值对参数**，并存入一个字典

```python
def keyword_only_args(name="default" , *args, age, **kwargs) :
    print("name=%s ,age=%d" %(name,age)) 
    print(kwargs) 
    print(args)


keyword_only_args("John",  "Teacher", {"Level": 1},age=23, hobby="swimming", swim="forg",cook="egg" )

#name=John ,age=23
#{'hobby': 'swimming', 'swim': 'forg', 'cook': 'egg'}
#('Teacher', {'Level': 1})
```

**可以看到：参数的处理是有优先级的， 首先通过位置匹配，然后进行关键字匹配，最后剩下的所有参数按照是否提供参数名来对应到可变参数或可变关键字参数。**

**练习：**定义一个可以处理任意类型任意参数数目的函数：

```python
# *args: 可变参数   **kwargs:关键字可变参数
def test_args(*args , **kwargs) :
    print(args) 
    print(kwargs) 

test_args("Teacher", [1,2,3], {"salary":20000}, age=45, sex="female", family="none") 
```



#### 函数参数的传递形式

```python
def test_input_args(list0, num, name="Tom") :
    print("list:%s, num:%d, name:%s" % (str(list0), num, name))

test_input_args([1],2,name="John")
test_input_args(*([1],2), **{"name":"John"})
```

第一种调用和第二种调用的参数作用是等价的。 即可以使用位置和关键字传递，也可以使用可变参数和可变关键字参数传递。第二种参数传递方式可以允许一个函数中调用不同的函数，这个特性对实现装饰器函数非常重要。 



```python
def func1(n) :
    print("from %s ,%d" %(func1.__name__, n))

def func2(n,m) :
    print("from %s ,%d" % (func2.__name__, n+m))
def test_call_args(func , *args, **kwargs) :
    func(*args,**kwargs)

test_call_args(func1,1) 
test_call_args(func2,2,3)  
```

#### 函数文档化

```python
def square(x):
    "calculate the square of the number x"
    return x*x

print(square.__doc__)

```

### 高阶函数

>  functools 模块提供了一系列的重量级函数，这些函数有一个特点，函数调用其他函数完成复杂功能，或把一个函数作为返回值，这类函数被称为高阶（Higher-order）函数。 

#### map

map(func, *iterables) --> map object

make an iterator that computes the function using arguments form each of the iterables .

map()根据传入的函数对指定迭代对象做迭代处理，这一行为很像数学概念中的映射。

```python
mapobj= map(str, [1,2,3] )
print(type(mapobj))
print(mapobj is iter(mapobj))
print(list(mapobj)) 
#<class 'map'>
#True
#['1', '2', '3'] 
在python2，返回的是列表，python3返回的是map，这是一个重大调整，可以用来处理无限序列
```

#### reduce

reduce(function, sequence[,initial]) -> value 

Apply a function of two args cumulatively to the items of sequence from left to right, so as to reduce the sequence to a single value. 

```python
from functools import reduce 
total = reduce ((lambda x,y:x*y) ,[1,2,3,4]) 

print(total) 
```

#### filter

filter(function or none, iterable) --> filter object

> Return an iterator yielding those items of iterable for which function(item) is true.If function is None, return the items that are true

```python
strs = ['hello' ,' ','world']
ret = filter(lambda x: not x.isspace() ,strs) 
print(type(ret)) 
print(ret== iter(ret))
print(list(ret)) 
```

 filter的返回值是一个filter对象，也是一个迭代器，filter还可以用于求交集。 

```python
a=[4,0,3,5,7]
b=[1,5,6,7,8] 
print(list(filter(lambda x:x in a,b)))
```

#### sorted

sorted(iterable, *, key=None, reverse=False) -->new sorted list

Return a new list containing all items form the iterable in ascending order

sorted()相对于列表的L.sort()有以下优点：

- 将功能拓展到所有可迭代对象
- L.sort直接作用在列表上，无返回，sorted返回新的排序列表
- sorted是稳定排序，且有优化，速度更快。 

```python
print(sorted([5, 2, 3, 1, 4]))
print(sorted((5, 2, 3, 1, 4)))
print(sorted({1: 'D', 2: 'B', 3: 'B', 4: 'E', 5: 'A'}))  # 字典默认使用键名排序

# sorted() 返回列表类型，用它对字符串排序，注意类型转换
print(''.join(sorted("hello")))

```

对于复杂对象，我们可以把元素中的部分成员作为排序关键字：

```python
scores = {'John': 15, 'Bill': 18, 'Kent': 12}

new_score= sorted(scores.items(),key=lambda x:x[1],reverse=True)
print(new_score) 
#[('Bill', 18), ('John', 15), ('Kent', 12)]
字典默认用key迭代， 用key来指定迭代成员 
```

对类成员：

```python
class Student(): 
    def __init__(self, name,age) :
        self.name=name 
        self.age=age
    def __repr__(self) :
        return repr((self.name,self.age)) 
Student_object = [
    Student('john',12),
    Student('mary',11) ,
    Student('wc',44)
]
print(sorted(Student_object,key=lambda student:student.age)) 

```





#### partial

partial(func, *args, **keywords) - new function with partial appplication of the given args and keywords 

### 作用域

globals()内建函数返回当前运行程序的所有全局变量，类型为字典：

<font color='blue'>测试python是否有块作用域：</font>

```python
dict0 = globals()
print(len(dict0))
print(dict0.keys())

while True:  # 在代码块中定义 block_para
    block_var = "012345"
    break

print(block_var)
dict0 = globals()
print(len(dict0))
print(dict0.keys())

没有。
```



python中只有模块module，类class以及函数def，lambda才会引入新的作用域，其他代码块（如if-else，elif，while，for，try/except）不会引入新的作用域。

Python一共有4种作用域，分别是：

 L（Local）局部作用域，或当前作用域

E（Enclosing）闭包函数外的函数

G（Globals）全局作用域

B（Build-ins）内建作用域

py解释器在查找变量时按照L->E->G->B作用顺序找 



**一个经典的闭包错误例子：**

```python
def bar() :
    a=0 
    def foo():
        a=a+1 
        return a 
    return foo 

func = bar() 
print(func()) 

>>UnboundLocalError: local variable 'a' referenced before assignment
```

按照闭包函数的机制来分析，func变量对应的闭包有两部分，变量环境a=0 和 a=a+1

问题在于：函数体中的变量和变量环境中的a不是同一个。 

py语言规定，所有赋值语句左侧的变量名如果是第一次出现在当前作用域中，都将被定义为当前作用域的变量。 

### 函数嵌套

传递函数：函数也是对象，那么函数对象的引用也可以作为参数传递给函数

函数内还可以嵌套定义函数 

```python
def foo() :
    a =1 
    b=2 
    def bar () :
        nonlocal a 
        a=a+1
        print(b) 
        print("i am in a bar a= ", a) 
    bar() 
    print("foo() runing") 
foo()  
```

nolocal 只能用于内嵌函数中。



应用：如计算物体重力公式为：G=mg，其中g的数值和地球各个位置不同，可以如下定义函数：

这样可以设定不同的重力加速度值，而不需要传两个参数，调用的时候，直接调用r函数

```python
# G = m*g ; 
def weigth(g) :
    def cal_G(m) :
        return m*g 
    return cal_G 
# 返回值r是一个函数 
r = weight(9.8)   
G = r(1) 
print(G)  
```

### 装饰器

wrapper是一个闭包函数

```python

def func(n=0):
    print("form func, n is %d!" % (n), flush=True)


def log(func):
    def wrapper(*args, **kwargs):
        ret = func(*args, **kwargs)
        print('%s is called' % func.__name__)
        return ret
    return wrapper


# 添加装饰器log函数
a = log(func)
a(0)





@log
def func2(n):
    print("from func2(), n is %d!" % (n), flush=True)
t = log(func)
func2(3)
t(0)

```

使用语法糖@符号，无需在调用处修改函数，只需要在被装饰函数前一行加上装饰器 



**有参装饰器：**

```python  
def log(level='debug') :
    def decorator(func) :
        def wrapper(*args,**kwargs):
            # 调用函数，保存返回值
            ret=func(*args,**kwargs) 
            if level=='warning' :
                print('warning：{} is called'.format(func.__name__)) 
            else :
                print('debug: {} is called'.format(func.__name__)) 
            return ret 
        return wrapper 
    return decorator 


@log(level='debug')
def func(n) :
    print("form func , n is %d" % (n) , flush=True)  

func(2)

#@log 相当于执行了如下操作
# func = log('warning')(func)
# func()

```



**类方法装饰器 ⚙**

对于类方法来说，都有一个默认形参self，所以在装饰器的内部wrapper中也要传入该参数，其他用法和函数装饰器相同。 

```python
import time 
def decorator(func ) :
    def wrapper(self ,*arg,**kwarg):
        start_time = time.time() 
        ret = func(self,*arg,**kwarg) 
        end_time =time.time() 
        print("%s.%s() cost %f second!" %(self.__class__,func.__name__,end_time-start_time)) 
        return ret 
    return wrapper 
    
class TestDecorator():
   
    def mysleep(self,n) :
        time.sleep(n) 
obj = TestDecorator()
# obj.mysleep(2)
s= decorator(obj.mysleep)
s(2)
```

装饰器类🧊：

装饰器类更有灵活性，高内聚，封装性特点。

装饰器必须定义`__call__()`方法，它将一个类实例变成一个用于装饰器的方法

```python
class Tracker():
    def __init__ (self,func):
        self.func=func 
        self.calls = 0 
    def __call__(self,*arg,**kwarg):
        self.calls+=1 
        print("%s has called %d times!"%( self.func.__name__,self.calls)) 
        return self.func(*arg,**kwarg) 

@Tracker 
def test_tracer(val,name="defalut") :
    print("func() name %s, val name=%s"%(val,name)) 


for i in range(2) :
    test_tracer(i,name=("name")+str(i)) 

```



### 闭包

如果在一个内部函数中，对定义它的外部函数的作用域中的变量进行了引用，那么这个子函数就被认为是闭包。 

闭包Closure是引用了自由变量的函数

它有两个显著的特点：

- 它是函数内部定义的内嵌函数
- 它引用了它作用域之外的变量，但并非全局变量

```python
def offset(n) :
    base = n 
    def step(i) :
        return base+i
    return step

offset0 = offset(0)
offset100= offset(100) 
print(offset0(1))   # 1
print(offset100(1))  #101 
```

按照常规分析，offset函数结束后，base作为局部变量应该被释放，不能再被访问了。在py中，当内嵌函数作为返回值传递给外部变量时，将会把定义它时设计到的引用环境和函数自身复制后打包成一个整体返回，这个整体就像一个封闭的包裹，不能再被打开修改，所以形象称之为闭包。 

应用：计算抛物线

```python
def parabola(a,b,c) :
    def p (x) :
        return a*x**2+b*x+c 
    return p 
f = parabola(2,3,4) 
#  2*1 + 3*1+4 = 9 
print(f(1))  
```

使用了闭包后，就可以直接调用p(x) ， 更专注于代码本身。 

> 编程范式提供了（同时决定了）程序员对程序执行的看法。例如：在面向对象编程中，程序员认为程序是一系列相互作用的对象；而在函数式编程中，一个程序会被看作无状态的函数计算的串行。 
>
> 编程语言和编程范型之间的关系十分复杂，因为一个编程语言可以支持多种范型，如C++支持过程化编程，面向对象编程以及范型编程。 



### lambda

```python
def add(x):
    x+=3 
    return x 
numbers = range(10) 
list(numbers)
new_numbers = []  
for i in numbers :
    new_numbers.append(add(i)) 
print(new_numbers)
```

其中的add函数就是一个中间操作，我们可以替换成：

```python 
numbers = range(10) 
list(numbers)
new_numbers = [ i+3 for i in numbers]
```

如果使用lambda函数代替add函数：

```python
numbers = range(10)
list(numbers)
lam = lambda x:x+3 
n2=[] 
for i in numbers:
    n2.append(lam(i))
print(n2)
```



### 不懂

```python 
la = 'python' 
try :
    s= eval(input('请输入整数：'))
    print(s) 
    ls=s*2 
    print(ls) 
except :
    print("请输入整数")
```



## 正则表达式

python提供re模块包含正则表达式的所有功能，由于python中字符串本身也用`\`转义

所以：

```
s='ABC\\-001' 
相当于 'ABC\-001' 
```

所以最好用python的`r`前缀，这样就不用考虑转义问题了

#### 查找

`search()`方法判断是否匹配，如果匹配成功，返回一个match对象，否则返回none.

```python
import re 
phoneNumRegex= re.compile(r'\d\d\d-\d\d\d-\d\d\d\d') 
mo=phoneNumRegex.search('My number is 412-111-3463.') 
print('Phone number found:'+mo.group()) 

```

group()方法

`group(0)永远是原字符串 `

#### 特殊字符

` . ^ $ * + ? { } [ ] \ | ( ) `

` \. \^ \$ \* \+ \? \{ \} \[ \] \\ \| \( \) `



#### |

或者的意思，在正则中符合其中一个就好 

```python
import re 
batRegex=re.compile(r'Bat(man|mobile|copter|bat)') 
mo=batRegex.search('Batmobile lost a wheel') 
print(mo.group())
print(mo.group(1))  

```

#### ?

可选的选项

```python
import re 
batRegex=re.compile(r'Bat(wo)?man') 
mo=batRegex.search('THe adventures of Batman')  
print(mo.group())

mo2=batRegex.search('THe adventures of Batwoman') 
print(mo2.group()) 

```

当需要匹配一个问号时，需要转义

#### *

0次或者多次 

 

```python
import re 
batRegex=re.compile(r'Bat(wo)*man') 
mo=batRegex.search('THe adventures of Batman')  
print(mo.group())

mo2=batRegex.search('THe adventures of Batwowowowowoman') 
print(mo2.group()) 

```



#### +

匹配1次或者多次

```python
import re 
batRegex=re.compile(r'Bat(wo)+man') 
mo=batRegex.search('THe adventures of Batman')  
print('mo==None',mo==None )

mo2=batRegex.search('THe adventures of Batwowowowowoman') 
print(mo2.group()) 
```

#### 多个匹配

`{x}`指定一定要x个才行

`{a1,a2} `出现a1(min)-a2(max)个都行

```python
import re 
haRegex=re.compile(r'(Ha){3,5}') 
mo1=haRegex.search('HaHaHaHa') 
print(mo1.group()) 

```



#### findall()

找到所有匹配的选项

```python
import re 
haRegex=re.compile(r'\d\d\d-\d\d\d-\d\d\d\d')  
print(haRegex.findall('Cell:415-333-9999 Work: 212-555-0000'))  
```

返回的是一个list 

#### 缩写



| **Shorthand character class** | **Represents**                                               |
| ----------------------------- | ------------------------------------------------------------ |
| \d                            | Any numeric digit from 0 to 9.                               |
| \D                            | Any character that is *not* a numeric digit from 0 to 9.     |
| \w                            | Any letter, numeric digit, or the underscore character. (Think of this as matching “word” characters.) |
| \W                            | Any character that is *not* a letter, numeric digit, or the underscore character. |
| \s                            | Any space, tab, or newline character. (Think of this as matching “space” characters.) |
| \S                            | Any character that is *not* a space, tab, or newline.        |

#### 示例

```python
import re 
xmasRegex=re.compile(r'\d+\s\w+')
print(xmasRegex.findall('12 drummers, 11 pipers, 10 lords, 9 ladies, 8 maids, 7swans, 6 geese, 5 rings, 4 birds, 3 hens, 2 doves, 1 partridge') ) 
```



```python
#说明
import re 
# 匹配所有的元音字母
vowelRegex=re.compile(r'[aeiouAEIOU]')
list =vowelRegex.findall('RoboCop eate baby food, BABY FOOD') 
print(list)
# 匹配所有不是元音的字母
vowelRegex2=re.compile(r'[^aeiouAEIOU]')
list2=vowelRegex2.findall('RoboCop eate baby food, BABY FOOD')
print(list2) 
```

#### ^ 和 $ 

`^`在正则表达式的前面要求匹配字符串需要以^后面的字符串开头

`$`放在后面要求匹配字符串要以$前面的结束

^和$放在首尾要求整个字符串都按照这个两个确定的规则匹配 

 

```py
import re 

beginWithHello=re.compile(r'^Hello')
print(beginWithHello.search('Hello,World'))
```



#### re.split()

按照能匹配的字符串进行切分，返回切分后的字符串列表

```python 
re.split(pattern,string[,maxspilt=0,flags=0])
```

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190910204350904.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMxNjcyNzAx,size_16,color_FFFFFF,t_70) 



```python
import re

file_path = r'D:\python\text-summary\LDA主题分类\1.txt'
 with open(file_path, "r", encoding='utf-8') as f:  # 打开文件
    text = f.read()  # 读取文件


text = '徐州 18 岁农家女孩宋爽，今年考入清华大学。除了自己一路闯关，年年拿奖，还帮妹妹、弟弟制定学习计划，姐弟仨齐头并进，妹妹也考上区里最好的中学。这个家里的收入，全靠父亲务农和打零工，但宋爽懂事得让人心疼，曾需要 200 元奥数竞赛的教材费，她羞于开口，愣是急哭了...  戳腾讯公益帮帮她们！#助学圆梦#  江苏新闻的秒拍视频 '
# 按照逗号分隔，#字符切割
 result_list = re.split(r'，', text)


# 按照逗号和句号拆分，两个字符以上切割需要放在 [ ] 中
 result_list = re.split(r'[，。]', text)
# 按照 ，。和空白字符切割
result_list = re.split(r'[，。\s]', text)

# 使用括号捕获分组，默认保留分割符
result_list = re.split(r'([，。])', text)

# 不想保留分隔符，以（?:...）的形式指定，不知道哪里问题，下面代码未实现
 result_list = re.split(r'(?:！[，。])', text)
 print(result_list)

```







## IO 

#### 文件路径

windows下的是\， linux和maxos下的是/ ；

python 提供了 path()函数 提供分割符

```python
from pathlib import Path, WindowsPath 
Path('spam','bacon','eggs') 
print(WindowsPath('spam/bacon/eggs'))
print(str(Path('spam','bacon','eggs'))) 
```

#### 打开文件

用`open`内置函数，传入文件名和标识符：

```python 
>>> f = open('/Users/michael/test.txt', 'r')
```

如果不存在，会抛出IOError错误



打开后，可以用 `read`方法一次性读取文件的全部内容，python将内容读到内存，用一个str对象表示



py提供了with语句帮我们调用close()方法

```python 
with open('/path/to/file','r') as f :
    print(f.read())
```

read(size) 指定最多读取size个字节的内容

readline() 可以读取一行的内容， readlines()一次读取所有内容并按行返回list，因此要决定怎么调用



#### 获取指定文件夹下所有文件名

os.listdir(filePath) 返回指定文件夹下包含文件或文件夹的列表，按字母排序。

```python 
import os
filePath = 'C:\\myLearning\\pythonLearning201712\\carComments\\01\\'
os.listdir(filePath)
```



### 异常



```python
try:
    age = int(input("age = "))
except ValueError as ex  :
    print(ex) 
    print("You didn't enter a valid age.") 
```



```python
def cala (age) :
    if(age <=0) :  
        raise ValueError ("age can't be 0 or less") 
    xfactor = age / 10 

try :
    cala (-1) 
except ValueError as ex :
    print(ex)
```



#### with语句

v-2.6

with 语句适用于对资源访问的场合，确保在使用过程中是否发生异常都会执行必要的清理操作，释放资源。 

> 要使用 with 语句，首先要明白上下文管理器这一概念。有了上下文管理器，with 语句才能工作。



1. 上下文管理协议 (Context Management Protocol) 包含方法enter()和 exit() ， 支持该协议的对象要是想这两个方法
2. 上下文管理器 (Context Manager)  实现了上面协议的对象，负责执行 with 语句块上下文中的进入与退出操作。



## module 

Python有一个交互式的解释器，在解释器中，你可以使用Python的所有功能，但这个解释器是一次性的，也就是说，如果关掉解释器，那么之前定义的，运行的一切东西，都会丢失不见。

我们把相对稳定，代码较长的文本保存成一个py文件。又叫python脚本。 一个.py文件就是一个模块， 在模块内部，模块名存储在全局变量`__name__`中，是一个string 

```python
'a test module' 
__author__ = 'WingChi' 
import sys 
def test() :
    args = sys.argv 
    if len(args)==1  :     
        print("hello") 
    elif len(args) ==2 :
        print("hello,%s" % args[1])
    else :
        print("error")


if __name__ ==  '__main__':
    test() 

```

第一行是一个字符串，表示模块的文档注释，任何模块代码的第一个字符串都被视为模块的文档注释

第二行使用 `__author__ `变量把作者名字写进去

导入sys模块后，我们就有sys变量指向这个模块，可以访问sys模块的所有功能

sys模块中有一个argv变量，用list存储了所有命令行的参数，因此第一个参数永远是文件名 xx.py 

```python
if __name__=='__main__':
    test()
```

#### 模块的识别

对于模块来说，最重要的就是它的`__name__`,每当python执行一个脚本，它就会为该脚本赋予一个名字，对于主程序来说，这一脚本的`__name__`就被定义为`”__main__“`,对于一个被import进主程序的模块来说，这一脚本的`__name__`被定义为base filename，因此我们可以用`if __name__=="__main__"` 在模块代码中定义一些测试代码。 





#### 作用域

在一个模块中，我们可能会定义很多函数和变量，但有的函数和变量我们希望给别人使用，有的函数和变量我们希望仅仅在模块内部使用。在Python中，是通过`_`前缀来实现的。

正常的函数和变量名是公开的（public），可以被直接引用，比如：`abc`，`x123`，`PI`等；

类似`_xxx`和`__xxx`这样的函数或变量就是非公开的（private），不应该被直接引用，比如`_abc`，`__abc`等；（但实际上可以被直接引用）



## psutil



**api列表：**

`psutil.pids()`

 Return a sorted list of current running PIDs. To iterate over all processes and avoid race conditions [`process_iter()`](https://psutil.readthedocs.io/en/latest/#psutil.process_iter) should be preferred. 

## argparse

argparse 模块可以让人轻松编写用户友好的命令行接口.由程序定义它需要的参数，然后解析这些参数

#### 用法

第一步：

创建一个解析器，ArgumentParse对象

添加参数：调用`add_argument()` 方法添加参数



```python
>>> parser.add_argument('integers', metavar='N', type=int, nargs='+',
...                     help='an integer for the accumulator')
>>> parser.add_argument('--sum', dest='accumulate', action='store_const',
...                     const=sum, default=max,
...                     help='sum the integers (default: find the max)')
```

然后，parse_args() 将返回一个具有integers和accumulate两个属性的对象



#### add_argument()

ArgumentParser.add_argument(name or flags)

## 面向对象编程 

```python
print(isinstance(object,type))
print(isinstance(type,object))
print(isinstance(object,object))
print(isinstance(type,type))
True
True
True
True
```

Python的解释器CPython中没有对类Class的实现，只有对Object的实现， 也就是一切介对象，python的类更准确地应该称为Type 



### 对象object

所有对象都有三个基本属性：ID，类型和值 ，也就是有一块内存存储了一个对象，这块内存中一定存有这三个属性。 



### 类型type 

**对象和类型的关系**

- object是所有其他对象的基类，它本身没有基类，数据类型定义为type
- type继承了object，所有类型的对象都是它的实例，包括它自身，判断一个对象是否为类型对象，就看它是否是type的实例。 

### 类

在Python中定义一个新类（class）　等于创建了一个新类型type的对象object，解释器中一切对象均存储在结构中。　

class和type本质上是一样的，我们把用户定义的叫做class，把ｐｙ内置的叫做type。

用class关键字来定义类，通过缩进确定类的代码块。 写在类里面的函数我们通常称之为方法。方法的第一个参数通常`self` 代表了接收这个消息的对象本身

```python
class Point :
    def __init__(self,x,y):
        self.x = x 
        self.y = y

    def draw(self):
        print(f"Point ({self.x},{self.y})") 

point = Point(1,2) 
point.draw()   # 相当于： point.draw(point)  
```

在类的方法中，形参self必不可少，而且要位于其他形参之前，每个类相关联的方法在调用时都要自动传递self，它是一个指向实例本身的引用 

#### 类属性和方法

```python
class Employee() :
    class_version = "v1.0" 
    def __init__(self,id,name):
        self.id = id
        self.name = name


print(Employee.class_version)   # 类名直接访问类属性
worker0 = Employee(0, "John")
print(worker0.class_version)    # 实例访问类属性

Employee.class_version="v1.1" 

```

类属性在内存中只有一份，在创建实例时不会被单独创建，都是引用类的属性。 

同时我们可以通过实例来改变类属性，此时将发生拷贝，该实例的类属性将脱离类的属性，实现属性的解绑定，该属性成了实例的私有属性，其他实例不会被影响 

```python
worker0.class_version = "v1.2" 
print(worker0.class_version)  // v1.2
print(Employee.class_version) //v1.1
```

**类的私有属性，用__开头，私有属性只能够通过类方法访问，不能通过类名或类实例来访问。**

类方法的第一个参数总是cls，指向类本身，在调用时被隐式传递，声明类方法时必须加上@classmethod装饰器说明符

```python
class Employee() :
    _class_version = "v1.0" 
    def __init__(self,id,name):
        self.id = id
        self.name = name
    @classmethod
    def cls_ver_get(cls):
        return cls._class_version 

print(Employee.cls_ver_get()) 

```

**类的静态方法** 

使用@staticmethod装饰器说明符可以定义类的静态方法。但没有参数 cls ，通常用于对一类函数进行封装。

**类的静态方法无法访问类属性，另外，无论是类的静态方法还是类方法，都只能通过类名加.的方式调用，不能间接调用它们**

```python
class Employee() :
    _class_version = "v1.0"  #类私有属性 
    def __init__(self,id,name):
        self.id = id
        self.name = name
    @classmethod
    def cls_ver_get(cls):
        return cls._class_version 
    @classmethod
    def cls_ver_set(cls,newVersion):
        cls._class_version = newVersion 
    @staticmethod 
    def static_get():
        print("this is a static method") 

e1 = Employee(1,"a")  
e2 = Employee(2,"b")  
print(e1.cls_ver_get())
e1.static_get()  
e1.cls_ver_set("v1.2") 
print(e2.cls_ver_get())

#v1.0
#this is a static method
#v1.2
```

**查看类属性和方法** 

dir(...) 

dir([object]) -> list of strings 

dir()内建函数用于获取任意对象的所有属性和方法。 



#### 对象属性和方法

 对一个对象进行动态添加属性和方法，只是它们均为这个对象所私有，不会影响类的其他实例对象。如果对象方法已经存在，则被覆盖。 



```python
class Employee() :
    _class_version = "v1.0"  #类私有属性 
    def __init__(self,id,name):
        self.id = id
        self.name = name
e1 = Employee(1,"a")  
e1.age= 34 
def say_age(self) :
    print("My age is %d" % self.age) 
from types import MethodType
e1.say_age=MethodType(say_age,e1) 
print(type(e1.say_age))
print(type(say_age)) 
e1.say_age() 

```

对象中的函数称为method方法类型，普通函数类型为function，这里借助types模块中的methodType将一个函数转化成一个对象的方法。

在类中定义的函数除非指明是类方法，它的类型默认是function，所以类可以通过赋值来动态绑定新的方法。对象是类的实例，实例化过程中会将function类型转换为method类型。所以对象动态添加方法不能直接赋值。

**限制属性和方法** 

我们可以动态地为对象添加属性和方法，但这也有可能破坏封装，类的特殊变量`__slots__`可以给这种灵活性加以限制。



`__slot__` 是一个名字字符串数组，如果定义了，所有属性名都必须在其中声明。 

这种限制同时对动态绑定起作用。同时方法也成为只读的，不能删除，也不能动态覆盖旧方法，不能添加新方法。 

```python
class Employee() :
    _class_version = "v1.0"  #类私有属性 
    __slots__ = ('id','name') 
    def __init__(self,id,name):
        self.id = id
        self.name = name
    def get_name(self) :
        return self.name 

e1 = Employee(1,"a")  
e1.age = 34  # AttributeError: 'Employee' object has no attribute 'age'
def get_name2 (self): 
    return self.name 
    print('普通的') 


e1.get_name = get_name2  # 'Employee' object attribute 'get_name' is read-only
e1.id=3  #可以
print(e1.id) 

```

但是这种限制只作用在类对象上，并不限制类自身

如果子类也定义了 __slots__ 属性，则它会继承父类的 __slots__ 属性，否则不受父类的限制。

```python 
class programmer(Employee) :
    def __init__ (self,language,id,name): 
        self.language=language 
        super().__init__(id,name) 

engineer = programmer("C",1,"john") 
print(engineer.language )  # C 
```
以上继承不受Employee中slots属性的影响，一旦子类中定义了slots，language也就不是实例的合法属性了。 
`__slots__ =('language') ` 手动定义且添加 

#### 魔法方法

在Python中，所有以`__`双下划线包起来的方法，都统称为“Magic Method”（魔术方法） 

`__init__`  构造器，当一个实例被创建的时候调用的初始化方法 

```python
class Point:
    def __init__(self,x,y):
        self.x=x 
        self.y=y
    def draw(self):
        print("draw")
point=Point(1,2)
print(point.x)
print(type(point))

```

和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数永远是实例变量`self`，并且，调用时，不用传递该参数。除此之外，类的方法和普通函数没有什么区别，所以，你仍然可以用默认参数、可变参数、关键字参数和命名关键字参数。

self是当前对象的引用

**类中方法的互相调用**

方法一： `self.方法(参数列表) ` 不用self

方法二： `类名.方法(self, 参数列表)`

#### 访问限制

在class中，如果想要内部属性不被外部访问，可以把属性的名称前加`__` ， python中的实例变量名如果以__开头，就变成一个**私有变量。**

```python
class Student(object) :
    def __init__(self,name,score):
        self.__name=name
        self.__score=score
    def prints(self):
        print("%s %s"% (self.__name,self.__score))


s=Student("herry",12)
s.prints() 
print(s.__name)  
# AttributeError: 'Student' object has no attribute '__name'


    #但是，Python并没有从语法上严格保证私有属性的访问，他只是给私有属性和方法换了一个名字来阻挠对
    #它们的访问，如果你知道更换名字的规则，还是可以访问到它们
stu1=Student("小明",55) 
print(stu1._Student__name,stu1._Student__score) 
    

```

变量以`__xx__` 命名的，是特殊变量，不是private变量，是可以直接访问的。



python还可以通过property装饰器为“私有”属性提供读取和修改的方法。

装饰器通过@符号放在类，函数或方法的声明之前

```python 
class Student:
    def __init__(self,name,age):
        self.__name=name
        self.__age=age

    @property 
    def name(self):
        return self.__name 

    @name.setter
    def name(self,name): 
        # 如果name参数为空，就赋值无名氏
        # 或； self.__name = name if name else '无名氏'
        self.__name = name or '无名氏'
    def study(self,course):
        print(f'学生正在学习{course}')

    @property
    def age(self):
        return self.__age 

    def play(self):
        print(f'学生正在玩游戏.') 

stu1=Student("小明",12) 
print(stu1.name)
stu1.name = '醉醉'
print(stu1.name)

```



#### 继承

`__mro__`属性记录类继承的关系，它是一个元组类型。 
python中支持类的多重继承。

```python
dict0 = dir(object) 
for i in dict0 :
    print("%-20s:%s" %(i,type(eval("object."+i))))  
    __class__           :<class 'type'>
__delattr__         :<class 'wrapper_descriptor'>
    用于del语句，删除类或对象的某个属性
__dir__             :<class 'method_descriptor'>
#用于类的所有方法名和属性，它是一个字典，内置函数dir()就是对它的调用
__doc__             :<class 'str'>
指向当前类的描述字符串
#class C():
#"A Sample Class C"  # 对类的描述，如果没有则为""
__eq__              :<class 'wrapper_descriptor'>
__format__          :<class 'method_descriptor'>
__ge__              :<class 'wrapper_descriptor'>
__getattribute__    :<class 'wrapper_descriptor'>
    在获取类属性时调用，无论属性是否存在
__gt__              :<class 'wrapper_descriptor'>
__hash__            :<class 'wrapper_descriptor'>
__init__            :<class 'wrapper_descriptor'>
__init_subclass__   :<class 'builtin_function_or_method'>
__le__              :<class 'wrapper_descriptor'>
__lt__              :<class 'wrapper_descriptor'>
__ne__              :<class 'wrapper_descriptor'>
__new__             :<class 'builtin_function_or_method'>
__reduce__          :<class 'method_descriptor'>
__reduce_ex__       :<class 'method_descriptor'>
__repr__            :<class 'wrapper_descriptor'>
repr()函数就是调用对象的__repr__()方法，返回一个py表达式，通常可以在eval中运行它
__setattr__         :<class 'wrapper_descriptor'>
    用于动态绑定属性
__sizeof__          :<class 'method_descriptor'>
__str__             :<class 'wrapper_descriptor'>
#用于str函数转换中，默认使用print打印一个对象时就是对它的调用，我们可以重写这个函数来实现自定义类对字符串的转换 
__subclasshook__    :<class 'builtin_function_or_method'>
```

使用示例：

```python 
class c:
    def __init__ (self):
        self.hello="123" 
    def __delattr__(self,name):
        print("delattr %s" %name) 
        super().__delattr__(name) 
    def __setattr__(self, name, value):
        print("setattar %s" % name) 
        super().__setattr__(name, value)
    def __getattribute__(self, name):
        print("getattribute %s"%name) 
        return super().__getattribute__(name)
cs=c()
del cs.hello 
# print(c.hello) 
cs.newarg="100" 
print(cs.newarg)

输出：
setattar hello
delattr hello
setattar newarg
getattribute newarg
100
```







还有一些特殊的方法没有在基类中实现，但他们有很特殊的功能，比如__call__() 可以将一个对象名函数化，实现__call__()函数的类的实例就是可以调用的，可以调用它

```python
class Employee():
    def __init__(self, id, name):
        self.id = id
        self.name = name

    def __call__(self, *args):
        print(*args)
        print('Printed from __call__')

worker0 = Employee(0, "John")
worker0("arg0", "arg1")

>>>
arg0 arg1
Printed from __call__
```
装饰器类 就是基于 __call__() 方法来实现的。注意 __call__() 只能通过位置参数来传递可变参数，不支持关键字参数，除非函数明确定义形参。

可以使用 callable() 来判断一个对象是否可被调用，也即对象能否使用()括号的方法调用。



```python
class Animal(object):
    def run(self) :
        print("animal is running") 
#这样Dog就继承了Animal类 
class Dog(Animal):
    def run(self):
        print("dog is running")

class Cat(Animal):
    pass 
```

多态：动物下有许多子类，各自都有自己的run方法

1. 多态中的子类既是自己的类型，也是父类
2. 多态中若有针对父类参数定义的方法，所有子类对象都可以调用，并在调用到子类复写过的方法是，调用的是子类的方法 

类都具有＿＿ｂａｓｅ＿＿属性，由于支持多继承，它是一个元组类型，指明了类对象继承了那些类。

```
class A():
    pass 
class B(A) :
    pass 
class C(B,A):
    pass 
print(C.__bases__) 

```



#### 动态vs静态

像java这样的静态语言中，如果想要传入Animal类型，传入的类型必须是Animal或者它的子类，否则无法调用run方法

像python这样的动态语言，不一定需要传入Animal类型，只需要保证传入的对象有i一个run()方法即可。

叫做动态语言的鸭子类型，它不要求严格的继承体系， 一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。 

#### 案例1

> 我们的扑克只有52张牌（没有大小王），游戏需要将52张牌发到4个玩家的手上，每个玩家手上有13张牌，按照黑桃、红心、草花、方块的顺序和点数从小到大排列，暂时不实现其他的功能。

首先从问题的需求中找到对象并抽象出对应的类，此外还要找到对象的属性和行为。扑克牌中至少有3类对象，分别是牌，扑克，玩家，类和类之间的关系可以粗略的分为 is-a （继承），has-a（关联）， use-a （依赖）关系，很明显扑克和牌是has-a关系：一副扑克有52张牌， 玩家和牌之间有has-a和use-a关系，

牌的属性有花色和点数，python可以通过 继承enum模块的Enum类来创建枚举类型



## 数据库





## turtle海龟画图

```python 
from turtle import * 

def drawStart(x,y) :
    # 画笔抬起来（不画）
    pu() 
    print(x,y) 
    #前往、定位 
    goto(x,y) 
    #画笔放下来（开始画画）
    pd() 
    seth(0) 
    for i in range(5) :
        fd(40)
        rt(123) 
        # 从0开始到250，每次递增50的
for x in range(0,250,50) :
    drawStart(x,0) 

done ()
```

























































































