## SpringBoot

### 前言

springboot2升级：和java8结合 响应式编程和Servelt编程(左和右) 

 <img src="https://spring.io/images/diagram-reactive-1290533f3f01ec9c57baf2cc9ea9fa2f.svg" alt="img" style="zoom: 25%;" /> 



- 创建独立的spring应用
- 内嵌web服务器
- 自动start依赖，简化构建配置
- 提供生产级别的监控，健康检查以及外部化配置
- 无代码生成，无需写xml 



### pom文件

pom.xml 文件默认有两个模块，`spring-boot-starter` (包括配置支持，日志和yaml) 和 `spring-boot-starter-test` 测试模块 

spring-boot-starter-*  *代表某种场景

只要引入starter 这个场景需要的常规依赖都将自动导入

**默认包扫描规则**

**主程序所在的包及其下面的子包都会被自动扫描** 

想要改变扫描路径，需要在主程序的注释中添加

`@SpringBootApplication(scanBasePackages="com.atguigu")` 指定扫描的包 

或者由 `ComponentScan`指定 ： 

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.wingchi")
```

#### 依赖管理特性

SpringBoot中的pom文件中有一个重要的父项目，（maven的父项目用于做依赖管理）

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
        // 表示继承自这个父项目
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.5.5</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>demo</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>1.8</java.version>
            	<mysql.version>5.0.43</mysql.version>
	</properties>
    上面的parent标签中定义了版本号，下面的依赖都不需要指出版本号，SpringBoot有一个自动版本仲裁机制。如果想要修改，先查看SpringBoot parent标签里的定义的key值。然后在上面的properties标签手动赋值。 如mysql.version 
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```

#### 场景启动器

> Starters are a set of convenient dependency descriptors that you can include in your application. You get a one-stop shop for all the Spring and related technologies that you need without having to hunt through sample code and copy-paste loads of dependency descriptors. For example, if you want to get started using Spring and JPA for database access, include the `spring-boot-starter-data-jpa` dependency in your project.
>
>  `spring-boot-starter-*`, where `*` is a particular type of application. 

Starter是一组SpringBoot提供的依赖标识符,SpringBoot的场景启动器都以“spring-boot-start-*开头，

*指的是某一种具体的场景。



### 底层注解

#### @Configuration和@Bean

- **@Configration 注解**：声明当前类是一个配置类，相当于 **Spring** 中的一个 **XML** 文件
- **@Bean 注解**：作用在方法上，声明当前方法的返回值是一个 **Bean**



使用：MyConfig.java 声明一个配置类：

```java 
package com.example.demo.config;

import com.example.demo.bean.Person;
import com.example.demo.bean.Pet;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableMBeanExport;

/**
 * 1.配置类里使用@Bean标注在方法上给容器注册组件，默认是单实例的
 * 2.配置类本身也是组件
 * 3.在SpringBoot2.0后， Configuration中的属性ProxyBeanMethods默认是true的,也就是说这个配置类是代理类
 * 组件依赖
 * 4.Full((proxyBeanMethods = false) 全模式 ，当被其他组件引用时，就一定能确定该组件是在容器中的，但每次实例化一个组件时，都要去IOC容器中查看该组件是否存在，所以程序启动会变慢 
 *   Lite((proxyBeanMethods = true )轻量级依赖
 最佳实践：
 配置类组件之间没有依赖关系的使用Lite模式加速容器启动过程，减少判断
 配置类组件之间有依赖关系，方法会被调用得到之前单实例组件，使用Full模式。
 */
@Configuration(proxyBeanMethods = true )
public class MyConfig {

    /**
     * 外部无论对这个方法调用多少次，得到的类都是一样的，单实例的
     * @return
     */
    @Bean // 使用@Bean给容器添加组件，方法的名称就是组件在容器中的Id,返回值就是组件,返回的类型就是组件的类型
    public Person Person1() {
        return new Person("杭肯",19);
    }

    @Bean(name = "tomdog") // 给这个组件自定义名称id
    public Pet pet(){
        return new Pet("tomcat") ;
    }
}

```



```java
public static void main(String[] args) {
		ConfigurableApplicationContext ioc = SpringApplication.run(DemoApplication.class, args);
    //查看IOC中有多少Bean，这些都是SpringBoot自动给我们配置的 
		String[] beans = ioc.getBeanDefinitionNames();
//		for(String s:beans){
//			System.out.println(s);
//		}
		MyConfig config =ioc.getBean(MyConfig.class) ;
		//该配置类是个代理对象com.example.demo.config.MyConfig$$EnhancerBySpringCGLIB$$1f4cb150@18fdb6cf
		System.out.println(config);

//		如果@Configuration(proxyBeanMethods = true )那么代理对象调用方法时,SpringBoot会检查这个组件是否在容器中存在，
//		保证容器单实例。
		Pet pet1= config.pet() ;
		Pet pet2=config.pet() ;
		System.out.println(pet1 == pet2); //true
	}

```



#### @RestController 

注解相当于 `@ResponseBody` + `Controller` 合在一起的作用

Spring MVC 是通过一个叫做DispatcherServlet前端控制器的来拦截请求的。而在Spring Boot中使用自动配置把前端控制器配置到框架中 

 ![/users请求流程](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_restful2.png) 

> https://www.cnblogs.com/fishpro/p/spring-boot-study-restcontroller.html#_label0



####  @RequestMapping 

> @RequestMapping
>
> RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。
>
> RequestMapping注解有六个属性，下面我们把她分成三类进行说明。

1. value：   指定请求的实际地址，指定的地址可以是URI Template 模式（后面将会说明）；

   method： 指定请求的method类型， GET、POST、PUT、DELETE等；

默认RequestMapping("....str...")即为value的值；

```java
   @RequestMapping("/hello")
    public String handle (){
        return "Hello ,Spring Boot" ;
    }
```

 

#### @Import 

该注解的主要作用是用来导入配置类（Configruation）或者一些需要前置加载的类

@SpringBootApplication默认扫描主类所在的包，如果需要扫描父类包中的配置，需要在ComponentScan中指定，也可以使用*Import来导入*

Import支持的三种方式： 

1. 带有@Configuration的配置类（4.2版本之前只可以导入配置类，4.2的也可以导入普通类）
2. ImportSelector的实现
3. ImportBeanDefinitionRegistrar的实现 

```java
/*写在配置类文件里面*/
/*SpringBoot调用无参构造器创建出Person的组件*/
@Import(Person.class)
@Configuration
public class config {
//    @Bean
//    public Person person (){
//        return new Person("小明",12) ;
//    }
    @Bean("龙猫")
    public pet tomcatPet() {
        return new pet ("猫");
    }

}
```

思考： 使用Import导入的普通类，和再上面代码中注释掉的手动配置的类，有什么区别？ 

做测试：

```java 
        String [] beanNamesForType  =run.getBeanNamesForType(Person.class) ;
        for(int i=0;i<beanNamesForType.length;i++){
            System.out.println(beanNamesForType[i]) ;
            if(i>0) {
                System.out.println("相同? " + beanNamesForType[i]==beanNamesForType[i-1]) ;
            }
        }

/** 输出：（没能理解）
com.atguigu.boot.bean.Person
person
false   
*/
```



#### @Conditional

作用是根据某个条件创建特定的Bean，通过实现Condition 接口，满足Conditional指定的条件，则进行组件注入 

Conditional及其派生注解： 

![image-20210121180427941](D:\Typora\自服务\javaWeb.assets\image-20210121180427941.png)

@ConditionalOnBean（仅仅在当前上下文中存在某个对象时，才会实例化一个Bean）
@ConditionalOnClass（某个class位于类路径上，才会实例化一个Bean）
@ConditionalOnExpression（当表达式为true的时候，才会实例化一个Bean）
@ConditionalOnMissingBean（仅仅在当前上下文中不存在某个对象时，才会实例化一个Bean）
@ConditionalOnMissingClass（某个class类路径上不存在的时候，才会实例化一个Bean）
@ConditionalOnNotWebApplication（不是web应用）

例子1：

```java 
//当这个组件存在时，这个配置类下的所有组件才能生效
@ConditionalOnBean(name = "xxx") 
@Configuration
public class MyConfig {

 
    @Bean
    public Person Person1() {
        return new Person("杭肯",19);
    }
//当Person1在容器中时，才会有Pet这个组件
    @ConditionalOnBean(name = "Person1")
    @Bean(name = "tomdog") // 给这个组件自定义名称id
    public Pet pet(){
        return new Pet("tomcat") ;
    }
}

```

 

#### @ImportResource

SPringBOot默认使用配置类来配置Bean，但如果已经写好了xml文件，需要在配置类中导入spring的xml配置文件（不用再写多一个配置类）



```
//当这个组件存在时，这个配置类下的所有组件才能生效
@ConditionalOnBean(name = "xxx") 
@Configuration
@ImportResource("classpath:beans.xml")
public class MyConfig {
}

```



#### @ConfigurationProperties

配置绑定：将properties文件的属性导入到JavaBean中。

- 只有在容器中的组件，才会有SpringBoot提供的支持。

![image-20210127155843069](D:\Typora\自服务\javaWeb.assets\image-20210127155843069.png)

该注解有一个prefix属性，通过指定的前缀绑定配置文件中的配置，可以放到类上或者方法上。

使得javabean和配置文件中的属性绑定起来 ，可以看到还需要Bean注解或者Configuration注解

##### 示例

```java
//Car.java
@Component
@ConfigurationProperties (prefix = "car")
public class Car {
    private String brand ;
    private Integer price;

    public Integer getPrice() {
        return price;
    }

    public String getbrand() {
        return brand;
    }

    public void setbrand(String brand) {
        this.brand = brand;
    }

    public void setPrice(Integer price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "hrano='" + brand + '\'' +
                ", price=" + price +
                '}';
    }
}

```

配置文件： 

```properties
//Application.properites 
#配置汽车的信息
car.brand=BYD
car.price=100000
```

测试：

```java
/*返回的数据给浏览器*/
@RestController
public class HelloController {

    @Autowired
    Car car ;

    @RequestMapping("/car")
    public Car getCar() {
        return car;
    }
}

```

参考： https://www.cnblogs.com/tian874540961/p/12146467.html

#### @EnableConfigurationProperties

- 上面的第二种方法是使用这个注解： 

@EnableConfigurationProperties注解的作用是：使使用 @ConfigurationProperties 注解的类生效。

在Car上标注@ConfigurationProperties ，但没有标注@Component，那这个类还是没有得到SpringBoot的支持，这个时候可以*使用在配置类中使用@ConfigurationProperties来1.开启这个类的配置绑定功能，2.将这个Car类注入到容器中。*

**说明：**

如果一个配置类只配置@ConfigurationProperties注解，而没有使用@Component，那么在IOC容器中是获取不到properties 配置文件转化的bean。

说白了 @EnableConfigurationProperties 相当于把使用  @ConfigurationProperties 的类进行了一次注入。



### 自动配置源码

https://juejin.cn/post/6988877227794366495

https://segmentfault.com/a/1190000021941617

问题：SpringBoot是如何实现自动配置的？

关键注解；@SpringBootApplication，这个注解是几个注解的集合，**其中关键的就是这三个：** 

**@SpringBootConfiguration**标识这是一个配置类

**@EnableAutoConfiguration**：SpringBoot特有的注解，用于加载spi定义的类
**@ComponentScan**定义扫描的路径，默认从注解所在类的package进行扫描



 <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/514867d473614b5c8bbb56403fa0efc2~tplv-k3u1fbpfcp-watermark.awebp" alt="image.png" style="zoom: 25%;" /> 

⭐EnableAutoConfiguration：它是自动配置**最重要**的一个注解，它借助@Import注解，动态的注册特定场景的bean定义。 

```java 
//   可以看到@EnableAutoConfiguration又标注了@Import和@AutoConfigurationPackage两个注解。 
@AutoConfigurationPackage  //通过主程序得所在的包名进行批量注册
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
}
```

接下来分析Import和AutoConfigurationPackage两个注解： 

1. AutoConfigurationPackage （自动配置包）源码：

```java
@Import({Registrar.class})
public @interface AutoConfigurationPackage 
  通过Import 给容器注册了一系列组件。 
```

2. @Import({AutoConfigurationImportSelector.class})

>   在找SpringBoot自动配置实现逻辑的入口方法前，我们先来看下`AutoConfigurationImportSelector`的相关类图，好有个整体的理解。看下图： ![preview](https://segmentfault.com/img/remote/1460000021941620/view) 
>
>  可以看到`AutoConfigurationImportSelector`重点是实现了`DeferredImportSelector`接口和各种`Aware`接口，然后`DeferredImportSelector`接口又继承了`ImportSelector`接口。 
>
>  我们会去关注`AutoConfigurationImportSelector`复写`DeferredImportSelector`接口的实现方法`selectImports`方法，因为`selectImports`方法跟导入自动配置类有关，而这个方法往往是程序执行的入口方法。经过调试发现`selectImports`方法很具有迷惑性，`selectImports`方法跟自动配置相关的逻辑有点关系，但实质关系不大。 

```Java 
 public String[] selectImports(AnnotationMetadata annotationMetadata) {
        if (!this.isEnabled(annotationMetadata)) {
            return NO_IMPORTS;
        } else {
            //getAutoConfigurationEntry方法获取自动配置相关的东西 
            AutoConfigurationImportSelector.AutoConfigurationEntry autoConfigurationEntry = this.getAutoConfigurationEntry(annotationMetadata);
            return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
        }
    }
```

```Java 
  AutoConfigurationImportSelector.AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
        if (!this.isEnabled(annotationMetadata)) {
            return EMPTY_ENTRY;
        } else {
            AnnotationAttributes attributes = this.getAttributes(annotationMetadata);
            // 获取所有候选配置 
            List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
            //对所有候选配置作删除修改的操作 
            configurations = this.removeDuplicates(configurations);
            Set<String> exclusions = this.getExclusions(annotationMetadata, attributes);
            this.checkExcludedClasses(configurations, exclusions);
            configurations.removeAll(exclusions);
            configurations = this.getConfigurationClassFilter().filter(configurations);
            this.fireAutoConfigurationImportEvents(configurations, exclusions);
            return new AutoConfigurationImportSelector.AutoConfigurationEntry(configurations, exclusions);
        }
    }
```

实际上，SPringBoot在一启动，就利用给容器中批量导入一些组件。 

`List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());`从META-INF/spring.factories位置来加载一个文件，将配置好的组件都导入到SPringBOOT容器中。

spring-boot-autoconfigure-2.3.4.RELEASE.jar包里面

加载完成后， SpringBOOT会按需加载。

### 最佳实践

- 引入场景依赖

  1. 在SPringBoot官网中，有各种场景依赖得参考。

     https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.build-systems.starters

- 查看自动配置配置了什么

  1. 在properties文件中加上： `debug=true` 开启自动配置报告，打印自动配置类生效，那些没有。

- 是否需要修改 
  
  - 参照文档修改配置项https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties.core

实验：改变SPringBOOt启动得Banner图。

在类路径下放一张图片：然后在项目properties文件中指定好，名字要改成banner.xxx  

![image-20211128200647605](D:\Typora\自服务\SpringBoot.assets\image-20211128200647605.png)

### lombok

用于简化javaBean的开发

第一步：引入依赖。

```xml
     <dependency>
     <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```

然后在bean中标注Lombok的属性：

```java 
@Data  //添加getter和setter 
 @ToString // 添加toString方法
@AllArgsConstructor //添加全参数构造器
@NoArgsConstructor // 添加无参构造器
public class Person {
    private String name ;
    private Integer age ;
}

```


### dev-tool
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency> 
```

### yaml文件

学习不同java类型在yaml中的配置，在SPring开发中，首选yaml配置

```java 
//这个注解将bean和yaml中的配置绑定起来，指定前缀是person 
@ConfigurationProperties(prefix = "person" )
@Component
@Data
@ToString
public class Person {
    private String name ;
    private Integer age ;
    private Pet pet ;
    private boolean hasHouse ;
    List<String> interest ;
    private String [] parents;
    private Date birth ;
    private Map<String,Object> socre ;
    private Set<Double> salary ;
    private Map<String,List<Pet>> allPets ;
}

```

```java 
@Data
@ToString
@AllArgsConstructor
@NoArgsConstructor
public class Pet {
    private String name ;
    private Double weight ;
}
```



```yaml 
person:
  name: zhangsan
  age: 19
  birth: 2011/11/23
#  数组类和List都的可以用[],也可以用-
#  interest: [篮球,足球]
  interest:
    - 篮球
    - 足球
  parents: [张三爸,张三妈]
#  socre:
#    englist: 80
#    math: 90
  socre: {english:90,math:90}
#  set类的也一样。
  salary: [99.99,100.0]
  pet:
    name: 阿狗
    weight: 10.0
#    分成健康的宠物和生病了的宠物
# Map<String,List<Pet>> 写Map就是kv 只不过v是数组，就按照数组的方式来写即可，用 []或者-
  allPets :
    sick :
      - {name: 阿狗 , weight: 10.0 }
    health: [{name: tomcat ,weight: 3} ,{name: tomcat, weight : 11.2 }]


```

yaml自动提示开启：

引入依赖：

```xml&#39; 
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-configuration-processor</artifactId>
</dependency>
```

重启几下，就可以有提示了



### 错误处理

>  By default, Spring Boot provides an `/error` mapping that handles all errors in a sensible way, and it is registered as a “global” error page in the servlet container. For machine clients, it produces a JSON response with details of the error, the HTTP status, and the exception message. For browser clients, there is a “whitelabel” error view that renders the same data in HTML format (to customize it, add a `View` that resolves to `error`). 

- 默认下，springboot提供了/error映射来处理所有的请求
- 对于机器客户（如postman），它返回的是json数据（包含错误细节和http状态码，异常信息等）
- 对于浏览器客户，有一个默认的白页视图，返回的数据和机器客户一样。
- 如果想要定制化错误处理，可以添加解析error视图的解析器

定制化错误页面：Custom Error Pages

>  you can add a file to an `/error` directory. Error pages can either be static HTML (that is, added under any of the static resource directories) or be built by using templates. The name of the file should be the exact status code or a series mask. 在资源路径下增加error目录，放对应的错误页面

实验：

![image-20211218215513286](D:\Typora\自服务\SpringBoot.assets\image-20211218215513286.png)

定制化后，当出现404或5xx后，就会出现这些页面：

<img src="D:\Typora\自服务\SpringBoot.assets\image-20211218215555231.png" alt="image-20211218215555231" style="zoom: 25%;" />

#### 错误源码分析

https://www.cnblogs.com/mzq123/p/11967963.html

🌴 SpringBoot的异常自动配置类是ErrorMvcAutoConfiguration.java

- 配置了 DefaultErrorAttributes的默认组件  id=errorAttributes, *定义错误页面中包含那些数据*
- 配置了 BasicErrorController basicErrorController的默认组件，id=basicErrorController; *按照返回的视图名为组件的id去容器中找View对象.(json+白页适配响应)*
  - 配置了View,id=error(视图名为error)
  - 配置了BeanNameViewResolver,按照返回的视图名作为组件的id容器去找View对象. 
- 配置了DefaultErrorViewResolver 

```java 
1. 
@Bean
@ConditionalOnMissingBean(value = ErrorAttributes.class, search = SearchStrategy.CURRENT)
public DefaultErrorAttributes errorAttributes() {
    return new DefaultErrorAttributes();
}
实现了： DefaultErrorAttributes implements ErrorAttributes, HandlerExceptionResolver
    2. 
      🍤   🍤   🍤   🍤   🍤   🍤   🍤   🍤   🍤
    @Bean
    @ConditionalOnMissingBean(value = ErrorController.class, search = SearchStrategy.CURRENT)
    public BasicErrorController basicErrorController

    @Controller
    @RequestMapping("${server.error.path:${error.path:/error}}")
    默认路径冒号表示分割：server.error.path-->error.path-->/error 
    public class BasicErrorController extends AbstractErrorController {}
3. 
   🍤   🍤   🍤   🍤   🍤   🍤   🍤
    @Bean(name = "error")
    @ConditionalOnMissingBean(name = "error")
    public View defaultErrorView() {
    return this.defaultErrorView;
}
```

#### 调试

1. 执行目标方法，运行期间有任何异常都会被catch，并且用dispatchException封装，并标志当前请求结束。 

2. 进入视图解析流程（页面渲染）`processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);`

3. ```java 
   #processDispatcherResult 方法
   if (exception != null) {
       // 是不是modelandView的异常
       if (exception instanceof ModelAndViewDefiningException) {
           logger.debug("ModelAndViewDefiningException encountered", exception);
           mv = ((ModelAndView DefiningException) exception).getModelAndView();
       }
       //除了modelAndView意外的异常 
       else {
           // 拿到发生异常的控制器 
           Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
           //处理发生的异常，返回的是modelandview 
           mv = processHandlerException(request, response, handler, exception);
           errorView = (mv != null);
       }
   }
   ```

4. processHandlerException方法内

```Java
	// Check registered HandlerExceptionResolvers...
		ModelAndView exMv = null;
		if (this.handlerExceptionResolvers != null) {
            //1.遍历所有的handlerExceptionResolvers（处理器异常解析器），看有没有能处理的
			for (HandlerExceptionResolver resolver : this.handlerExceptionResolvers) {
				exMv = resolver.resolveException(request, response, handler, ex);
				if (exMv != null) {
					break;
				}
			}
		}
		if (exMv != null) {
			if (exMv.isEmpty()) {
				request.setAttribute(EXCEPTION_ATTRIBUTE, ex);
				return null;
			}
			// We might still need view name translation for a plain error model...
			if (!exMv.hasView()) {
				String defaultViewName = getDefaultViewName(request);
				if (defaultViewName != null) {
					exMv.setViewName(defaultViewName);
				}
			}
			if (logger.isTraceEnabled()) {
				logger.trace("Using resolved error view: " + exMv, ex);
			}
			else if (logger.isDebugEnabled()) {
				logger.debug("Using resolved error view: " + exMv);
			}
			WebUtils.exposeErrorRequestAttributes(request, ex, getServletName());
			return exMv;
		}

		throw ex;
	}

```

1、看到系统中默认的异常处理解析器有两个，第二个是一个组合。 有DefaultErrorAttributes ![image-20211220175206892](D:\Typora\自服务\SpringBoot.assets\image-20211220175206892.png)

2、handlerExceptionResolver的接口只有一个方法， 传入的是原生的request和response，发生异常的handler，发生的异常。 返回的是一个ModelAndView，告诉spring应该怎么渲染视图 

![image-20211220175522599](D:\Typora\自服务\SpringBoot.assets\image-20211220175522599.png)

2.1 DefaultErrorAttribute处理异常： 把异常信息保存到request，并返回空。 

5. 没有任何处理器能够处理异常，默认抛出去。 
6. 最终底层会发送/error请求，会被BasicErrorController处理。 
7. 解析错误视图，遍历所有的ErrorViewResolver，看谁能处理（DefaultErrorViewResolver）

#### 异常处理的注解



```Java
/**
 * 处理整个web controller的异常
 */
@ControllerAdvice
@Slf4j
public class GobalException {
    @ExceptionHandler(value = {ArithmeticException.class, NullPointerException.class})
    public String handlerArithException(Exception e ) {
        log.error("异常：{}",e) ;
        return "login";
    }
}
```

1、 @ExceptionHandler+@ControllerAdvice  底层由ExceptionHandlerExceptionResolver支持 

2、 @ResponseStatu+自定义异常 底层由ResponseStatusExceptionResolver支持，将@ResponseStatus的封装起来（状态码，状态信息等。调用response.sendError(statusCode);方法，**给tomcat发送的/error （请求就结束了） modelAndView就是空的。 **

### 注入原生组件

web原生组件指：Servlet，Filter，Listener

https://www.cnblogs.com/wwjj4811/p/14643899.html

 两种方式：

①、**使用注解@WebServlet，@WebFilter,@WebListener** 

**然后在主类（一个配置类中)添加自动扫描**

**@ServletComponentScan(basePackages = "com.wingchi.springboot1.servlet")**

```java 
@WebServlet(urlPatterns = "/my")
public class MyServlet extends HttpServlet {
    @Override

    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        resp.getWriter().write("123456") ;
    }
}

```

```Java 
@Slf4j
@WebFilter(urlPatterns = {"/css/*","/images/*"})
public class MyFilter extends HttpFilter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("filter初始化了") ;
        super.init(filterConfig);
    }

    @Override
    public void destroy() {
        log.info("filter销毁") ;
        super.destroy();
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        log.info("filter工作了");
        chain.doFilter(request,response);
    }
}
```

**②、使用RegistrationBean方式注册进去** 

```Java
@Configuration(proxyBeanMethods = true )
public class MyRegisterConfig {
    @Bean
     public ServletRegistrationBean myServlet() {
        MyServlet myServlet =new MyServlet();
        return new ServletRegistrationBean(myServlet,"/my","/mymy");
    }
    @Bean
    public FilterRegistrationBean myFilter() {
        MyFilter myFilter =new MyFilter();
//        return new FilterRegistrationBean(myFilter,myServlet()) ;
        FilterRegistrationBean filterRegistrationBean=  new FilterRegistrationBean(myFilter) ;
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/my","/mymy"));
        return filterRegistrationBean;
    }
    @Bean
    public ServletListenerRegistrationBean MylistenerRegistrationBean() {
        MyServletContextListener myServletContextListener= new MyServletContextListener();
        return new ServletListenerRegistrationBean(myServletContextListener) ;
    }
}

```

一个细节：上面的 proxyBeanMethods = true ，防止多次new新对象，容器太多对象了。 

#### 源码分析

拓展：DispatcherServlet如何注册进来？ 

- 容器中自动配置了DispatcherServlet，并绑定了文件WebMvcProperties文件，对应的配置文件项是pring.mvc
- 通过ServletRegistrationBean<DispatcherServelt>把DispatcherServlet配置进来
- 默认映射的路径是 / 
- 机制： Tomcat-Servlet的处理
  - 如果多个Servlet都能处理到同一个路径，那就是精确优先原则。 因为 /my，我们配置了更精确的路径，不走spring的流程

### 嵌入式Servlet容器

- 默认支持的webServer
  - Tomcat，jetty，undertow
- 原理
  - springboot应用启动发现当前是一个web应用，导入的starter
  - web应用会创建一个web版的ioc容器，servletWebServerApplicationContext
  - servletWebServerApplicationContext在启动的时候需按照 `ServletWebServerFactory`  （Servlet的web服务器工厂--->（生成Servlet的web服务器
  - 默认支持上面三种工厂
  - 使用ServletWebServerFactoryAutoConfiguration这个配置自动类
  - 内嵌服务器，就是手动把启动服务器的代码调用起来。 

切换web容器 https://blog.csdn.net/JAVA_MHH/article/details/115417612

### web定制化

有几种方式：

1. 修改配置文件
2. xxCustomizer
3. 自定义配置类：如xxxConfiguration+@Bean替换容器中的默认组件，因为许多默认组件都是加上了@ConditionalOnMissBean的注解
4. **⭐web应用下，实现webMvcConfigurer即可定制化web功能。** 

>  If you want to keep those Spring Boot MVC customizations and make more [MVC customizations](https://docs.spring.io/spring-framework/docs/5.3.14/reference/html/web.html#mvc) (interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`. 

5、全面接管springmvc，则在自己的配置类上加注解 @EnableWebMvc+WebMvcConfigurer，**所有规则全部自己重新写。** 

原理：

- WebMvcAutoConfiguration是默认的springmvc自动配置功能类. 配置了静态资源，欢迎页..等规则
- @EnableMvc源码内有导入：**@Import(DelegatingWebMvcConfiguration.class)**
- public class DelegatingWebMvcConfiguration extends **WebMvcConfigurationSupport** 的功能： 获取系统中所有的WebConfigurer，所有功能的定制一起生效。  
- 而WebMvcAutoConfiguration要生效的前提：**@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)**
- 所以一旦使用了EnableMvc注解，WebMvcAutoConfiguration则不生效 





## 单元测试

> 拓展：Mock测试，单元测试的重要方法之一。
>
> 在测试过程中，对于某些不容易构造或者不容易获取的对象，如HttpServletRequest必须在Servlet容器中才能构造出来，JDBC中的ResultSet对象。 用一个虚拟的对象（Mock对象）来创建以便测试的测试方法
>
> Mock的最大功能是把单元测试的耦合分解开，如果代码对另一个类或接口有依赖，它可以帮你模拟这些依赖，并帮你验证所调用的依赖的行为。 
>
> https://waylau.com/mockito-quick-start/



单元测试：针对最小的功能单元编写测试代码。 在java中最小的功能单元就是方法，因此对java程序进行单元测试就是针对单个java方法的测试。 

Junit是开源的java测试框架，有如下特性：

- 用于测试期望结果的断言
- 用于共享共同测试数据的测试工具
- 用于方便的组织和运行测试的测试套件
- 图形和文本的测试运行器

单元测试的注解：缺一不可 

```java 
@SpringBootTest(classes = UserApplication.class)
@RunWith(SpringRunner.class)
//由于是Web项目，Junit需要模拟ServletContext，因此我们需要给我们的测试类加上@WebAppConfiguration。
@WebAppConfiguration
```

1. 如果没有RunWith注解，会导致注入的bena为空
2. 测试类所在的包名最好和主类的包名相同！ 

@SpringBootTest ：用来提供容器的环境 





#### Junit5

```xml 
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
```



> Junit5 =  Junit Platform + Junit Jupiter + Junit Vintage 

**①、Junit Platform**

启动Junit测试，ide，构建工具或插件都需要包含和扩展platform api，它定义了TestEngine在平台运行的新测试框架的api

**②、junit jupiter** 

用于编写测试代码的新的编程和扩展模型

**③、junit vintage**

支持在junit的测试代码中运行junit3，4方式写的测试，它能够向前兼容之前的测试代码

#### 注解

| Annotations    | 描述                                                     |
| :------------- | :------------------------------------------------------- |
| `@BeforeEach`  | 在方法上注解，在每个测试方法运行之前执行。               |
| `@AfterEach`   | 在方法上注解，在每个测试方法运行之后执行                 |
| `@BeforeAll`   | 该注解方法会在所有测试方法之前运行，该方法必须是静态的。 |
| `@AfterAll`    | 该注解方法会在所有测试方法之后运行，该方法必须是静态的。 |
| `@Test`        | 用于将方法标记为测试方法                                 |
| `@DisplayName` | 用于为测试类或测试方法提供任何自定义显示名称             |
| `@Disable`     | 用于禁用或忽略测试类或方法                               |
| `@Nested`      | 用于创建嵌套测试类                                       |
| `@Tag`         | 用于测试发现或过滤的标签来标记测试方法或类               |
| `@TestFactory` | 标记一种方法是动态测试的测试工场                         |
| `@ExtendWith`  | 用于注册自定义扩展                                       |

```Java
//注解测试
@DisplayName("测试类")
public class junit5Test {
    @DisplayName("测试方法名")
    @Test
    public  void test01() {
        System.out.println("1");
    }

    @Test
    @Disabled
    public  void test2(){
        System.out.println("2");
    }

    @Test
    @Timeout(value=500 ,unit= TimeUnit.MILLISECONDS)
    public  void test3 () throws InterruptedException {
//        Thread.sleep(600);
        System.out.println("3");
    }

//    重复测试3次
    @RepeatedTest(3)
    public void test4() {
        System.out.println("\uD83E\uDD69");

    }
    @BeforeEach
    public  void beforeEach(){
        System.out.println("测试就要开始啦");
    }

    @AfterEach
    public  void afterEach(){
        System.out.println("测试结束啦");
    }

    @BeforeAll
    public  static void beforeAll() {
        System.out.println("所有测试都要开始啦");
    }

    @AfterAll
    public static void afterall() {
        System.out.println("所有测试都结束啦");
    }
}
```

#### 断言

| 断言方法                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `assertNull(java.lang.Object object)`                        | 检查对象是否为空                                             |
| `assertNotNull(java.lang.Object object)`                     | 检查对象是否不为空                                           |
| `assertEquals(long expected, long actual)	`               | 检查long类型的值是否相等                                     |
| `assertEquals(double expected, double actual, double delta)	` | 检查指定精度的double值是否相等                               |
| `assertFalse(boolean condition)	`<br/>`assertTrue(boolean condition)` | 检查条件是否为假<br>假检查条件是否为真                       |
| `assertSame(java.lang.Object expected, java.lang.Object actual)	`<br>`assertNotSame(java.lang.Object unexpected, java.lang.Object actual)	` | 检查两个对象引用是否引用同一对象（即对象是否相等）<br/>检查两个对象引用是否不引用统一对象(即对象不等) |
|                                                              |                                                              |
|                                                              |                                                              |

断言测试：

```Java
  @Test
    @DisplayName("简单断言")
    public  void simpleAssert(){
        
       Assertions.assertEquals(3,cal(1,1)); 
        Assertions.assertEquals(3,cal(1,2));
      
        Assertions.assertArrayEquals(new int[]{1,2},new int[]{1,2});  //通过
    }

    int cal(int a ,int b){
        return a+b ;
    }
```

**如果前面的断言失败了，后面的代码都不会执行**



#### 嵌套测试

具有更强大的功能表达不同测试组之间的关系

https://stackoverflow.com/questions/36220889/whats-the-purpose-of-the-junit-5-nested-annotation

@Nested测试

```java 

@DisplayName("嵌套测试")
class TestingAStackDemo {

    Stack<Object> stack;

    @Test
    @DisplayName("new Stack()测试")
    void isInstantiatedWithNew() {
        new Stack<>();
//        外层的测试驱动不了内层的
        assertNull(stack);
    }

    @Nested
    @DisplayName("when new")
    class WhenNew {

        @BeforeEach
        void createNewStack() {
//            内层的test可以驱动外层的
            stack = new Stack<>();
        }

        @Test
        @DisplayName("is empty")
        void isEmpty() {
            assertTrue(stack.isEmpty());
        }

        @Test
        @DisplayName("throws EmptyStackException when popped")
        void throwsExceptionWhenPopped() {
            assertThrows(EmptyStackException.class, stack::pop);
        }

        @Test
        @DisplayName("throws EmptyStackException when peeked")
        void throwsExceptionWhenPeeked() {
            assertThrows(EmptyStackException.class, stack::peek);
        }

        @Nested
        @DisplayName("after pushing an element")
        class AfterPushing {

            String anElement = "an element";

            @BeforeEach
            void pushAnElement() {
                stack.push(anElement);
            }

            @Test
            @DisplayName("it is no longer empty")
            void isNotEmpty() {
                assertFalse(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when popped and is empty")
            void returnElementWhenPopped() {
                assertEquals(anElement, stack.pop());
                assertTrue(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when peeked but remains not empty")
            void returnElementWhenPeeked() {
                assertEquals(anElement, stack.peek());
                assertFalse(stack.isEmpty());
            }
        }
    }
}
```



#### 参数测试

如果待测试的输入和输出是同一组数据，可以把测试数据组织在一起用不同的测试数据调用相同的测试方法。 

@ParameterizedTest注解用来进行参数化测试

```Java 
@ParameterizedTest
@DisplayName("参数化测试")
@ValueSource(ints={1,1,2,3,4,5})
public  void paramTest(int i ) {
    System.out.println(i);
}
```



**springbooot整合**

springboot 2.2.0版本开始引入Junit5作为单元测试默认库

springBoot Test是在spring Test之上的再次封装，增加了切片测试，提高了mock功能。

-------------

`@SpringBootTest`具有加载applicationContext的能力。在前面的数据库junit测试中，如果只在测试类上标注junit的@Test注解，因为没有ioc容器，JdbcTemplate等组件根本没有导入。 使用@SpringBootTest注解才有容器的支持。







###  启动过程

**🔔第一步：创建SpringApplication**

- 保存一些属性，利用ClassUtil判断当前应用是Servlet还是Reactive
- 找**bootstrappers**：初始化引导器（List类型）在**Spring.factories**文件中找
- **ApplicationContextInitializer** ，**Spring.factories**文件中找，找到了7个
- 找**ApplicationListener** ,List<ApplicationListener<?>> listeners,也是在**Spring.factories**文件中找

**🔔第二步：运行SpringApplication** 

- StopWatch:记录应用的启动时间
- 创建引导上下文（Context环境）createBootstrapContext()
  -   获取到之前的bootstrappers挨个执行，initialize()来完成对引导启动器上下文环境设置
- 让当前应用进入headless模式, java.awt.headless 





## 杂记



### @profile

https://blog.csdn.net/loongkingwhat/article/details/105745303

 `@profile`注解的作用是指定类或方法在特定的 Profile 环境生效，任何`@Component`或`@Configuration`注解的类都可以使用`@Profile`注解。在使用DI来依赖注入的时候，能够根据`@profile`标明的环境，将注入符合当前运行环境的相应的bean。 





### ApplicationContextAware

这个接口有什么用？

如果Bean类实现了ApplicationContextAware接口，这个类就可以方便获取ApplicationContext中所有的bean。

Spring能自动执行它的setApplicationContext方法。





### InitializingBean

Spring中支持3种初始化bean的方法

1. xml中指定init-method方法
2. 使用@PostConstruct注解
3. 实现InitializingBean接口

 .使用@PostConstruct注解

```Java
@Service public class AService {     			@PostConstruct   
    public void init() {       	 			             System.out.println("===初始化===");   
	}                                           } 
```



在需要初始化的方法上增加@PostConstruct注解，这样就有初始化的能力。



实现InitializingBean接口

```java 
@Service
public class BService implements InitializingBean {

    // 在bean属性初始化后就会执行该方法
    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("===初始化===");
    }
}
```





### BeanPostProcess

https://www.cnblogs.com/tuyang1129/p/12866484.html

BeanPostProcess是SpringIOC容器给我们提供的一个扩展接口，可以利用它对spring bean进行加工，如生成一个动态代理实例等等。 



```java 
public interface BeanPostProcessor {
    //bean初始化方法调用前被调用
    Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException;
    //bean初始化方法调用后被调用
    Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException;
}

```



运行顺序：

1. SpringIOC容器实例化Bean
2. 调用postProcessBeforeInitialization方法
3. 调用bean实例的初始化方法
4. 调用postProcessAfterInitialization方法

 ![img](https://upload-images.jianshu.io/upload_images/13150128-bb5c9389cd0acc6c.png?imageMogr2/auto-orient/strip|imageView2/2/w/844/format/webp) 





### 多模块扫描容器

> 前言：RPC框架中的例子需要Server引入模块，并且将其他模块的类扫描进容器中。



默认扫描的是启动类所在的包及其子包。

加上注解： @ComponentScan(basePackages={""})  ，会导致冲突，ComponentScan注解指定的包会覆盖掉SpringBootApplication扫描的包，解决方法是两个都要加上。

或者用：@SpringBootApplication(scanBasePackages = "demo.rpc")



- 多模块下会把一个大项目拆分成多个小项目，但是每个小项目package比如demo.rpc.xxx ，前缀是一样，方便扫描。

























