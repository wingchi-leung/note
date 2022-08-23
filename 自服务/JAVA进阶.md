





## JAVA进阶

### 时间类

👴java.util.Date 包含年月日时分秒，精确到毫秒级别

```java
 Date date =new Date() ;
System.out.println(date);  //Sun Apr 24 11:59:55 CST 2022 
```

👴 java.sql.Date

包含年月日，时分秒都被设置为0，之所以这样设计是为了适应SQL中的`DATE`类型。

虽然说这个类是使用年月日的，但是初始化的时候，需要一个long类型的参数，这个参数代表着January 1, 1970, 00:00:00 GMT到某个时间的毫秒数。如果是当前时间的话，可以用System.currentTimeMillis()或者new Date().getTime()获取。

```java 
  java.sql.Date sqlDate = new java.sql.Date(System.currentTimeMillis()) ;
System.out.println(System.currentTimeMillis());  //1650772795288

System.out.println(sqlDate);  //2022-04-24 
```

👴 java.sql.Time

包含时分秒 

```Java
// 语句
Time time = new Time(System.currentTimeMillis());
System.out.println(time);

// 输出结果
15:07:35
```

👴java.sql.Timestamp 

时间戳，适配sql中的`TIMESTAMP`类型，精确到纳秒级别

格式化输出：java.text.SimpleDateFormat

  这个类提供时间的各种格式化输出和将字符串转换为时间类，简单来说，它拥有`date → text` 以及`text → date`的能力。
例如：将Date格式化输出 、 将时间字符串转化为Date 

```java
 public static void main(String[] args) throws ParseException {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss") ;
        String datestr= sdf.format(new Date()) ;
        System.out.println(datestr);   // 将时间格式化

        // 将字符串转成date
        SimpleDateFormat sdf_StrToTime=new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
        Date date = sdf.parse("2022年03月12日 12:06:12") ;
        System.out.println(date);
    }
```



### transient

https://juejin.cn/post/6844904003877208077

transient关键字的主要作用是让某些某些变量不被序列化。

序列化：java提供的一种对象序列化的机制，用一个字节序列可以表示一个对象。

### 注解

Java.Annotation JDK5.0 引入。Annotation其实是一种接口，通过Java的反射机制相关的API来访问Annotation信息。

**通过注解开发人员可以在不改变原来代码的逻辑下嵌入补充信息。**

用于对代码进行说明，可以对类，包，接口，字段，方法参数，局部变量进行注解，主要有以下方面作用：

- 生成文档，通过代码标识的元数据生成javadoc文档。
- 编译检查，通过代码里标识的元数据让编译器在编译期间进行检查验证
- 编译时动态处理，编译时通过代码里标识的元数据动态处理，例如动态生成代码
- 运行时动态处理，运行时通过代码里标识的元数据动态处理，例如反射注入实例







#### 内置注解：

- @Override - 检查该方法是否是重写方法。如果发现其父类，或者是引用的接口中并没有该方法时，会报编译错误。
- @Deprecated - 标记过时方法。如果使用该方法，会报编译警告。
- @SuppressWarnings - 指示编译器去忽略注解中声明的警告。

#### 元注解

用来负责注解其他注解，meta-annotation； 

- @Retention - 标识这个注解保留的时间范围，是只在源代码中，还是编入class文件中，或者是在运行时可以通过反射访问。
- @Documented - 标记这些注解是否包含在用户文档中。
- @Target - 标记这个注解应该是哪种 Java 成员。
- @Inherited - 被它修饰的Annoation将具有继承性，如果某个类使用了被@Inherited修饰的Annotation，则其子类将自动具有该注解。

#### 自定义注解

使用`@interface`，它继承了java.lang.annotation.Annotation 接口，并不是申明了一个interface 

使用：

- 注解中可以添加成员变量，以方法形式定义
- 需要使用@Retention注解来规定它的生命周期（编译期间、运行时等）
- 需要使用@Target注解来规定它的适用范围（类型、方法、字段、方法参数等）







### RTTI

Run-Time Type Identification 运行时期类型鉴定----手上只有基础类型的一个句柄时，利用它判断一个对象的正确类型。

> 句柄 handle 在英语中有操作，处理控制的意思。 指某个中间媒介，通过这个媒介可以控制某样东西； 如door handle 门把手 可以控制门但不是door本身， knife handle 



## SPI

SPI Service Prodiver Interface ，是JDK内置的一种服务提供发现机制，可以用来启用框架扩展和替换组件。

https://www.jianshu.com/p/46b42f7f593c

![](D:\Typora\自服务\JAVA进阶.assets\java-advanced-spi-8.jpg)





## 反射

能够分析类能力的程序称为反射，反射机制可以用来：

1. 在运行时分析类的能力。
2. 在运行时查看对象

java反射机制(Reflection)是运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法，并且调用它的任意一个方法和属性，这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制

**动态语言：**

在运行时可以改变其结构的语言，例如新的函数，对象，甚至代码可以被引进，已有的函数可以被删除或者是其结构上的变化

主要的动态语言： C#， JS，PHP，Python   

**静态语言：**

运行时结构不能变的语言就是静态语言

JAVA可以称为“准动态语言”， 有一定动态性，而其动态性就是由反射实现的！

#### Class对象

**反射的入口**

Java运行时系统始终为所有对象维护一个被称为运行时的类型标识，这个信息跟踪每个对象所属的类。虚拟机利用这些运行时类型信息选择相应的方法执行。

在执行其RTT，多态就是基于RTT实现的

每一个类都有一个Class对象，Class类中没有公共的构造方法，Class对象是由类加载时由虚拟机以及通过类加载器中的defineClass方法自动构造的。 



每次写一个新类时，同时也会创建一个Class对象（保存在完全同名的.class文件中）。 在运行期间,一旦我们想生成那个类的一个对象，JVM首先检查那个类型的Class对象是否已经载入，若尚为载入，JVM回查找同名的.class文件，并将其载入，所以JAVA程序启动时并不是完全载入的。 

`Class` 没有公共构造方法。`Class` 对象是在加载类时由 Java 虚拟机以及通过调用类加载器中的 `defineClass` 方法自动构造的。

####  类加载细节



 ![img](https://pic1.zhimg.com/v2-41136c8c767fb7469ddb9c446f662de8_b.jpg) 

1. 当程序主动使用某个类时，如果该类还未加载到内存中，系统会通过加载，连接，初始化三个步骤来对该类进行初始化，这三个步骤称为类加载或类初始化 
2. .java文件经过javac编译后，生成.class文件在磁盘中，（.class是二进制文件，内容是只有jvm才能识别的机器码）然后经过类加载器加载类到内存，生成Java.lang.Class类的实例，这个实例就是程序访问这个类的入口。 通过这个class实例的newInstance方法可以得到类的实例对象

#### 获取Class对象

**如何获取Class类对象？**

1. 使用类对象的getClass() 方法

   Apple apple =new Apple() ;

   Class clazz = apple.getClass() ;

2. 类名.class （类字面常量）,只适合在编译前就知道操作的Class

   Class clazz = Apple.class;

3. Class.forName(“类的全限定名”) 

   Class clazz = Class.forName("jvm.apple") ;  

```java
public class Apple {
    private int price ;

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    public Apple() {

    }
}

class test{
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException,
            InvocationTargetException, InstantiationException, IllegalAccessException {
        Class clazz = Class.forName("jvm.Apple");
        Method setPrice = clazz.getMethod("setPrice", int.class);
        Constructor appleConstructor = clazz.getConstructor();
        Object appleObj = appleConstructor.newInstance();
        setPrice.invoke(appleObj,12);
        Method getPriceMethod = clazz.getMethod("getPrice");
        System.out.println("price="+ getPriceMethod.invoke(appleObj));
    }
}

```



public static Class<?>forName(String className)

返回与带有给定字符串名的类或接口相关联的 `Class` 对象 

调用 `forName("X")` 将导致命名为 `X` 的类被初始化。



#### 创建实例

**如何通过Class对象创建实例对象？**

①、使用Class对象的newInstance()

创建一个此Class对象的新实例，如果该类未初始化，则初始化这个类

==注意：这个类的必须能被访问的有一个空参数的构造函数！否则报错==

```java
  public static void main(String[] args) throws Exception {
        createObject();
        // 相当于 Person obj=new Person();
    }
    public static void createObject() throws Exception {
        String name = "it.cast.bean.Person" ;
        Class clazz = Class.forName(name) ;
        Person obj = (Person)clazz.newInstance();
    }
```



②、使用Constructor对象的newInstance方法

```java
    public static void main(String[] args) throws Exception {
   	    String name = "it.cast.bean.Person" ;
        //找寻该名称类文件，并加载进内存产生class对象
        Class t = Class.forName(name) ;
        //获取到了指定的构造函数对象
        Constructor personConstructor = t.getConstructor(String.class, int.class);
        //通过该构造器对象的newInstance方法进行对象的初始化
        Object obj = personConstructor.newInstance("xiaoming",12) ;
    }
```



#### 获取字段

Field用于获取某个类的属性或者该属性的值。

步骤

1. 获得字节码文件后，Class.getDeclaredField(String name) 等方法获取字段
2. 如果该字段是私有的，需要通过其父类的方法`setAccessible(boolean flag)` 方法取消访问的权限检查
3. 实例化一个对象， 每个对象的字段值都不同，==要指明一个实例对象==才能拿到该对象的字段值

方法

```java 
Class.getDeclaredField(String name) ; //返回Feild对象，该对象包含Class对象所表示的类或接口的指定已声明的字段（包括私有成员）
Class.getDeclaredField();  //返回 Field 对象的一个数组
Class.getFields() ; //返回一个数组，该数组包含此 Class 对象所表示的类或接口的所有可访问公共字段。



Object get(Object obj)// 返回指定对象上此Field表示的字段的值，但是获取私有属性的时候必须先设置Accessible为true，然后才能获取。

void set(Object obj,Object value) // 通过set方法可以重新设置字段的值。
```



```java
Field field=person.getClass().getDeclaredField("name");
System.out.println(field.get(person));
field.setAccessible(true); //如果是私有属性，要设置权限。
field.set(person, "Maoge");
```







#### Method

每个方法都包括返回值，名称，参数，注解以及抛出的异常。

继承的方法（包括重载，重写，隐藏的）会被编译器强制执行，这些方法都无法反射。因此，**反射一个类的方法时不考虑它的父类方法**

通过Class对象获取Method的API：

- Clazz.getMethod(name, Class...)：返回某个public类的Method（包括父类）
- Clazz.getDeclaredMethod(name, Class...)：返回当前类的某个Method（不包括父类）
- Clazz.getMethods()：返回所有public的Method（包括父类）
- Clazz.getDeclaredMethods() ：返回当前类的所有Method（不包括父类

**Method对象的常用API：**

- getName: 返回方法名称，例如”getPrice“
- getReturnType() ：返回方法返回值类型，是一个Class实例，如String.class
- getParameterTypes() ： 返回方法的参数类型，是一个Class数组
- getModifiers() : 返回方法的修饰符

```java 
class test{
    public static void main(String[] args) throws Exception {
        final String fmt = "%24s:   %s\n";
        Class clazz = Class.forName("jvm.Apple");
        System.out.println(clazz.getCanonicalName());
        Method[] methods = clazz.getDeclaredMethods();
        for(Method method :methods){
            System.out.println(("Method name:  " + method.getName()));
            //获得完整的方法信息（包括修饰符、返回值、路径、名称、参数、抛出值）
            System.out.println("GenericString: "+method.toGenericString());
            System.out.println("ReturnType: "+method.getReturnType());
            System.out.println("Genreic ReturnType: "+method.getReturnType());

            Class<?>[] parameterTypes = method.getParameterTypes();
            Type[] genericParameterTypes = method.getGenericParameterTypes();
            for (int i = 0; i < parameterTypes.length; i++) {
                System.out.format(fmt, "ParameterType", parameterTypes[i]);
                System.out.format(fmt, "GenericParameterType", genericParameterTypes[i]);
            }

        }
    }
}
```





-----

将方法运行起来： 要有一个对象！ 和字段同理

`public Ojbect invoke(Object obj,Object ...arg);` 

第一个参数是对象实例，即在那个实例上调用这个方法，后面的可变参数要与方法参数一致，否则报错。 

如果调用的是静态方法，如果获取到的method是一个静态方法，调用静态方法时，无需指定实例对象，即第一个参数为null 

返回的是这个方法返回的值； 

```java
  public static void main(String[] args) throws Exception {
        getMethod() ;
        System.out.println("--------方法测试------------");
        runMethod();
    }
    public static void getMethod() throws Exception {
        Class<Person> t = (Class<Person>) Class.forName("it.cast.bean.Person");
        Method[]  method =t.getMethods() ; //获取的是公有的方法
        Method[]  privateMethod =t.getDeclaredMethods();
        for(Method m: method) {
            System.out.println(m) ;
        }
        System.out.println("---------private-------");
        for(Method m1:privateMethod) {
            System.out.println(m1) ;
        }
    }
    //将方法跑起来
    public static void runMethod() throws Exception{
        Class<Person> t = (Class<Person>) Class.forName("it.cast.bean.Person");
        Method method=t.getMethod("show",null) ;
        Person p=t.newInstance() ;
        method.invoke(p,null) ;
        /**带参的构造函数**/
        System.out.println("-------------------------");
        Constructor con =t.getConstructor(String.class,int.class) ;
        Person per = (Person) con.newInstance("小明",12);//用构造器的构造方法创建对象
        Method m1=t.getMethod("paramMethod",String.class , int.class);
        m1.invoke(per,"小明",33);
    }
```



## 代理

### 代理模式

代理模式（Proxy Pattern）是一种设计模式，提供了对目标对象额外的访问方式，即通过代理对象去访问目标对象，这样可以不修改原目标对象的前提下，提供额外的操作 

- 现实生活中如 中介，代理人，帮甲方做事的行为，都是代理模式的体现

使用代理模式主要有两个目的： 1. 保护目标对象 2.增强目标对象 



代理模式角色分3种：

1. Subject（抽象主题角色）：定义代理类和真实主题的公共对外方法，也是代理类代理真实主题的方法
2. RealSubject ： 真实实现业务逻辑的类
3. Proxy：代理角色，用来代理和封装真实主题。 

代理模式的结构比较简单，核心是代理类，为了让用户端能够一致性地对待真实对象和代理对象，在代理模式中引入了抽象层

  ![img](https://static001.geekbang.org/infoq/79/7980749234585f364651f522f7e45c73.png)  



### 静态代理

客户类： 

```java
package com.wugn.demo;

public class Client {
    public static void main(String[] args) {
        Host host =new Host() ; // 要用房东  
        Proxy proxy =new Proxy(host);  //要用中介，中介里要带有房东 
        proxy.rent() ;
    }

}
```



房东类： 

```java
package com.wugn.demo;

public class Host implements Rent{
    public void rent() {
        System.out.println("房东要租房子");
    }
}

```

中介类： 

```java 
package com.wugn.demo;

public class Proxy implements Rent{
    private Host host ;
    public  String name ;
    public Proxy(){

    }
    public Proxy(Host host) {
        this.host=host;
    }
    public void rent() {
        seeHouse() ;
        host.rent();
        fare() ;
    }
    public void seeHouse(){

        System.out.println("中介带你看房");
    }
    public void fare(){
        System.out.println("中介收中介费");
    }
}

```

租房子（行为） 

```java
package com.wugn.demo;

public interface Rent {
    public void rent() ;
}
```

**静态代理的缺点：**

1. 当需要代理多个类的时候，由于代理对象要实现与目标对象一致的接口 ， 有两种方式
   - 只维护一个代理类，由于这个代理类实现了多个接口，导致代理类过于庞大
   - 新建多个代理类，每个目标对象都对应一个代理类，产生过多代理类
2. 当接口需要增删修改方法时，目标类和代理类都要同时修改，不便于维护

### 动态代理

https://blog.csdn.net/luanlouis/article/details/24589193

> 动态代理的想法是这样的：在运行状态中，需要代理的地方根据subject和RealSubject，动态地创建一个proxy代理对象，用完之后就会销毁，这样就可以避免proxy角色的class在系统中冗杂的问题了。

**java.lang.reflect包中的Proxy类和InvocationHandler接口提供了生成动态代理类的能力**



**动态代理的一大特点就是编译阶段没有代理类，在运行时才生成代理类。** 

Java虚拟机类加载机制有五个阶段：加载、验证、准备、解析、初始化

其中加载阶段需要完成以下三件事：

1. 通过一个类的全限定名来获取定义此类的二进制字节流, .class文件就是二进制文件,只有jvm才能够识别的机器码. 
2. 将这个字节流所代表的==静态存储结构转化为方法区时所运行的数据结构==
3. 在内存中生成一个代表这个类的java.lang.Class 对象,作为方法区这个类的各种数据访问入口

由于虚拟机规范这三点的要求不具体,所以实际的实现很灵活,关于第一点 **获取类的二进制字节流class字节码就有很多途径**

- 从ZIP包获取, JAR,EAR,WAR等格式基础
- 从网络中获取,典型的应用是Applet
- 运行时计算生成,这种场景使用最多的是动态代理技术, 在 `java.lang.reflect.Proxy`类中,就是用了`ProxyGenerator.generateProxyClass`来为特定接口生成形式为`*$Proxy`的代理类的二进制字节流

 ![img](https://img-blog.csdn.net/20140427155129031) 



动态代理就是想办法,根据接口或目标对象,计算出代理类的字节码,然后再加载到JVM中使用, 但是如何计算?(使用各种类库)



其实代理类的处理逻辑很简单，**就是在调用某个方法前后做了一些额外的业务，**换一种思路：在触发invoke真实角色的方法前或之后做一些思路．　



#### InvocationHandler

其实代理类的逻辑很简单，在调用真实业务的前后做一些额外的业务，那么，**可以将所有的触发真实角色的动作交给一个触发管理器，让这个管理器统一管理触发，就是　Invocation Handler** 

动态代理的工作模式时将自己的方法功能实现叫给Invocationhandler来处理,而InvocationHandler则调用具体角色的方法





![img](https://img-blog.csdn.net/20140515134257500?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHVhbmxvdWlz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 

在这个模式下，代理Proxy和RealSubject应该实现相同的功能,这一点非常重要，有两种方式:

1. 定义一个功能接口,proxy和readSubject都实现这个接口(jdk方式
2. 通过继承,Proxy继承RealSubject,  Proxy还可以重写RealSubject的方法, 实现多态(cglib方式) 



如果想为RealSubject这个类创建一个动态代理对象,jdk会做以下工作:

1. 获取RealSubject上的所有接口列表
2. 确定要生成的代理类的类名,默认为"com.sum.proxy.$ProxyXXX"
3. 根据需要实现接口的信息,在代码中动态创建该Proxy类的字节码 
4. 将对应的字节码转换为class对象
5. 创建InvocationHandler实例handler,用来处理Proxy所有方法调用
6. Proxy的class对象以创建的handler对象为参数,实例化一个proxy对象



![img](https://pic4.zhimg.com/80/v2-28223a1c03c1800052a5dfe4e6cb8c53_1440w.jpg)





**InvocationHandler详解**

> 每一个动态代理类都需要实现InvocationHandler这个接口，并且每个代理类的实例都关联到了一个handler。

InvocationHandler这个接口只有一个方法：![image-20220528150412395](D:\Typora\自服务\JAVA进阶.assets\image-20220528150412395.png)

- proxy：我们所代理的对象
- method：我们所要调用的对象的某个方法的对象
- arg：要调用的方法的参数 



### proxy

这个类的作用就是用来动态创建一个代理对象。它提供了许多方法

```java 
//构造实现指定接口的代理类的一个新实例，所有方法会调用给定处理器对象的invoke方法
public static Object newProxyInstance(ClassLoader loader,   Class<?>[] interfaces, InvocationHandler h)
// 返回指定接口的代理类
public static Class<?> getProxyClass(ClassLoader loader, Class<?>... interfaces)
// 用于获取指定代理对象所关联的调用处理器
static InvocationHandler getInvocationHandler(Object proxy)
    返回 cl 是否为一个代理类
static boolean isProxyClass(Class<?> cl) 

```





### CGLIB

> JDK的动态代理类机制有一个缺点：某个类必须要有实现的接口，生成的代理类也只能代理某个类接口定义的方法

而cglib可以通过类继承实现代理。 

CGLIB Code Generation Library，可以在运行时拓展java类和java接口。 

cglib创建某个类A的动态代理模式为：

1. 查找A上所有不是final的public类型的方法定义
2. 将这些方法的定义转换成字节码
3. 将组成的字节码转换成相应的代理class对象
4. 实现MethodInterceptor接口，来处理代理类上所有方法的请求。 





## Idea快捷键

| 动作               | 快捷键                             | 说明                                       |
| ------------------ | ---------------------------------- | ------------------------------------------ |
| Override Methods…  | Ctrl+O                             | 重写基类的方法                             |
| Implement Methods… | Ctrl+I                             | 实现基类或接口中的方法                     |
| Generate…          | Alt+Insert                         | 产生构造方法、getter/setter等方法          |
| Surround With…     | Ctrl+Alt+T                         | 将选中的代码使用if、while、try/catch等包装 |
| Unwrap/Remove…     | Ctrl+Shift+Delete                  | 去除相关的包装代码                         |
| ctrl+delete        | 删除光标所在到单词结尾处的所有字符 |                                            |
| ctrl+backspace     | 删除光标所在到单次开头的所有字符   |                                            |
| ctrl+>             |                                    |                                            |

## 前置知识

学习SpringBoot前置知识

### SSM

Spring+SpringBoot+Mybaits框架

### Servlet

编写一个完善的http服务器，以http/1.1为例，需要考虑：

- 识别正确和错误的http请求
- 识别正确和错误的http头
- 复用tcp连接
- 复用线程
- io异常处理

因此，在javaEE平台上，处理TCP连接，解析http协议这些底层工作都交给了现有的web服务器去做，我们只需要将自己的程序跑在web服务器上，为了实现这个目的，javaEE提供了ServletAPI，我们使用Servlet API 编写自己的Servlet来处理http请求，Web服务器实现ServletAPI几口，实现底层功能。 



**war文件表示的是java Web Application Archive** ，普通的java程序是通过启动jvm，然后执行main方法开始运行，但web应用程序有所不同，无法直接运行war文件，必须先启动web服务器，由web服务器加载我们编写的Servlet，这样就可以处理浏览器的请求了。

常见的Web服务器有：Tomcat，Jetty，等

实际上，Tomcat也是由java编写的，启动Tomcat服务器实际上是启动java虚拟机，执行tomcat的main方法，然后tomcat负责加载我们的war文件，创建一个xxServlet实例，最后以多线程的模式来处理http请求。所以，类似Tomcat这样的服务器又叫Servlet容器

在Servlet容器运行的Servlet有以下特点：

- 无法在代码中直接通过new创建Servlet实例，必须由容器自动创建
- 容器只会给每一个Servlet类创建唯一的实例
- Servlet容器会使用多线程执行doGet()或doPost() 方法

一个WebAPP就是由一个或多个Servlet组成的，每个Servlet通过注解说明自己处理的路径：

```java 
@WebServlet(urlPatterns = "/hello")
可以处理/hello这个路径的请求
public class HelloServlet extends HttpServlet {
    ... 每个浏览器发送请求的时候，还会有请求方法，即GET，POST，PUT等类型，需要复写doGet，doPost方法，如果没有复写，它会直接返回400或405的请求。 
}

```



>  　Servlet是sun公司提供的一门用于开发动态web资源的技术。
>  　　Sun公司在其API中提供了一个servlet接口，用户若想用发一个动态web资源(即开发一个Java程序向浏览器输出数据)，需要完成以下2个步骤：
>  　　1、编写一个Java类，实现servlet接口。
>  　　2、把开发好的Java类部署到web服务器中。
>  　　按照一种约定俗成的称呼习惯，通常我们也把实现了servlet接口的java程序，称之为Servlet 

#### Servlet运行过程

 Servlet程序是由WEB服务器调用，web服务器收到客户端的Servlet访问请求后：
　　**①、**Web服务器首先检查是否已经装载并创建了该Servlet的实例对象。如果是，则直接执行第④步，否则，执行第②步。
　　**②、**装载并创建该Servlet的一个实例对象。
　　**③、**调用Servlet实例对象的init()方法。
　　**④、**创建一个用于封装HTTP请求消息的HttpServletRequest对象和一个代表HTTP响应消息的HttpServletResponse对象，然后调用Servlet的service()方法并将请求和响应对象作为参数传递进去。
　　**⑤、**WEB应用程序被停止或重新启动之前，Servlet引擎将卸载Servlet，并在卸载之前调用Servlet的destroy()方法。



### HttpServletRequest 

https://www.cnblogs.com/xdp-gacl/p/3798347.html

>  　HttpServletRequest对象代表客户端的请求，当客户端通过HTTP协议访问服务器时，HTTP请求头中的所有信息都封装在这个对象中，通过这个对象提供的方法，可以获得客户端请求的所有信息。 

https://www.cnblogs.com/xdp-gacl/p/3789624.html 

### Http协议

https://www.cnblogs.com/xdp-gacl/p/3751277.html

### JSP

Java Server Pages 的缩写，它的文件必须放到/src/main/webapp下，整个文件与html无太大区别

```java
<html>
<head>
    <title>Hello World - JSP</title>
</head>
<body>
    <%-- JSP Comment --%>
    <h1>Hello World!</h1>
    <p>
    <%
         out.println("Your IP address is ");
    %>
    <span style="color:red">
        <%= request.getRemoteAddr() %>
    </span>
    </p>
</body>
</html>
```

包含在<%-- --%>之间的是jsp的注解

包含在<% %>之间的是java代码，可以编写任意的java代码

使用<%= xxx %> 可以快捷输出一个变量的值

jsp内置了几个变量： 

- out：表示HttpServletResopnse的PrintWriter
- session：表示当前HttpSession对象
- request：表示HttpServletRequest对象

Jsp实际上和Servlet没有区别，因为JSP在执行时会被编译成一个Servlet。

Jsp页面还可以通过page指令来引入java类：

``` jsp
<%@ page import="java.io.*"%>
<%@ page import="java.util.*"%>
```



### mvc开发

可以发现：servlet适合编写java代码，但不适合输出复杂的html

jsp适合编写html，但不适合编写复杂的java代码，使用mvc模型可以将两者结合起来

```java 
public class User {
    public long id;
    public String name;
    public School school;
}

public class School {
    public String name;
    public String address;
}
```

在UserServlet中，我们可以通过数据库读取user，school等信息，并把读取到的javabean先放到httpServletRequest中，再通过forward传递给user.jsp处理。在user.jsp中，只需要展示相关javaBean的信息。 

我们把UserServlet看作业务处理逻辑，User看作模型，user.jsp看作渲染，这种设计模式通常被称为mvc



### 微服务

架构风格，一个应用应该是一组小型服务，通过http的方式进行互通

### Serverless

 ![服务器发展](https://segmentfault.com/img/remote/1460000012042637?w=638&h=359) 

无服务器、在行业内，有几种解读方式：某些场景可以解读为一种软件系统架构方法，成为Serverless架构，而某些情况可以代表一种产品形态，成为Serverless产品



在说起Serverless架构时，代表的是

## yaml

### 语法

1. 大小写敏感
2. 使用缩进表示层级关系
3. 缩进不能用tab，只能用空格
4. 缩进的空格数不重要，只需要相同层级元素左对齐
5. #表示注释
6. 字符串不需要加`""` 或者 `''`



### 数据类型

yam支持的数据结构有三种：对象，数组，字面量

####  对象

 键值对集合/哈希对/映射 语法：`key: value`  value前面有空格

```yaml
person:
	name: xiaoming
	age: 18
	pet: dog 
```

#### 数组

包括array ，list， queue

 ```yaml
arr:
- cat
- dog
- goldfish

 ```

#### 示例

```java
//Person.java
@Component
@ConfigurationProperties(prefix="person")
public class Person {
    String name ;
    int age ;
    Date birth;
    pet pe; //宠物的参数
    Boolean marriaged;
    String [] hobbies;
    HashMap<String,Object> score;
    List<String> animal;
    Set<Double> salays;
    HashMap<String,List<pet>> allpets; 
}

```



```yaml
#person 和上面的ConfigurationProperties 中的前缀一样 
person:
  name: xiaoming
  age: 12
  marriaged: false
  birth: 2001/1/1
  #hobbies: [swimming,reading]
  hobbies:
    - swimming
    - reading
    - sss
  animal  : [cat,dog]
#  score:
#    english : 80
#    math : 10
  score : {english: 39, math: 33}
  salays :
    - 999.9
    - 933.3
    - 222.3
  pe :
    name : maomao
    weight : 44
  allpets :
    sick :
      - {name: mao ,weight : 33}
      - name : 猫
        weight: 35
    health:
      - { name: gou , weight : 356}

```

## xml

类似于html的一种标记性语言，但和html不同的是，它用来传输和存储数据，焦点是数据的内容

既然类似于html，那么他们很像，可以自定义标签

用途：

1. 把数据从html分离，通过xml让数据独立存储在xml文件中，可以专注使用html、css进行显示和布局，并确保数据修改不用改动html
2. 简化数据共享
3. 简化数据传输

**xml将数据组织成树结构，DOM解析xml文档并在逻辑上建立一个树模型**

#### 语法

xml必须包含根元素，它是其他元素的父元素。 

所有的xml元素都必须有关闭标签（区别于html）

#### xml属性

xml属性必须加引号，单引号和双引号都可以

如：

```xml
<person sex="female"></person>
<person sex='femal'></person>
```

xml元素 VS 属性

```xml
<person sex="female"> 是属性
<firstname>Beth</firstname>
<lastname>Smith</lastname>
</person>
```

```xml
<person >
<sex>female</sex>  是元素
<firstname>Beth</firstname>
<lastname>Smith</lastname>
</person>
```

应该尽量避免使用属性，如果信息看起来很像数据，就用元素把

## Maven 

一个自动化的构建工具：基于项目对象模型（POM project object model），可以用一小段配置信息来管理项目的构建，报告和文档的软件项目管理工具。 

### 仓库

通过pom.xml的配置，就可以获取到想要的jar包，这些jar包就是从仓库中来的

仓库分为：本地仓库、第三方仓库（私服）、中央仓库

中央仓库： 由maven自己来维护，里面有大量的常用类库，并包含了世界上大部分流行的开源项目构建。

私服一般是公司自己设立的，只为本公司内部共享使用，可以作为共用类库镜像缓存，减少外部的访问和下载的频率（为了减少对中央仓库的访问）



### POM文件 

maven中有两个重要的文件，一个是setting.xml,一个是pom.xml



> POM代表项目对象模型。这是Maven的核心概念。
>
> POM文件使用XML格式来声明项目资源（如依赖项）。

 <project>是`pom`文件的根元素，project下有`modelVersion、groupId、artifactId、version、packaging`等重要的元素。其中，<groupId>、<artifactId>、<version>三个元素用来定义一个项目的坐标，也就是说，一个maven仓库中，完全相同的一组groupId、artifactId、version，只能有一个项目。

- project：整个pom配置文件的根元素，所有的配置都是写在project元素里面的；

- modelVersion：指定了当前POM模型的版本，对于Maven2及Maven 3来说，它只能是4.0.0；

- groupId：这是项目组的标识。它在一个组织或者项目中通常是唯一的。一般被分为多个段，第一段为域，第二段为公司名称，域有org，com（商业组织），cn（中国）等等，

- artifactId：这是项目的标识，通常是工程的名称。它在一个项目组（group）下是唯一的。

  > groupId 和 artifactId统称为坐标，是为了保证项目唯一性提出的，如果你把你的项目搞到maven本地仓库去，找项目的时候就必须根据这两个坐标去查找。

- version：这是项目的版本号，用来区分同一个artifact的不同版本。

- packaging：这是项目产生的构件类型，即项目通过maven打包的输出文件的后缀名，包括jar、war、ear、pom等。

### 文件结构

maven定义了一个标准的目录结构 

```java
- src源代码和测试代码的根目录。
  - main与源代码相关的根目录到应用程序本身，而不是测试代码。
    - java 
    - resources 包含项目所需的资源。
    - webapp
  - test测试源代码。
    - java
    - resources

- target由Maven创建。它包含所有编译的类，JAR文件等。 当执行mvn clean命令时，Maven将清除目标目录。
```

main和test下的` java `目录包含Java代码的应用程序本身是在main和用于测试的Java代码。

`webapp `目录包含Java Web应用程序，如果项目是Web应用程序。

`webapp `目录是Web应用程序的根目录。webapp目录包含` WEB-INF `目录。

如果按照目录结构，你不需要指定你的源代码的目录，测试代码，资源文件等。 

### mvn 命令

`mvn clean`  把编译好的项目所有信息都删掉（target文件夹）

`mvn compile` 编译

`mvn test` 测试

`mvn package` 打包，动态web工程打包成war包，java工程打包成jar包

`mvn install` 把生成的jar包放到本地仓库里 



# java8

## 新特性

### 接口内允许添加默认实现的方法

`default`关键字修饰的函数，不必在实现类中强制实现。  

```java
interface Formula{
    double add(int a,int b) ; 
    default  double sqrt(int s) {
        return Math.sqrt(a) ; 
    }
}
```

使用default关键字可以非常方便地对以前的接口做拓展，而接口的实现类不需要做任何改动。 

## 函数编程

> 面向对象编程是对数据做抽象，函数式编程是对行为进行抽象。

核心思想：使用不可变值和函数，函数对一个值进行处理，映射成另一个值。 

**lambda表达式**

在java中，有一些单方法的接口，即一个接口中只定义了一个方法，如Runnable，Comparator，Callable。

以Comparator为例，我们想要调用sort方法，可以传入一个Compartor实例，用匿名内部类：

```java 
  String []arr = new String[]{"abed","eds","wingchi","hangk","money"};
        Arrays.sort(arr, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o1.compareTo(o2);
            }
        });
```

这个写法可以用java8的lambda表达式替换：

```java 
Arrays.sort(arr, (s1,s2) -> {return s1.compareTo(s2);}) ;
```

lambda格式如下： 

```java 
(parameters) -> expression
或
(parameters) ->{ statements; }

// 1. 不需要参数,返回值为 5  
() -> 5  
  
// 2. 接收一个参数(数字类型),返回其2倍的值  
x -> 2 * x  
  
// 3. 接受2个参数(数字),并返回他们的差值  
(x, y) -> x – y  
  
// 4. 接收2个int型整数,返回他们的和  
(int x, int y) -> x + y  
  
// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
(String s) -> System.out.print(s)
```

参数为（s1,s2)参数类型可以忽略，即由编译器推断出string类。

` ->{}`表示方法体，所有代码写在内部即可。 λ表达式没有class的定义，写法很简洁。

**lambda有三个部分：参数列表，箭头和lambda主体。 lambda是匿名的，它是一种参数，并且可以作为参数传递。** 

**lambda表达式又称为闭包或匿名函数。**

### 函数式接口

不是每个接口都可以缩写lamdba表达式，只有函数式接口`Functional Interface` 才缩写成Lambda表达式的。 函数式接口**就是只有一个抽象方法的接口**，针对该接口类型的所有lambda表达式都会和这个抽象方法匹配。

 为了标识一个接口被定义成了函数式接口，需要为接口添加注解@FunctionalInterface,这样，如果再添加一个函数到接口，编译器就会报错。



```Java
@FunctionalInterface
interface Conveter<F,T>{
    //拿到F，转成T
    public T convert(F src) ;
}
// 看构造的方法，连new都没有。
 Conveter<String,Integer> conveter = (f)-> Integer.valueOf(f);
System.out.println(conveter.convert("12345"));
//`::`运算符可以让构造方法更简单：
Conveter<String,Integer> conveter2= Integer::valueOf; 
```

`::`运算符被用作方法引用。

> 我们使用表达式lambda创建匿名方法， 但有时lambda只是调用了一个已经存在的方法，在这种情况下，方法引用可以让代码更加简洁。 

方法引用的一些语法：

1. 静态方法引用： className::methodName 如 Person::getAge
2. 对象的实例方法引用： InstanceName::methodName ,如 System.out::println
3. 对象的超类方法引用：super::methodName
4. 类的构造器方法引用：className::new ,如 ArrayList::new 
5. 数组构造： typeName[] ::new  ,如 String[]:new 



**示例**

```java 

class Test{
    public static void print(Object obj){
        System.out.println(obj);
    }

    public static void main(String[] args) {
        List<String> list = Arrays.asList("ass","bsd","dlsd") ;
        //调用静态方法 
        list.forEach(Test::print);
    }
}
```



```Java

class Person {
    String firstName;
    String lastName;
    Person() {}
    Person(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
}
interface PersonFactory<P extends Person> {
    P create(String firstName,String lastName) ;
}
class Test{
    public static void main(String[] args) {
        // Person::new能够直接引用Person类型的构造器,然后Java编译器能够
        //根据上下文选中正确的构造器去实现PersonFactory.create()
        PersonFactory personFactory = Person::new;
        Person person = personFactory.create("Peter","parker") ;
    }

}
```

### lambda访问外部变量

如何在lambda表达式中访问外部变量（局部变量，成员变量，静态变量等等），它与匿名内部类房屋内外部变量很相似。 

**局部变量**

在 Lambda 表达式中，我们只能访问外部的 `final` 类型变量

```java 
public class newFeature {
    public static void main(String[] args) throws ParseException {
        final int num=1;
        Conveter<Integer,String> conveter = (form)-> String.valueOf(form+num);
        System.out.println(conveter.convert(12345));
        //num=3 【报错：java: 从lambda 表达式引用的本地变量必须是最终变量或实际上的最终变量】
    }
}
```

num变量不需要显式指定为`final`类型，但必须为隐式的final类型。因为到编译期为止，num对象都是不能改变的。 

- 在lambda内部改变num的值一样不允许：

```Java
int num = 1;
Converter<Integer, String> converter = (from) -> {
	String value = String.valueOf(from + num); // 
	num = 3;
	return value;
};
```



### 访问成员变量和静态变量



### 内置的函数式接口

> jdk1.8 包含了许多内置的函数式接口，如compartor和runnable，java8为他们添加了@FunctionallInteface注解。 



### Predicate断言

抽象方法就是对T进行断言，

```java 

    /**
     * Evaluates this predicate on the given argument.
     *
     * @param t the input argument
     * @return {@code true} if the input argument matches the predicate,
     * otherwise {@code false}
     */
    boolean test(T t);
```



## Stream流

流`java.util.Stream`是对集合对象功能的增强，用于对集合对象进行各种聚合或大批量数据操作。  我们可以对流做中间或终端操作。 中间操作：返回的是流，终端操作：对流结束操作，返回结果。

- 只能对实现了java.util.Collection接口的类做流操作
- Map不支持stream流
- 流支持同步执行，也支持异步执行。

**特点：** 

Stream 会隐式地在内部进行遍历，做出相应的数据转换。
Stream 就如同一个迭代器（Iterator），单向，不可往复，数据只能遍历一次，遍历过一次后即用尽了，就好比流水从面前流过，一去不复返。而和迭代器又不同的是，Stream 可以并行化操作。

- stream不保存数据，只是像流水一样从数据源抓取数据。数据都存储在对应的collection里，需要才生成。

- stream不改变数据源，总是返回新的stream 



### filter过滤

```Java
 List<String> list = Arrays.asList("anb","dls","iej","sddeds","dlsfjis","dls") ;
        long n = list.stream() .filter(x->x.length()<5) .count() ;  
        System.out.println(n);  // 单词长度小于5的单词个数
```

### sorted 

```java 
 public static void main(String[] args) throws ParseException {
        List<String> list = Arrays.asList("anb","dls","iej","sddeds","dlsfjis","dls") ;
        list.stream().sorted().forEach(System.out::println); 
   	  System.out.println(list);	//[anb, dls, iej, sddeds, dlsfjis, dls]
    }
anb
dls
dls
dlsfjis
iej
sddeds
```

sorted不会改变原来的list。 



### Map

中间操作Map能对List中的每一个元素做处理

```java
List<String> list = Arrays.asList("anb","dls","iej","sddeds","dlsfjis","dls") ;
        list.stream().sorted().map(String::toUpperCase).forEach(System.out::println); //将每个单词变成大写
```

### Match匹配

返回一个boolean类型

# 接口

## 幂等性



幂等方法可以使用相同参数重复执行，并能获得相同结果。这些函数不会影响系统的状态，因此不用担心重复执行会对系统造成改变。例如：

1. 前端重复提交选中的数据，后台也只会产生对应这个数据的一个反应结果
2. 用户发起一笔付款请求，就只扣一次钱。即使遇到网络重发或系统bug重发请求
3. 发送验证短信也应该只发一次，同样的验证短信不会多发



幂等性需要解决的场景：

1. 前端重复提交表单
2. 用户恶意进行刷单
3. 接口超时重复提交
4. 消息进行重复消费



### Http的幂等性

GET，Head，Option，天然有幂等性

PUT：用于更新资源，可能产生不幂等

Delete：删除资源，可能不幂等

POST：用于添加资源，多次提交也可能有副作用，也应该满足幂等性。 





## 鲁棒性

接口的鲁棒性取决于它对异常的承载能力。

如果一个接口严重依赖外部输入的合法性以及第三方服务的正确性，一旦外部输入非预期的内容，或者依赖的第三方接口奔溃，该接口就会出现未知问题，那就没有鲁棒性

异常主要包括：

1. 输入异常
2. 流程异常
3. 性能异常





