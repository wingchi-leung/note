# Spring

容器框架。包含、管理功能

 ![spring overview](https://docs.spring.io/spring-framework/docs/4.3.x/spring-framework-reference/html/images/spring-overview.png) 

**对应springFramwork/lib中的release包** 

Test ： Spring的单元测试模块

Core Container： 核心容器IOC，黑色代表这部分的功能由哪些jar包组成，要使用该功能必须进行导包

```java
四个核心包，基本上都要用到
spring-beans-5.2.9.RELEASE.jar
spring-context-5.2.9.RELEASE.jar
spring-core-5.2.9.RELEASE.jar
spring-expression-5.2.9.RELEASE.jar
```

AOP+Aspects ： 面向切面编程

```java 
spring-aop-5.2.9.RELEASE.jar
spring-aspects-5.2.9.RELEASE.jar
```



### 基本概念

#### POJO 

`Plain Ordinary Java Object`

pojo 一个简单的java类， 这个类没有实现/继承任何特殊的java接口或类，不遵循任何主要的java模型，约定或者框架的java对象，在理想的情况下，pojo不应该有注解 

#### javaBean

 JavaBean是符合一定规范编写的Java类 它的方法命名，构造及行为必须符合特定的约定：

1. 所有属性为private。

2. 这个类必须有一个公共的缺省构造函数。即是提供无参数的构造器。
3. 这个类的属性使用getter和setter来访问，其他方法遵从标准命名规范。
4. 这个类应是可序列化的。实现serializable接口。 

**两者区别**

1. POJO其实是比javabean更纯净的简单类或接口。POJO严格地遵守简单对象的概念，而一些JavaBean中往往会封装一些简单逻辑。
2. POJO主要用于数据的临时传递，它只能装载数据， 作为数据存储的载体，而不具有业务逻辑处理的能力。
3. Javabean虽然数据的获取与POJO一样，但是javabean当中可以有其它的方法。

#### wiring

创建应用组件之间协作的行为通常被称为装配wiring， Spring 有多种装配bean的方式如 xml，java方式 

#### AOP

aspect-oriented programming 面向切面编程允许你把遍布应用各处的功能分离出来形成可重用的组件。 

>  AOP可以说是OOP（Object Oriented Programming，面向对象编程）的补充和完善。OOP引入封装、继承、多态等概念来建立一种对象层次结构，用于模拟公共行为的一个集合。不过OOP允许开发者定义纵向的关系，但并不适合定义横向的关系，例如日志功能。日志代码往往横向地散布在所有对象层次中，而与它对应的对象的核心功能毫无关系对于其他类型的代码，如安全性、异常处理和透明的持续性也都是如此，这种散布在各处的无关的代码被称为横切（c ross cutting），在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。 

 AOP把软件系统分为两个部分：**核心关注点**和**横切关注点**



#### MVC框架

- M ：业务模型

- V：用户界面 
- C ：控制器

使用MVC的目的就是将M和V实现的代码分离，从而是同一个程序可以使用不同的表现形式



#### IOC 容器

Inversion Of Control ： 控制反转

> 前提： Java面向对象，对象有方法和属性，那么就需要对象实例来调用方法和属性（即实例化）;

> Spring 容器在初始化时先读取配置文件,根据配置文件或者元数据创建与组织对象存入容器中,程序使用再用从IOC容器中取出需要的对象。 

IoC：指程序开发中，类的实例的创建不再由调用者管理，而是由Spring容器创建，Spring容器会负责控制程序之间的关系，而不是由程序代码直接控制，因此控制权由程序代码转移到了Spring容器中，即发生反转。

#### Spring 配置 

>        Spring配置文件是用于指导Spring工厂进行Bean生产、依赖关系注入（装配）及Bean实例分发的"图纸"。Java EE程序员必须学会并灵活应用这份"图纸"准确地表达自己的"生产意图"。Spring配置文件是一个或多个标准的XML文档，applicationContext.xml是Spring的默认配置文件，当容器启动时找不到指定的配置文档时，将会尝试加载这个默认的配置文件。

以下示例显示了基于XML的配置元数据的基本结构：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">  
        <!-- collaborators and configuration for this bean go here -->
    </bean>
    <!-- more bean definitions go here -->
</beans>
```

#### 依赖注入

Dependency Injection（DI）

容器能知道哪一个组件（类）运行时，需要另一个类，并通过反射的形式，将容器中准备好的对象注入

### classpath

classpath指的是:

`src/main/java`

`src/main/resources`

`src/main/webapp`

由此可知，如果在main文件夹下新建一个文件properties，那么classpath又多了一个

src/main/properties

 ![img](https://img-blog.csdn.net/20171017234100175?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2V1ZG9uZ25hbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) 







#### pom.xml文件

已经导入依赖 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>january_01</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!--dependency 写好后，maven会自动下载所需要的包-->

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.7.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.7.RELEASE</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
    </dependencies>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

</project>
```



一个主流的JAVA Web开发框架，轻量级的应用框架。 

**以IoC(Inverse of Control，控制反转)和 AOP（Aspect Oriented Programming 面向切面编程）为内核。** 



在实际开发中，通常的服务器端采用了三层体系结构：表现层（web)， 业务逻辑层（service） ，持久层（dao）

 ![Spring的体系结构](http://c.biancheng.net/uploads/allimg/190606/5-1Z606104H1294.gif)   

## bean实验

![image-20210904113432385](D:\Typora\自服务\javaWeb.assets\image-20210904113432385.png)

1. 根据bean的类型从IOC容器中获取bean的实例

```java
   person bean = applicationContext.getBean("person2"); // 要求容器中只能一个person对象，否则不知道要哪个，报错
person bean = applicationContext.getBean("person2",person.class); //给出名字和person类名，不需要强转
```

2. 通过p名称空间为bean复制

   名称空间：在xml中名称空间是用来防止标签名重复的

## Bean



### bean标签的属性

<id>

id 用来唯一标识bean；只有一个；不能用特殊字符 ：x#@ 在bean引用时只能用id指向你需要的bean；(bean的对象名)

<name>

name定义的是bean的别名，可以有多个，并且可以和其他bean重名；一个bean可以用多个名称 ：name = "bean1,bean2,bean3" 

<property>

property用于调用Bean实例中的setter方法完成对属性的赋值，该元素有name，ref，value子元素 

子属性： `name:`属性名 ；`type=""` 引号为类型 

**class**

bean对象所对应的全限定名；

全限定名： 包名+类名 

**alias**

起个别名 ，可以同等替换 name是本名， alias是新起的别名 

```xml 
<alias name="xiaoming" alias="小红的同桌"/>
User user = (User) context.getBean("小红的同桌");
User user = (User) context.getBean("xiaoming");
```



### Bean作用域 

bean作用域用于确定那种类型的bean实例应该从Spring容器中返回给调用者

https://blog.csdn.net/kongmin_123/article/details/82048392

| 作用域      | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| singleton   | 单实例：在spring IoC容器仅存在一个Bean实例，Bean以单例方式存在，bean作用域范围的默认值。在容器启动完成之前就创建好对象，并保存在容器中。 |
| prototype   | 多实例：**每次从容器中调用Bean时，都返回一个新的实例，即每次调用getBean()时，相当于执行newXxxBean()。** |
| request     | 每次HTTP请求都会创建一个新的Bean，该作用域仅适用于web的Spring WebApplicationContext环境。 |
| session     | 同一个HTTP Session共享一个Bean，不同Session使用不同的Bean。该作用域仅适用于web的Spring WebApplicationContext环境。 |
| application | 限定一个Bean的作用域为`ServletContext`的生命周期。该作用域仅适用于web的Spring WebApplicationContext环境。 |

```xml
通过指定scope可以确定它的作用域
<bean id="books" class="bean.Book" scope="prototype"></bean>
```





### xml装配bean

提供了2种注入方式：构造器注入和属性注入

**构造器注入** 

```java
class BraveKnight implements Knight {
    private Quest quest ;
    public BraveKnight (Quest quest) {
        this.quest = quest ;  //Quest 被注入进来 
    }
    pulbic void embarkOnQuest() {
        quest.embark() ;
    }
}
```

在构造时把探险任务作为构造器参数传入，是依赖注入的方式之一即 构造器注入 （constructor injection)

松耦合的好帮手

- 使用无参构造创建对象 （默认）

- 当需要使用带参的构造器有以下方式：

构造函数+参数索引 : ==索引从0开始==

```xml
    <bean id="xiaoming" class="com.wing.User">
        <constructor-arg index="0" value="xiaoming"/>
    </bean>
```

构造函数+函数名称

```xml
 <bean id="xiaoming" class="com.wing.User">
        <constructor-arg name="name" value="xiaoming"/>
    </bean>
```

构造函数+参数类型匹配

使用`type`属性显式指定构造函数参数的类型，则容器可以使用简单类型的类型匹配。

==注意：可以看到，如果是包装类一定要带包名！== 

不推荐： 因为如果有一个构造参数，有两个String类型的参数，引起歧义，会导致错误发生

```xml
    <bean id="xiaoming" class="com.wing.User">
        <constructor-arg type="java.lang.String" value="小明"/>
        <constructor-arg type="int" value="12"/>
    </bean>
```



**Settter**注入

在调用无参构造函数或无参数static工厂方法以实例化bean之后在你的bean上调用setter方法来完成的。

**①、在xml文件中为不同类型的类成员赋值**

```java
package bean;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.Properties;

public class person {
    //基本类型用直接property标签赋值，自动进行类型转换
    private int  age;
    private String name ;
    private String gender;
    private Car car ;
    private ArrayList<Book> bookList;
    private Map<String,Object> maps ;
    private Properties  properties;
//... 
}

```

**②、给自定义对象赋值**

```xml
<bean id="car1" class="bean.Car">
        <property name="carName" value="小明的爱车"></property>
        <property name="price" value="999"></property>
        <property name="color" value="green"></property>
    </bean>



<bean id="person6" class="bean.person">
        <property name="name" value="小明"></property>
        <property name="age" value="19"></property>
<!--        给Car赋值，有两种方式，1.使用引用ref指向前面声明car，一定是同一个实例-->
        <property name="car" ref="car6"></property>
        <property name="car">
<!--        2. 在里面声明一个bean，内部bean，无法通过id获取-->
            <bean class="bean.Car">
                <property name="carName" value="小明的爱车"></property>
                <property name="price" value="999"></property>
                <property name="color" value="green"></property>
            </bean>
        </property>
    </bean>
```

**③、给list对象赋值**

```xml
 <bean id="person6" class="bean.person">
        <property name="bookList">
<!--            相当于bookList=new ArrayList<Book>()-->
            <list>
<!--内部bean方式-->
                <bean id="hlm" class="bean.Book" >
                    <property name="bookName" value="红楼梦"></property>
                    <property name="author" value="吴承恩"></property>
                </bean>
 <!--ref方式，注意ref方式的写法-->
                <ref bean= "book2"></ref>
            </list>
        </property>
    </bean>
```

```java 
 public void demo(){
        person persons = (person) applicationContext.getBean("person6") ;
        ArrayList<Book> books = persons.getBookList();
        System.out.println(books.toString());    
     //报错：hlm内部bean，id相当于无效
        Book book = (Book) applicationContext.getBean("hlm");
        System.out.println(book)
    }
```

**④、给map赋值**

```xml
    <bean id="person6" class="bean.person">
        <property name="maps">
<!--            maps=new LinkedHashMap<>();-->
            <map>
                <entry key="key01" value="咸蛋黄"></entry>
                <entry key="car" value-ref="car1"></entry>
                <entry key="car2">
                    <bean class="bean.Car">
                        <property name="carName" value="亚斯娜"></property>
                        <property name="price" value="77"></property>
                    </bean>
                </entry>
            </map>
        </property>
    </bean>
```

**⑤、给properties赋值**

```xml
 <property name="properties">
<!--            相当于 properties = new Properties()所有的k：v都是string类型-->
            <props>
<!--                因为都是string类型的，value直接写在标签体中-->
                <prop key="password">123456</prop>
                <prop key="username"> root</prop>
            </props>
        </property>
```

**⑥、用util名称空间创建集合类型的bean，方便重复引用**

1、引入util命名空间

```xml
xmlns:utils="http://www.springframework.org/schema/util"

//配置util的应用资源 xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/util
  http://www.springframework.org/schema/util/spring-util-4.0.xsd
"
```

2、使用

```xml
 
    <utils:map id="mmp"> <!--    相当于new LinkedHashMap()，下面就写元素了-->
        <entry key="key01" value="咸蛋黄"></entry>

        <entry key="car2">
            <bean class="bean.Car">
                <property name="carName" value="亚斯娜"></property>
                <property name="price" value="77"></property>
            </bean>
        </entry>
    </utils:map>

<bean id="person6" class="bean.person">
    根据id使用
        <property name="maps" ref="mmp"></property>
    </bean>

```



**⑦、级联属性赋值：就是属性的属性，例如：person-car-carname** 

```xml
    <bean id="car1" class="bean.Car">
        <property name="carName" value="小明的爱车"></property>
        <property name="price" value="999"></property>
        <property name="color" value="green"></property>
    </bean>

    <bean id="person6" class="bean.person">
        <property name="car" ref="car1"></property>
        <property name="car.price" value="199"></property>
    </bean>
```

**⑧、配置继承**

`parent`属性当前bean的配置信息继承于另一个bean

```xml
  <bean id="person6" class="bean.person">
        <property name="name" value="小明"></property>
        <property name="age" value="19"></property>
        <property name="gender" value="南"></property>
    </bean>
    <bean id="person7" class="bean.person" parent="person6">
        <property name="name" value="李建军"></property>
    </bean>
```

`abstract:true` 属性： 指定为true后，该配置的bean就是专门被继承的，相当于一个模板的bean

```xml
  <bean id="person6" class="bean.person" abstract="true">
        <property name="name" value="小明"></property>
        <property name="age" value="19"></property>
        <property name="gender" value="南"></property>
    </bean>
创建person6会报错
org.springframework.beans.factory.BeanIsAbstractException: Error creating bean with name 'person6': Bean definition is abstract

```

**⑨、通过工厂**

静态工厂：工厂本身不用创建对象，通过静态方法调用。 对象=工厂类.工厂方法名

实例工厂：工厂本身需要创建对象

工厂类 工厂对象 = new  工厂类（）

工厂对象.getAirplane()

```java
public class airInstnaceFactory {
    public  airPlane getAriPlane(String pliotName){
        airPlane plane = new airPlane() ;
        System.out.println("实例工厂在为你造飞机");
        plane.setPilotName(pliotName);
        plane.setLength(3000);
        plane.setNumOfCarry(500);
        plane.setName("波音777");
        return plane;
    }
}
```

 

```java
public class airStaticFactory {

    public static airPlane getAriPlane(String pliotName){
        airPlane plane = new airPlane() ;
        System.out.println("airStaticFactory..正在为你造飞机");
        plane.setPilotName(pliotName);
        plane.setLength(3000);
        plane.setNumOfCarry(500);
        plane.setName("波音777");
        return plane;
    }
}
```

```xml
<!--    静态工厂的用法，不是创建工厂，使用factory-method标签 -->
    <bean id="airPlane1" class="factory.airStaticFactory"
        factory-method="getAriPlane">
        <constructor-arg value="rick-ollie"></constructor-arg>
    </bean>

<!--    实例工厂的用法,要先创建工厂对象
    1. 先创建工厂
    2.创建要生成的对象,并指定要生产它的工厂
        1.Factory-bean
        2.Factory-method-->
    <bean id="instanceFactory" class="factory.airInstnaceFactory"></bean>
    <bean id="airPlane2" class="bean.airPlane"
        factory-method="getAriPlane"
        factory-bean="instanceFactory">
        <constructor-arg value="wingchileung"></constructor-arg>
    </bean>
```

**⑩、实现FactoryBean的工厂**

FactoryBean是Spring规定的一个接口，只要是这个接口的实现类，spring都认为是一个工厂



```java
package factory;
import bean.Book;
import org.springframework.beans.factory.FactoryBean;
import java.util.UUID;
/**
 * 使用spring提供的工厂类
 * 1.编写类，实现该接口
 * 2.在spring中注册
 */
public class factoryBeanImpl implements FactoryBean <Book>{
    /**
     * getObject：工厂方法
     * 返回创建的对象
     *
     * @return
     * @throws Exception
     */
    @Override
    public Book getObject() throws Exception {
        System.out.println("spring工厂帮你造对象。。");
        Book book = new Book();
        book.setBookName(UUID.randomUUID().toString());
        return book;
    }

    /**
     * 返回创建的对象的类型
     * spring会自动嗲用这个方法来确认创建的对象的类型
     * @return
     */
    @Override
    public Class<?> getObjectType() {
        return null;
    }

    /**
     * 是单例吗？
     * false：不是
     * true：是
     * @return
     */
    @Override
    public boolean isSingleton() {
        return true;
    }
}

```



```xml
<!--   配置这个工厂时，会自动调用实现类的getObject()函数，返回要生产的对象的实例，而不是工厂对象
ioc容器启动时候不会创建实例，在需要时才创建       -->
    <bean id="springFactory" class="factory.factoryBeanImpl">
    </bean>
```

### bean的生命周期

spring bean的完整生命周期从创建spring容器开始，直到最终spring容器销毁bean，这其中包含了一些关键节点：

 ![img](https://images0.cnblogs.com/i/580631/201405/181453414212066.png) 

 ![img](https://images0.cnblogs.com/i/580631/201405/181454040628981.png) 

实例化-->属性赋值-->初始化-->销毁



### 数据库连接池

> 数据库连接池（Connection pooling）是程序启动时创建足够的数据库链连接，并将这些连接组成一个连接池，由程序
>
> 传统连接：java应用程序访问数据库的过程时：
>
> 1. 装载数据库驱动程序
> 2. 通过jdbc建立数据库连接
> 3. 访问数据库，执行sql语句
> 4. 断开连接
>
> 使用数据库连接池的过程：
>
> 1. 程序初始化时创建连接池
> 2. 使用时向连接池申请可用连接
> 3. 使用完毕将连接返回给连接池
> 4. 程序退出时断开所有连接，释放资源



常见的数据库连接池有：C3P0，DBCP，Proxool

C3P0 ：是一个开放源代码的JDBC连接池，它在lib目录中与Hibernate一起发布,包括了实现jdbc3和jdbc2扩展规范说明的Connection 和Statement 池的DataSources 对象。 比较耗资源，效率比较低

Proxool：比较推荐，



数据库连接池作为单实例时最好的，一个项目就一个连接池，连接池里面管理许多连接。 用Spring管理连接池

**Spring配置连接池**

1. 用maven依赖导入包

   ```xml
     <dependency>
         <groupId>c3p0</groupId>
         <artifactId>c3p0</artifactId>
         <version>0.9.1.2</version>
       </dependency>
       <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>8.0.16</version>
       </dependency>
   ```

2. 配置数据库连接池

   ```xml 
      <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <property name="user" value="root"></property>
           <property name="password" value="ss123000"></property>
           <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/test"></property>
           <property name="driverClass" value="com.mysql.cj.jdbc.Driver"></property>
       </bean>
   ```

   测试：

   ```java 
    //main方法中：
   DataSource dataSource = applicationContext.getBean("dataSource", DataSource.class);
   
   System.out.println(dataSource.getConnection()); // 报错，原因不明确
   ```



**②、引用外部配置文件**

在spring中，配置文件一般使用.properties文件来保存，然后再在配置文件中动态应用值：

1. 创建dbconfig.properties 文件

   ```java 
   jdbc.username=root
   jdbc.password=ss123000
   jdbc.jdbcUrl=jdbc:mysql://localhost:3306/test
   jdbc.driverClass=com.mysql.cj.jdbc.Driver
   ```

2. 然后在xml中使用context命名空间，并配置：

   ```xml
     <context:property-placeholder location="classpath:config.properties"/>
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <property name="user" value="${jdbc.username}"></property>
   <!--        ${key}动态取出配置文件中某个key对应的值-->
           <property name="password" value="${jdbc.password}"></property>
           <property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>
           <property name="driverClass" value="${jdbc.driverClass}"></property>
       </bean>
   ```

3. 测试通过

### SpEL测试

Spring Expression Language Spring表达式语言

①、字面量赋值

②、运算符

`<property name="age" value="#{2021-1999}"></property>`

③、静态方法、实例方法

调用静态方法的语法：

```xml 
#{T(全类名).静态方法(arg1,arg2)} ;
        <property name="email" value="#{T(java.util.UUID).randomUUID().toString().substring(0,5)}"></property>

```

实例方法：` #{对象id.对象属性} `或

`#{对象id.对象方法(arg1,arg2..)}`

https://www.jianshu.com/p/e0b50053b5d3

### 自动装配Bean

传统的Spring做法是使用.xml文件来对bean进行注入或者配置aop,事物,这么做有两个缺点

1. .xml文件非常庞大(所有内容都要配置)
2. 开发中在.java, .xml文件之间不断切换很麻烦

为了解决这两个问题,Spring引入了注解,通过@xxx的方式,使得注解和java bean 紧密结合



Spring从两个角度实现自动化装配

- 组件扫描 component scanning 
- 自动装配 autowiring 

基于**xml**的自动装配

xml标签里的`autowire`属性取值有`byName，byType，constructor ，default，no`



`byName `方式：在XML配置文件中beans的auto-wire属性中设置为byName， 他将尝试将他的属性与文件中定义为相同名称的beans进行匹配和连接，如果找到匹配项，它将注入beans，否则将抛出异常。

需要保证所有的bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致 

```xml
   <bean id="cat" class="com.wing.Cat"/>
        <bean id="dog" class="com.wing.Dog"/>
        <bean id="person" class="com.wing.Person" autowire="byName">
                <property name="cat" ref="cat"/>
        </bean>
```

`byType`需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性一致，如果有多个该Type的，会报错。 没找到，装配null

```xml
   <bean id="cat" class="com.wing.Cat"/>
        <bean id="dogsss" class="com.wing.Dog"/>
        <bean id="person" class="com.wing.Person" autowire="byType">
<!--            在自动在容器上下文中查找，和自己对象属性类型相同的bean -->
               <property name="name" value="小明"/>
        </bean>
```

`constructor` 方式

1. 先按照有参构造器的参数方式进行装配，没有就直接装配null
2. 如果按照类型找到了多个，参数的名作为id继续匹配，如果没有直接装配null
3. 不会报错



### 注解装配bean

**通过注释分别创建Dao、Service、Controller（控制器：控制网站跳转逻辑的Servlet）**

**Spring中的四个注解：某个类中加入任何一个注解都能快速地将这个组件加入到ioc容器的管理中。**

1. @Controller：控制器，给控制器层的组件添加
2. @Component ：组件，成分，不属于其他三层的组件添加 
3.  @Service ： 业务逻辑，给业务逻辑层的组件添加该注解
4. @Repository ：  给数据库层（持久化层dao层的组件添加

使用注解将组件快速加入到容器中的步骤：

1. 给要添加的组件上标四个注解的任意一个

2. 依赖context命名空间，自动扫描加了注解的组件

   在xml中配置：

   ​	

   ```xml
   <!--    base-package：指定要扫描的基础包,spring会自动扫描该包下的类并发现带有注解的类-->
       <context:component-scan base-package="com.wingchi"></context:component-scan>
   ```

3. 导入aop包，支持注解模式 

   在pom文件中导入依赖

   ```xml
   <dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-aop</artifactId>
   			<version>5.1.9.RELEASE</version>
   </dependency>
   ```

```java 
package com.wingchi.servlet;

import org.springframework.stereotype.Controller;

@Controller
public class bookServlet {
}

```

1. **组件的id，默认就是组件的类名，首字母小写**
2. 组件的作用域，默认就是单实例的

```java 
//改变组件的id名称
@Repository("bookdao1")
//改变组件的作用域
@Scope("prototype")

public class bookDao {
}

```

**②、排除 | 指定扫描的某个类**

下面展示了排除扫描的某一个类 `context:exclude-filter`   `context:include-filter`

```xml
    <context:component-scan base-package="com.wingchi">
<!--        扫描的时候可以排除一些不要的组件
            type="annotation" 指定排除规则，按照注解进行排除，标注了指定注解的组件不要
            expression：注解的全类名
            type:                                         expression
            assignable:指定排除某个具体类，按照类排除         类的全类名
            aspectj ： aspectj表达式
            custom ：  自定义一个TypeFilter，实现接口来决定那些使用
            regex  ：  正则表达式
            -->
        <context:exclude-filter type="regex" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
```

### 泛型依赖注入

泛型注入利用了java 继承和多态的特性

**书城实验：**

Service层：

有BookService<book>，UserService<user>，baseService， 其中baseService是前者的父类，

```java 
public class baseService <T>{
    @Autowired
    baseDao<T> basedao  ;
    public void save() {
        System.out.println("自动注入的dao "+basedao);
        basedao.saveDao();
    }
}


@Service
public class bookService extends baseService<books> {

}

```

Dao层：

有baseDao，userDao<user>，bookDao<Book>  其中baseDao是父类，有抽象方法save() ，两个子类重写了该方法。

```java 
public abstract class baseDao<T> {
    public abstract  void saveDao();

}


@Repository
public class userDao extends baseDao<user>{
    @Override
    public void saveDao() {
        System.out.println("保存用户 userDao..");
    }
}
```



## 注解

 ![图片](https://mmbiz.qpic.cn/mmbiz_png/PMZOEonJxWcAT6EsUDOiatUOf1nesAqfs9aR2elJWUJvp6ibockInIrcNj1jPTW4NamvGZl4InuG0O6cffuNMtFA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1) 



### web

@Controller：组合注解（组合了@Component）

@RestController： 复合注解，相当于@Controller+@ResponseBody 的组合。

@GetMapping，@PostMapping，@PutMapping，@DeleteMapping

@ResponseBody：支持将返回值放在response内，而不是一个页面，通常用户返回json数据

@PathVariable：用于接收路径参数





### @Value

Value注解可以注解在字段、方法、参数、注解上。 



```java
package com.entity;



import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component("cat")
public class Animal {

    @Value("kitty")
    private String name ;
    @Value("wingchi")
    private String ownerName ;
    @Value("cat")
    private String species ;
}

```

```java 
Animal cat = applicationContext.getBean("cat", Animal.class);
System.out.println(cat.getName());
```

配置文件中只需要配置自动扫描和注解即可。 

### @Autowired 

使用@Autowired注释可以装**配在构造器，属性，方法上面**，来表示让Spring自动为该属性赋值。

使用@AutoWired来消除set、get方法

- @Autowired是通过byType的方式（xml中的atuowired属性 ）

- Autowired注解，默认是一定要装配上，否则就会报错，可以通过Autowired的属性required来指定是否需要一定装配

https://blog.csdn.net/u013257679/article/details/52295106/

### @Qualifier

当创建多个具有相同类型的bean并且无法根据属性名匹配id时，可以使用Qualifier来指定一个名字作为id 

```java 
public class BookServlet {
//    Qualifier让Spring根据bookServiceExt作为id去匹配
    @Qualifier("bookServiceExt")
    @Autowired
    private bookService onYourtun;

    public void doGet() {
        onYourtun.bookSave();
    }
}

```

**要记住默认情况下，spring会将类名（首字母小写）作为id放在容器中**



### @Resource

@Autowired，@Resource ，@Inject都是装配的意思

@Autowired：最强大，Spring的注解

@Resourece：是java自己的注解 ，扩展性更强，如果切换其他框架，依然可以使用，Autowired可能不可以用。 

### @Nullable

 `Nullable`  字段标记了这个注解，说明了这个字段可以为null 

```java
  public void setName(@Nullable String name) {
        this.name = name;
    }
```

```java
// 如果显示定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空@Autowired(required=false)  
```

### @Required 

@Required注释应用于bean属性中的setter方法，它表明受影响的bean属性在配置时必须放在xml配置文件中，否则容器就会抛出异常。 

### @Component

标注一个类为Spring容器的Bean，使用@Component注解到一个类上，表示此类标记为Spring容器中的一个Bean 

相当于配置文件中的：

`<bean id="" class=""/>` 

#### 衍生注解

 ![img](https://images2015.cnblogs.com/blog/659572/201606/659572-20160629101345374-1626956838.png) 

@Component 有几个衍生注解，在web开发中，按照mvc三层架构分层有：

- dao --- @Repository 
- service -- @Service 
- controller  -- @Controller 

这三个注解的功能和Component 的功能是一样的，标记为Spring管理的bean 只是在不同层的名称不同 

#### @Value

在类方法上使用了Component可以使用vaule 表达式通常用来获取bean的属性或者调用某个方法 （注入属性） 

```java
        @Component
        public class User {
            @Value(value="哈哈")
            public String name;
```

### javaConfig实现配置类

创建javaConfig类的关键在于添加@Configuration注解用来表明这个类是一个配置类

```java
        @Configuration
        public class AppConfig {
            @Bean 
            public User getUser() {
                return new User();
            }
        }
```



#### @Bean 

@Bean注解是个方法级别上的注解，主要用在@Configuration注解的类里，

默认情况下，bean的ID与带有@Bean注解的方法名是一样的。 如果想设置一个不同的名字，可以重命名 

```java

        @Configuration
        public class config {
            @Bean
            public Person person (){
                return new Person("小明",12) ;
            }
            //Bean的名字就是龙猫
         //相当于 pet 龙猫 = new Pet("猫");
            @Bean("龙猫") 
            public pet tomcatPet() {
                return new pet ("猫");
            }
```



```java
	public class Test {
            public static void main(String[] args) {
                ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
                User user = context.getBean("xiaoming",User.class);
                System.out.println(user.name);
            }
        }
```

### getBean()方法

我们可以通过ApplicationContext 的getBean方法来获取Spring容器中已初始化的bean

`getBean(String name)` // bean的id或者name,且要求id或者name在ioc容器中都不能重名

`getBean(Class <T> type)` //如果该类型没有继承任何父类（Object除外） 和实现接口的话，要求该类型的bean 在ioc容器也是唯一的 

```xml
        <bean class="com.bean.Person">
        <property name="name" value="张三"/>
        </bean>

        <bean class="com.bean.Person">
        <property name="name" value="李四"/>
        </bean>

        //只能通过类型来查找bean（但类型重复，会抛出异常）
        @Test
        public void testPerson()
        {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        Person p = ctx.getBean(Person.class);
        System.out.println(p);
        }
```

`getBean(String name, Class<T> type)`

`getBean(String name , Object [] args)`

### getBeanNamesForType(Class type)

type ：指定的类的Class示例 

返回： 指定类型的所有的JavaBean的名称



##  AOP

Aspect Oriented Programming 面向切面编程

Spring框架一般都基于AspectJ实现AOP操作 

AspectJ 一个独立的AOP框架，一般把AspectJ和Spring框架一起使用，进行AOP操作

![无标题-2022-06-14-1133](D:\Typora\自服务\Spring.assets\无标题-2022-06-14-1133.png)

### AOP术语

> 类比:抄表员抄城市电表

**①. 通知(Advice)** 切面有目标---它必须完成的工作,在AOP的术语中,切面的工作被称为通知 ,**通知定义了切面是什么以及何时使用**

Spring切面可以应用5种类型的通知

- 前置通知(before) 在目标方法调用之前通知功能
- 后置通知(after) 在目标方法完成后调用通知,此时不会关心方法的输出是什么
- 返回通知After-returning :在目标方法成功执行之后再通知
- 异常通知(After-throwing) :在目标方法抛出异常后调用通知
- 环绕通知(Around):通知包裹了被通知的方法,在被通知的方法调用前和调用后执行 

**②. 连接点 Join Point**

连接点是应用执行过程中能够插入切面的一个点,这个是可以调用方法时,抛出异常甚至是修改一个字段时,切面代码可以利用这些点插入到应用的正常流程中,并添加新的行为. **切面类的方法可以在这个位置(连接点)上和目标方法进行连接** （相当于一个（被切面地）类中有多少个函数，每一个都有可能被增强） 

**③.切点 Pointcut**

**一个切面并不需要通知应用的所有连接点 ,可以选择某部分连接点真正切入**

如果说通知定义了切面是"什么" 和"何时" 的话, 切点就定义了"何处" ,切点的定义会匹配通知要织入的一个或多个连接点. 我们通常使用明确的类或方法名称

**在众多连接点中选择出感兴趣的切点** （相当于一个被切面类选中的方法，不是所有都会被选中） 

**切入点表达式:把感兴趣的连接点表达出来**

**④.Aspect切面**

在实际应用中，切面通常是指封装的用于横向插入系统功能的类。

**⑤.引入Introduction**

引入允许我们向现有的类添加新方法和属性,

**⑥.织入Weaving**

织入是切面应用到目标对象并创建新的代理对象的过程. 切面在指定的连接点被织入到目标对象中,在目标对象的生命周期中有多个点可以进行织入

- 编译期:需要特殊的编译器
- 类加载器: 在目标对象被加载到jvm时织入,这种方式需要特殊的类加载器 
- 运行期:切面在应用运行的某个时刻被织入,一般情况下,在织入切面时,AOP容器会为目标对象动态地创建一个代理对象. 

>  通知包含了需要用于多个应用对象的横切行为；**连接点是程序执行过程中能够应用通知的所有点**；**切点定义了通知被应用的具体位置（在哪些连接点）**。其中关键的概念是切点定义了哪些连接点会得到通知。 

### Spring的AOP

>  并不是所有的 AOP 框架都是相同的，它们在连接点模型上可能有强弱之分。有些允许在字段修饰符级别应用通知，而另一些只支持与方法调用相关的连接点。它们织入切面的方式和时机也有所不同。 



Spring中提供了4种类型的AOP支持

- 基于代理的经典SpringAOP
- 纯POJO切面
- @AspectJ注解驱动的切面
- 注入式AspectJ切面

**Spring通知是java编写的**

Spring所创建的通知都是标准的java类编写的,而定义通知所应用的切点通常会使用注解或spring配置文件采用xml编写

**Spring在运行时通知对象**

**SPring只支持方法级别的连接点**



使用AOP的步骤:

1. 导包
2. 配置
3. 测试

导入的包有:

```xml
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aspects</artifactId>
			<version>5.1.9.RELEASE</version>
		</dependency>
上面一个是基础包,下面三个是增强包
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>com.springsource.org.aspectj.weaver</artifactId>
			<version>1.6.8.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.aopalliance</groupId>
			<artifactId>com.springsource.org.aopalliance</artifactId>
			<version>1.0.0</version>
		</dependency>
		<dependency>
			<groupId>net.sourceforge.cglib</groupId>
			<artifactId>com.springsource.net.sf.cglib</artifactId>
			<version>2.1.3</version>
		</dependency>

```

引入aop的命名空间

```xml 
<beans
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation=" 
                           // 这里的东西是成对的!!
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
">
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```



### ProxyFactoryBean

> 在Spring中创建AOP代理的基本方法是使用org.springframework.aop.framework.ProxyFactoryBean。这可以完全控制切入点、任何适用的advice以及它们的顺序。但是，如果你不需要这样的控制，可以选择更简单的选项。

### 切入点表达式

作用：说明对那个类里面的那个方法进行增强

语法：`execution ([权限修饰符] [返回类型] [类全路径] [方法名称] ([参数列表]))`

例如：

1. 对com.atguigu.dao.BookDao类中的add方法进行增强

`execution(* com.atguigu.dao.BookDao.add(..))`// *代表都可以

2. 对com.atguigu.dao.BookDao类中的所有方法进行增强

``execution(* com.atguigu.dao.BookDao.*(..))``

3. 对com.atguigu.dao.BookDao类中的所有类，所有方法进行增强

   `execution(* com.atguigu.dao *.*(..))`
   
4. execution (* concert.Performance.perform(..)) 

   	         任意权限符（不写就是） | 返回任意类型 | 方法所属的类 | 方法  | 任意参数



### 例子

> 计算器

```java 
package com.wingchi.inter;
//接口类
public interface calculator {

    int add(int x,int y) ;
    int sub(int x,int y) ;
    int mul(int x,int y) ;
    int div(int x,int y) ;
}

```



```java
package com.wingchi.impl;

import com.wingchi.inter.calculator;
import org.springframework.stereotype.Service;
//实现类
@Service
public class myCalculator implements calculator {

    @Override
    public int add(int x, int y) {
        return x+y;
    }

    @Override
    public int sub(int x, int y) {
        return x-y;
    }

    @Override
    public int mul(int x, int y) {
        return x*y;
    }

    @Override
    public int div(int x, int y) {
        return x/y;
    }
}

```



日志类：

```java
package com.wingchi.logUtil;

import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Component
//告诉Spring这是一个切面
@Aspect
public class log {

    /**
     * 告诉Spring每个方法再什么时候运行
     */
    @Before("execution(public int com.wingchi.impl.myCalculator.*(int,int))")
    public void logStarat(){
        System.out.println("the method is going to start..");
    }
    @After("execution(public int com.wingchi.impl.myCalculator.*(int,int))")
    public void logEnd(){
        System.out.println("the method is going to end..");
    }
    @AfterReturning("execution(public int com.wingchi.impl.myCalculator.*(int,int))")
    public void logReturn(){
        System.out.println("the method is going to return..");
    }
    @AfterThrowing("execution(public int com.wingchi.impl.myCalculator.*(int,int))")
    public void logError(){
        System.out.println("the method is going to error..");
    }
}

```

测试：

```java 
package com.aop.demo;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.Test;
import com.wingchi.impl.myCalculator;
import com.wingchi.inter.calculator;

public class AopApplication {
	static ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
	@Test
	public static void main(String[] args) {
		calculator bean = applicationContext.getBean(calculator.class) ;
		bean.add(1,4) ;
  //      class com.sun.proxy.$Proxy22
//com.wingchi.impl.myCalculator@6150c3ec
		System.out.println(bean.getClass());
		System.out.println(bean);
        //实际上它是代理类
	}

}

```

### 环绕通知

```java 
   public void logAround() {
        try{
//            前置通知
            method.invoke(obj, args);
//            返回通知
        }catch ( Exception e) {
//          异常通知
        }finally {
//            结束通知
        }
    }
}

```

这四合一就是环绕通知

```java 
  @Around("myPointcut()")
    public Object logAround(ProceedingJoinPoint proceedingJoinPoint)  {

        Object [] args=  proceedingJoinPoint.getArgs();
//        就是例用反射调用目标方法即可，就是method.invoke(obj,args)
        Object proceed = null;
        try {
            logStarat(proceedingJoinPoint);
//            十分重要，它是通过反射帮我们调用方法的
            proceed = proceedingJoinPoint.proceed(args);
            logReturn(proceedingJoinPoint,proceed);
        } catch (Exception e) {
            logError(e);
            e.printStackTrace(); 
          
            throw new RuntimeException(e) ;
        } catch (Throwable throwable) {
            throwable.printStackTrace();
        } finally {
            logEnd(proceedingJoinPoint);
        }

        return proceed;
    }
```



**环绕通知优先于普通通知执行，原因是一下这句代码，要先执行它，其他普通通知才开始。**

```java
proceed = proceedingJoinPoint.proceed(args);
```

环绕前置--普通前置--目标方法执行--环绕正常返回/通知异常---环绕后置--- 普通后置----普通返回

如果同时有环绕通知和普通的前置后置通知，环绕通知会先执行，当函数抛出异常时，也会首先被环绕通知catch住，所以，要想让普通通知也能发现异常，在catch后还要继续抛出去

### JoinPoint

连接点，封装了当前目标方法的详细信息

```java
    @Before("myPointcut()") 
// 获取方法的参数 joinPoint.getArgs() 
// 获取方法的签名：joinPoint.getSignature().getName()
    public void logStarat(JoinPoint joinPoint) {
        Object[] args = joinPoint.getArgs();
        System.out.println(joinPoint.getSignature().getName() + "开始了 ，参数列表是： " + Arrays.asList(args));
    }

// 获取方法的返回值：returning="res" , 
    @AfterReturning(value = "myPointcut()", returning = "res")
    public void logReturn(JoinPoint joinPoint, Object res) {

        System.out.println("the method is going to return.." + " 返回值为：" + res);
    }
//获取方法抛出的异常， throwing = "e" 
  @AfterThrowing(value = "myPointcut()", throwing = "e")
    public void logError(Exception e) {
        System.out.println("the method is going to error.." + e);
    }
```



### 多切面运行顺序



如果同一个方法有两个切面切入，那么两个切面中的普通通知、环绕通知的执行顺序？

```java 
package com.wingchi.logUtil;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

import java.util.Arrays;

@Component
@Aspect
@Order(1)
public class ValidateAspect {

    /**
     * 告诉Spring每个方法再什么时候运行
     */
    @Before("com.wingchi.logUtil.log.myPointcut()")
    public void logStarat(JoinPoint joinPoint) {
        Object[] args = joinPoint.getArgs();

        System.out.println("切面二的切入。。。。"+joinPoint.getSignature().getName() + "开始了 ，参数列表是： " + Arrays.asList(args));
    }


    @AfterReturning(value = "com.wingchi.logUtil.log.myPointcut()", returning = "res")
    public void logReturn(JoinPoint joinPoint, Object res) {

        System.out.println("切面二的切入。。。。"+"the method is going to return.." + " 返回值为：" + res);
    }



    @After("com.wingchi.logUtil.log.myPointcut()")
    public void logEnd(JoinPoint joinPoint) {

        System.out.println("切面二的切入。。。。"+joinPoint.getSignature().getName() + " 方法结束了");
    }

    @AfterThrowing(value = "com.wingchi.logUtil.log.myPointcut()", throwing = "e")
    public void logError(Exception e) {
        System.out.println("切面二的切入。。。。"+"the method is going to error.." + e);
    }

}

```

通知切面：先出现的切面后出去，后出现的切面先出去。 环绕通知优先普通通知是对同一个切面而言的。











## 声明式事务

事务就是一组由于逻辑上紧密关联的而合并成一个整体的多个数据库操作（增删查改） ，这些操作要么全部执行，要么都不执行

ACID特性：原子性，一致性，隔离性，持久性。 

Spring的声明式事务把事务管理代码的固定模式作为一种横切关注点，通过AOP模块化，进而借助AOP框架来实现声明式事务进行管理

Spring的事务切面叫做事务管理器。



https://www.cnblogs.com/caoyc/p/5632198.html



#### 事务管理器

- PlatformTransactionManager 接口

  - TransactionStatus getTransaction(Transaction definition) 
  - void commit (TransactionStatus  status )
  - void rollback(TransactionStatus  status) 

  

就是一个事务切面。

当标注在类上，表示类中的所有方法都进行事务处理。

只能应用到**public方法上**，对其他非public方法，如果标记了@Transactional也不会报错，但该方法没有事务功能。 

@Transactional注解：

| Modifier and Type | Optional Element and Description            | 解释                   |
| :---------------- | :------------------------------------------ | ---------------------- |
| `Isolation`       | `isolation`The transaction isolation level. | 事务的隔离性           |
| `Class[]`         | noRollbackFor                               | 指定那些类不需要回滚   |
| `String[]`        | noRoolbackForClassName                      | 指定类的全限定名       |
| `Propagation`     | propagation                                 | 事务的传播行为         |
| `boolean `        | readOnly                                    | 指定事务为只读事务     |
| `Class[]`         | rollbackFor                                 | 指定那些类需要回滚     |
| `String`          | rollbackForClassName                        | 指定类的全限定名       |
| `int`             | timeout                                     | 设置超时（以秒为单位） |

①、超时timeout

`@Transactional(timeout =3)`

②、readOnly ，default:false 

如果该事务只有读操作，使用readOnly 可以进行事务优化，加快查询速度，不用管事务的操作

③、noRollbackFor

 异常分类：

1. 编译异常（检查异常，默认不回滚
2. 运行时异常（非检查异常），默认回滚

假设事务中出现：ArithmeticException异常 

`i=10/0` 会出现异常： 

指定出现该异常不回滚：

```java 
@Transactional(noRollbackFor = {ArithmeticException.class})
```

④、rollbackFor

和上面的同理



#### 事务的隔离性

事务并发的问题：

1. 脏读：一个事务还没有提交时，它的变更就可以被别的事务看到，即事务可以读取未提交的数据，也叫做脏读

2. 不可重复读：一个事务先后读取同一条记录，但两次读取记录不同，称为不可重复读。

3. 幻读：

   A第一次查询所有数据，结果只有一行。  B在这中间插入了一条新数据

   A再查询所有数据，结果变成两行数据，就像出现了幻觉一样。

   ==解决：使用间隙锁==

MYSQL数据库事务的隔离级别： 

1. 读未提交（脏读，幻读，不能重复读）

2. 读已提交（避免脏读、不可以避免不可重复读）

   事务所作修改在没提交之前，对其他事务是不可见的。

3. 可重复读 **（MYSQL默认）** 保证同一事务多次读取同一个数据的结果是相同的。

4. 序列化

| 隔离级别 | 脏读   | 不可重复读 | 幻读   |
| -------- | ------ | ---------- | ------ |
| 读未提交 | 可能   | 可能       | 可能   |
| 读提交   | 不可能 | 可能       | 可能   |
| 可重复读 | 不可能 | 不可能     | 可能   |
| 串行化   | 不可能 | 不可能     | 不可能 |



**事务隔离级别:**

　　@Transactional(isolation = Isolation.READ_UNCOMMITTED)：读取未提交数据(会出现脏读, 不可重复读) 基本不使用
　　@Transactional(isolation = Isolation.READ_COMMITTED)：读取已提交数据(会出现不可重复读和幻读)
　　@Transactional(isolation = Isolation.REPEATABLE_READ)：可重复读(会出现幻读)
　　@Transactional(isolation = Isolation.SERIALIZABLE)：串行化

每个数据库厂商支持的隔离级别不同：

　　MYSQL: 默认为REPEATABLE_READ(可重复读）级别
　　SQLSERVER: 默认为READ_COMMITTED （读已提交）



**mysql相关**

**查看mysql的隔离级别：**

Select @@global.transaction_isolation; //查看全局隔离级别



Select @@transaction_isolation;  同上（当前会话

**修改mysql的隔离级别 ：**

Set [Session|Global] transaction isolation  level {READ UNCOMMITTED | READ COMMITTED, REPEATABLE-READ}



**实验1：在mysql操作读脏数据**：读到了未提交的数据。 

右边的一回滚，左边的发生了脏读。 

<img src="D:\Typora\自服务\Spring.assets\image-20211006190117602.png" alt="image-20211006190117602" style="zoom:150%;" />

9.9就是那个脏数据

**实验二、在mysql实验不可重复读**

![image-20211006191404387](D:\Typora\自服务\Spring.assets\image-20211006191404387.png)

右边的没有读到脏数据，但是发生了不可重复读：没有修改但是两次读的结果不一样。

**实验三：mysql可重复读避免所有问题** 

![image-20211006192127419](D:\Typora\自服务\Spring.assets\image-20211006192127419.png)

左边隔离级别为可重复读，在会话中即使右边的已经提交了，左边的都不会变化。 

又叫快照读。 



**需要隔离的事务，一般都是一边读，一边写的时候才会出问题，如果全部都是读，不修改，或者全部都是修改，没有读（mysql底层自动给它排队）就不会出现问题**

mysql实验三：两边都在写数据

![image-20211006192938631](D:\Typora\自服务\Spring.assets\image-20211006192938631.png)

#### 事务的传播性

伪代码表示：

```java
public void methodA() {
    methodB() ;
    doSth ;
    methodC() ;
}
@Transactional (Propagation = XXX) 
public void methodB() {
    // dosomething
}
@Transactional (Propagation = XXX) 
public void methodC() {
    // dosomething
}
  代码中A方法嵌套调用B方法，其中如果B方法中抛出异常，或者是C方法抛出异常，对应的A方法，B方法是否需要回滚？ 
```



| 事务传播行为类型          | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| PROPAGATION_REQUIRED      | 如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。这是Spring的默认方式。 |
| PROPAGATION_SUPPORTS      | 支持当前事务，如果当前没有事务，就以非事务方式执行。         |
| PROPAGATION_MANDATORY     | 使用当前的事务，如果当前没有事务，就抛出异常。               |
| PROPAGATION_REQUIRES_NEW  | 新建事务，如果当前存在事务，把当前事务挂起。                 |
| PROPAGATION_NOT_SUPPORTED | 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。   |
| PROPAGATION_NEVER         | 以非事务方式执行，如果当前存在事务，则抛出异常。             |
| PROPAGATION_NESTED        | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。 |

🍤@Transactional(propagation=Propagation.REQUIRED) ：我们平常使用@Transactional注解的默认行为。 如果当前有事务, 那么加入该事务, 没有的话新建一个，即：

- 如果外部方法没有开启事务，Propagation.REQUIRED修饰的内部方法会新开自己的事务，且开启的事务相互独立。
- 如果外部方法开启事务并且被Propagation.REQUIRED修饰，所有的内部方法和外部方法都属于同一个事务，只要一个方法回滚，则整个事务回滚。

## spring中的模式

- 工厂模式

  Spring容器本质上是个大工厂，使用工厂模式通过BeanFactory，ApplicationContext创建Bean对象

- 代理模式：Spring中的Bean默认就是单例的，这样有利于容器对Bean的管理。

- 模板模式：Spring中jdbcTemplate，RestTemplate等以Template结尾的对数据库，网络等进行操作的模板类，就是用了模板模式

- 适配器模式：SpringMVC中用到了适配器模式适配Controller

- 策略模式：Spring中的Resource接口，它的不同实现类，会根据不同策略去访问资源。 

- 单例模式：Spring中的Bean默认都是单实例的，这样有利于容器对Ｂｅａｎ的管理

- 观察者模式：Spring事件驱动模型就是观察者模式很经典的一个应用。



**Spring容器启动阶段会干什么？**

容器启动阶段和Bean实例化阶段。



**Bean的生命周期**

大致分成4个阶段：实例化，属性赋值，初始化，销毁

























