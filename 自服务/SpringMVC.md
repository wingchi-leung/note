| **Ctrl + Y** | **删除光标所在行 或 删除选中的行 （必备）**                  |
| ------------ | ------------------------------------------------------------ |
| **Ctrl + D** | **复制光标所在行 或 复制选择内容，并把复制内容插入光标位置下面 `（必备）`** |

| **Shift + End**  | **选中光标到当前行尾位置** |
| ---------------- | -------------------------- |
| **Shift + Home** | **选中光标到当前行头位置** |

### Servlet 

```ascii
                                                                        ┌───────┐
                                                                        │My Servlet   │
                                                                        ├───────┤
                                                                        │Servlet api  │
┌───────┐             HTTP                ├───────┤
│Browser         │          <──────>  │Web Server │
└───────┘                                        └───────┘
```



> Servlet是一个java编程语言类，用于拓展服务器的功能。 

Servlet=Server+Applet（Applet，小程序）

Servlet是运行在web服务器上的java小程序

使用Servlet可以收集来自网页表单的用户输入并处理，呈现来自数据库或其他源的记录，既可以与用户交互

Servlet可以动态地生成网页

广义的Servlet是指任意实现了Servlet接口的java程序

Servlet在网络中所处的位置如下：
  ![img](https://upload-images.jianshu.io/upload_images/14205257-8b51498bb35f1bf3.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/347/format/webp) 



```java 
@WebServlet(name="TestServlet")
public class TestServlet extends HttpServlet {
    protected  void doPost(HttpServletRequest request,
                           HttpServletResponse response) throws IOException,ServletException {
        String dev = request.getParameter("developer");
        response.setContentType("test/html;charset=UTF-8");
        response.getWriter().println("欢迎开发者"+dev) ;
    }
    protected  void doGet(HttpServletRequest request,
                           HttpServletResponse response) throws IOException,ServletException {
    }
}

```

我们使用ServletAPI时，并不直接和底层的TCP打交道，也不需要解析http协议，因为httpServletRequest和HttpServletResponse就已经封装好了请求和响应，以发送请求为例，我们只需要设置正确的相应类型，然后获取PrintWritter，写入响应即可。

Tomcat是一个支持了Servlet的服务器，由java编写，启动tomcat就是启动虚拟机，执行tomcat的main方法，然后由tomcat负责加载我们的.war文件，并创建一个TestServlet实例，最后以多线程的模式来处理http请求。我们编写的Servlet也不是直接运行的，而是由Web服务器加载后创建实例运行，所以类似TOmcat这样的web服务器也称为Servlet容器。
在Servlet容器中运行的Servlet有以下特点：

- 无法在代码直接通过new创建servlet实例，必须通过Servlet容器自动创建Servlet实例。
- Servlet容器只会给每个Servlet创建唯一实例。
- Servlet容器会使用多线程执行doGet() 或doPost方法。

### HttpServletRequest

HttpServletRequest封装了一个http请求，它实际上是由ServletRequest继承来的。通过HttpServletRequest提供的接口方法可以拿到http请求的几乎全部信息，常用的方法有：

- getMethod()：返回请求方法，例如，`"GET"`，`"POST"`；

- getParameter(name)：返回请求参数，GET请求从URL读取参数，POST请求从Body中读取参数；

- getCookies()：返回请求携带的所有Cookie；

- getHeader(name)：获取指定的Header，对Header名称不区分大小写；

- getHeaderNames()：返回所有Header名称；

- getInputStream()：如果该请求带有HTTP Body，该方法将打开一个输入流用于读取Body；

- getReader()：和getInputStream()类似，但打开的是Reader；

- getRemoteAddr()：返回客户端的IP地址；

  

  

- getMethod()：返回请求方法，例如，`"GET"`，`"POST"`；
- getRequestURI()：返回请求路径，但不包括请求参数，例如，`"/hello"`；
- getQueryString()：返回请求参数，例如，`"name=Bob&a=1&b=2"`；
- getParameter(name)：返回请求参数，GET请求从URL读取参数，POST请求从Body中读取参数；
- getContentType()：获取请求Body的类型，例如，`"application/x-www-form-urlencoded"`；
- getContextPath()：获取当前Webapp挂载的路径，对于ROOT来说，总是返回空字符串`""`；
- getCookies()：返回请求携带的所有Cookie；
- getHeader(name)：获取指定的Header，对Header名称不区分大小写；
- getHeaderNames()：返回所有Header名称；
- getInputStream()：如果该请求带有HTTP Body，该方法将打开一个输入流用于读取Body；
- getReader()：和getInputStream()类似，但打开的是Reader；
- getRemoteAddr()：返回客户端的IP地址；

----

两个重要方法

setAttribute()和getAttribute() 可以给当前的HttpServletRequest对象附加多个key-value，相当于把httpServletRequest当作一个map<String,Object>

### HttpServletResponse

HttpServletResponse封装了一个http响应，由于http响应必须先发送header，再发送body，所以操作HttpServletResponse对象时，必须先设置header的方法，再调用发送body的方法。 

### Servlet多线程模型

一个Servlet类在服务器只有一个实例，但对每一个http请求，web服务器都会使用多线程执行请求，因此，一个Servlet的doGet，doPost方法都是多线程并发执行的， 如果Servlet中定义了字段，要注意多线程并发访问的问题： 

 对于每个请求，Web服务器会创建唯一的`HttpServletRequest`和`HttpServletResponse`实例，因此，`HttpServletRequest`和`HttpServletResponse`实例只有在当前处理线程中有效，它们总是局部变量，不存在多线程共享的问题。 

### AJAX 

AJAX(Asynchronous Javascrpit and XML) 意思是用js执行异步网络请求。

### Seesion和Cookie

https://www.cnblogs.com/ityouknow/p/10856177.html



> 在Web应用中，我们经常要追踪用户身份，当一个用户登录成功后，如果继续访问其他页面，web程序如何识别出它的身份？
>
> Http协议是一个无状态协议，即Web应用无法区分收到的两个*http*请求是否是同一个浏览器发出的， 为了跟踪用户状态，服务器可以向浏览器分配一个唯一ID，并以Cookie的形式发送到浏览器，浏览器在后续访问时总是附带此Cookie，这样，服务器就可以识别用户身份。 

#### Cookie

http Cookie是服务器发送到浏览器并保存在浏览器本地的一小块数据，它会在浏览器下一次向服务器发送请求时被携带并发送到服务器上，通常用来告诉服务器两次请求是否来自同一个浏览器。

#### Session

Session代表着服务器和客户的一次会话的过程，Session对象存储特定用户会话所需的属性及配置信息，这样，当用户在web页之间跳转时，存储在session对象中的变量不会丢失，直到客户端关闭会话或Session超时失效。 



> javaee的Servlet机制内建了对Session的支持，以登录为例，一个用户登录成功后，可以把这个用户的名字放入一个HttpSession对象中，以便后续访问其他页面时，能直接从httpSession取出用户名。
>
> 对于web应用程序来说，我们总是通过httpSession这个高级接口访问当前的session，如果要深入理解session的原理，可以认为web服务器在内存中自动维护了一个ID到httpSession的映射表。
>
> 而服务器识别Session的关键就是依靠一个名为JSESSIONID的Cookie，在servlet中第一次调用req.getSession时，Servlet容器自动创建一个SessionID，通过一个名为JSESSION的Cookie发送给服务器。



 ![img](https://image-static.segmentfault.com/372/034/372034436-5c359860bfdca_fix732) 

# 三大作用域

Web交互最基本的单位为http请求，每个用户进入网站到离开网站这段过程成为一个http会话，一个服务器的运行过程中有多个用户访问， 就是多个http会话。



javaweb开发中Servlet三大域对象的应用：request，session，application

#### application

Application的作用范围在服务器一开始执行服务，到服务器关闭为止。 它的范围最大，停留时间最久。

只要数据传入Application对象，数据的范围Scope就是Application

Application对象的主要方法：

1、 getAttribute(String name)     return Object     

2、 getAttributeNames()             return Enumeration

3、 getInitParameter(String name)

4、 getServletInfo()

5、 setAttribute(String name , Object object)



#### session

http会话开始到结束这段时间，Session的作用范围为一段用户持续和服务器所连接的时间，但与服务器断线后，这个属性就无效了。

Session的开始时刻比较容易判断，从浏览器发出第一个HTTP请求即可认为会话开始。但结束时刻就不好判断了，因为浏览器关闭时并不会通知服务器，所以只能通过如下这种方法判断：如果一定的时间内客户端没有反应，则认为会话结束。Tomcat的默认值为120分钟，但这个值也可以通过HttpSession的setMaxInactiveInterval()方法来设置。

具有session范围的对象被绑定到javax.servlet.http.HttpSession对象中。

#### request

http请求开始到结束这段时间，

# SpringMVC

### 上下文对象

容器在java中最典型的就是Tmocat了，它是一个运行了Servlet的容器。 而SPring也有容器：IOC

> 容器是Spring框架实现功能的核心，容器不只是帮我们创建了对象那么简单，它负责了对象整个的生命周期的管理——创建、装配、销毁。 
>
> 只有容器什么都干不了，决定放什么对象的我们，决定对象之前依赖关系的也是我们，**我们需要通过SPring的应用上下文，像容器添加我们需要管理的对象** 
>
> SPring的应用上下文可以简单理解为Spring容器的一种抽象化表述，如ApplicationContext本质上是一个维护Bean定义以及对象之间协作关系的高级接口。

Spring提供了多种类型的容器实现，在不同场景选择：

- AnnotationConfigApplicationContext ：从一个或多个java的配置类中加载上下文，适用于java注解方式
-  ClassPathXmlApplicationContext:从类路径下的一个或多个xml配置文件中加载上下文，适用于xml配置的方式； 
- AnnotataionConfgWebApplicationContext:专门为web应用准备的，适用于注解
- XmlWebApplicationContext:从web应用下的一个或多个xml配置文件加载上下文，适用于xml配置方式。

 SpringWeb MVC 是一个基于Servlet的web框架

用户在web浏览器中点击链接或提交表单时，请求就开始了。 请求从离开浏览器到获取响应返回，会经历许多站点。 下图展示了请求使用SpringMVC所经历的所有站点：

![](D:\Typora\自服务\javaWeb.assets\image-20211007094438693.png)

> ① 用户请求发送至**DispatcherServlet**类进行处理。
>
> ②、 DispatcherServlet类遍历所有配置的HandlerMapping类，请求查Handler。
>
> PS：HandlerMapping是SPringMVC中完成url到controller映射的组件，DispatcherServlet接收Request，然后从HandlerMapping查找处理request的controller 
>
> HandlerMappin类根据request请求的URL等信息查找能够进行处理的Handler，以及相关拦截器HandlerInterceptor并构造HandlerExecutionChain。然后HandlerMapping类将构造的andlerExecutionChain类的对象返回给前端控制器DispatcherServlet类。 
>
> ③ 前端控制器拿着上一步的`Handler`遍历所有配置的`HandlerAdapter类`，请求执行`Handler` 
>
> ④ `HandlerAdapter类`执行相关`Handler`并获取`ModelAndView类`的对象，然后将该`ModelAndView类`的对象返回给前端控制器。
>
> ⑤ `DispatcherServlet类`遍历所有配置的`ViewResolver类`，请求进行视图解析。`ViewResolver类`解析视图后得到`View`对象并返回给前端控制器。
>
> ⑥ `DispatcherServlet类`进行视图`View`的渲染，填充`Model`。
>
> ⑦ `DispatcherServlet类`向用户返回响应。





**①、**请求的第一站是Spring的DispatchServlet，和大多数基于Java的web框架一样，springmvc的所有请求都会通过一个前端控制器front Controller Servlet 。

-  前端控制器是通用的web应用程序模式，在这里一个单实例的Servlet将请求委托给应用程序的其他组件来执行实际处理

**②、**DispatchServlet的任务是将请求发送给Spring MVC的控制器，控制器是一个用于处理请求的Spring组件，一般会有多个控制器，DispatchServlet**需要查询一个或多个处理器映射handler mapping 来确定请求的下一站在哪里。 处理器映射会根据请求所带的url信息来决策 （确定处理器后DispatchServlet的工作就完成了）** 

**③、**控制器完成逻辑处理后，通常会产生一些信息， 这些信息需要返回给用户显示，这些信息称为model， model还需要以用户友好的方式进行格式化，一般是html， 所以，model需要发送一个视图view，通常是jsp

**④、** 控制器所做的最后一件事就是将模型数据打包，用于标识用于渲染输出的视图名，它接下来会将请求连同模型和视图名发回DispatchServlet





### helloworld

https://xuanjian1992.top/2019/08/17/Spring-MVC%E5%9F%BA%E4%BA%8EJava-Config%E9%85%8D%E7%BD%AE%E7%9A%84%E5%90%AF%E5%8A%A8%E6%B5%81%E7%A8%8B%E5%88%86%E6%9E%90/



http://lihuia.com/%E3%80%90%E8%BD%AC%E3%80%91spring-mvc%EF%BC%9Atomcat%EF%BC%8Cservlet%EF%BC%8Cspring/

#### xml方式

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
          http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>

  <!--配置DispatcherServlet-->
  <servlet>
    <servlet-name>springmvc-deom</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <!--Servlet的参数配置 -->
	<!--contextConfigLocation：指定SpringMVC配置文件路径--> 
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:applicationContext.xml</param-value>
        <!---在 web.xml 中配置 DispatchcerServlet，DispatchcerServlet 加载时需要一个 Spring MVC 的配置文件，如果不提供名称，将默认会去 WEB-INF 下查找对应的 [servlet-name]-servlet.xml 文件。（这里提供了名称）->
    </init-param>
   <!--服务器已启动就创建对象-->   
    <load-on-startup>1</load-on-startup>
    <async-supported>true</async-supported>
  </servlet>
  <servlet-mapping>
    <servlet-name>springmvc-deom</servlet-name>
    <!-- 拦截路径： /    /和/*的区别，都是拦截所有路径，但前者不拦截.jsp的路径，后者都拦截（不能拦截.jsp的：1.有可能无法显示、2. 浪费资源 -->
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>

```



```java 
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <context:component-scan base-package="com.wingchi"></context:component-scan>

    <bean id="defaultViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/pages/"/><!--設定JSP檔案的目錄位置-->
        <property name="suffix" value=".jsp"/>
        <property name="exposeContextBeansAsAttributes" value="true"/>
    </bean>
</beans>
```





#### javaconfig

>  在Servlet 3.0环境中，容器会在类路径中查找实现javax.servlet.ServletContainerInitializer接口的类，如果能发现的话，就会用它来配置Servlet容器。
>
>  Spring提供了这个接口的实现，名为SpringServletContainerInitializer，这个类 反过来又会查找实现WebApplicationInitializer的类并将配置的任务交给它们来完 成。Spring 3.2引入了一个便利的WebApplicationInitializer基础实现，也就是**AbstractAnnotationConfigDispatcherServletInitializer**。因此当部署到Servlet 3.0容器中的时候，容器会自动发现它，并用它来配置Servlet上下文。 

https://www.cnblogs.com/lovozcf/p/14218242.html

https://www.cnblogs.com/lovozcf/p/14289531.html

①、导入依赖

```xml 
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.7.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.7.RELEASE</version>
</dependency>
<dependency>
     <groupId>org.apache.tomcat.embed</groupId>
     <artifactId>tomcat-embed-core</artifactId>
     <version>9.0.36</version>
</dependency>
```

②、写配置，这次使用java配置类

```java
@Configuration //申明该类为配置类
//扫描这个包
@ComponentScan("com/wingchi/controller")
@EnableWebMvc //提供mvc支持
public class WebApplicationConfig implements WebMvcConfigurer {

    //设置响应信息编码集
    @Override
    public void extendMessageConverters(
            List<HttpMessageConverter<?>> converters) {
        StringHttpMessageConverter stringHttpMessageConverter =
                (StringHttpMessageConverter) converters.get(1);
        stringHttpMessageConverter.setDefaultCharset(
                Charset.forName("utf-8"));
    }

    //提供静态资源的支持
    @Override
    public void addResourceHandlers(
            ResourceHandlerRegistry registry) {
        //客户端以/html/开始的请求，访问类路径下static/html
        registry.addResourceHandler("/html/**").
                addResourceLocations("classpath:/static/html/");
    }
}
```

可以在resources目录下创建static/html目录用来放置静态资源



③、编写启动类

```java
package com.wingchi.server;

import com.wingchi.config.webAppConfig;
import org.apache.catalina.Context;
import org.apache.catalina.Wrapper;
import org.apache.catalina.startup.Tomcat;
import org.springframework.web.context.WebApplicationContext;
import org.springframework.web.context.support.AnnotationConfigWebApplicationContext;
import org.springframework.web.servlet.DispatcherServlet;

import javax.servlet.ServletContext;

public class mainServer {
    public mainServer() {
        Tomcat tomcat =new Tomcat();
        tomcat.setPort(8088);
        tomcat.getConnector();
        Context context = tomcat.addContext("",null) ;
        try{
            //配置前端控制器
            DispatcherServlet dispatcherServlet =
                    new DispatcherServlet(this.createApplicationContext(context.getServletContext()));
            Wrapper servlet = Tomcat.addServlet(context,"dispatcherServlet",dispatcherServlet);
            servlet.setLoadOnStartup(1);
            servlet.addMapping("/*");

            tomcat.start();
        }catch (Exception e){
            System.out.println("what wrong?");
            e.printStackTrace();
        }
    }
    public WebApplicationContext createApplicationContext(ServletContext servletContext) {
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(webAppConfig.class) ;
        ctx.setServletContext(servletContext);
//        ctx.refresh();
        ctx.registerShutdownHook();
        return ctx ;
    }

    public static void main(String []args){
        new mainServer();
    }
}

```



④、编写控制器

```java 
package com.project.conrtroller;

@Controller
public class TestController {
    @RequestMapping("/test")
    public String testRequest(){
        System.out.println("执行应用控制器方法，参数name为");
       //重定向到指定页面
        //在resouces目录下创建html/info.html页面
        //然后访问localhost/test
        return "redirect:/html/info.html";
    }
}
```

spring-api报错地址：https://blog.csdn.net/a785975139/article/details/80640425

### DispatcherServlet

DispatcherServlet提供一个算法应对请求。

此Servlet需要被声明和映射到其他控制器中， 可以使用javaConfig和web.xml方式。 



下面的例子演示了如何使用java 配置注册和初始化DispatchServlet ,它由Servlet容器自动检测。

```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext servletContext) {

        // 加载SPring web 应用的配置
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        context.register(AppConfig.class); // 换成自己的配置类

        // 创建一个DispatchServlet类
        DispatcherServlet servlet = new DispatcherServlet(context);
        ServletRegistration.Dynamic registration = servletContext.addServlet("app", servlet);
        registration.setLoadOnStartup(1);
        registration.addMapping("/app/*");
    }
}
```

> SpringMVC前端控制器收到所有请求
>
> 看请求地址和@RequestMapping标注的那个匹配
>
> 找到目标Servlet和目标方法，直接例用反射执行目标方法
>
> 方法执行完后返回值，SpringMVC处理成要去的页面的地址，用视图解析器进行拼接得到完整的页面地址
>
> 拿到页面地址后，前端控制器帮我们转发页面。



DispatcherServlet需要一个WebApplicationContext（一个普通ApplicationContext的拓展）作为他自己的配置。



/* :拦截所有请求，包括jsp 即 *.jsp

/ ：拦截所有请求，不包括jsp ，即 *.jsp



#### 源码分析

​         ![img](D:\Typora\自服务\javaWeb.assets\clip_image002.jpg)  



https://blog.csdn.net/h294229025/article/details/100051049

https://greedypirate.github.io/2019/10/05/SpringMVC%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/#%E7%BB%93%E6%9D%9F

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
		HttpServletRequest processedRequest = request;
		HandlerExecutionChain mappedHandler = null;
		boolean multipartRequestParsed = false;

		WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

		try {
			ModelAndView mv = null;
			Exception dispatchException = null;

			try {
                // 1. 检查是否是文件上传请求 
				processedRequest = checkMultipart(request);
				multipartRequestParsed = (processedRequest != request);

				// Determine handler for the current request.
                		// 2.根据当前请求决定使用哪个 handler ，就是找到我们自定义的控制器
				mappedHandler = getHandler(processedRequest);
				if (mappedHandler == null || mappedHandler.getHandler() == null) {
					noHandlerFound(processedRequest, response);
					return;
				}

				// Determine handler adapter for the current request.
                			//3. 拿到能执行这个类的所有方法的适配器（反射工具）
				HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

				// Process last-modified header, if supported by the handler.
				String method = request.getMethod();
				boolean isGet = "GET".equals(method);
				if (isGet || "HEAD".equals(method)) {
					long lastModified = ha.getLastModified(request, mappedHandler.getHandler());
					if (logger.isDebugEnabled()) {
						logger.debug("Last-Modified value for [" + getRequestUri(request) + "] is: " + lastModified);
					}
					if (new ServletWebRequest(request, response).checkNotModified(lastModified) && isGet) {
						return;
					}
				}
				// Actually invoke the handler.
                // 4.【关键】用适配器执行目标方法 ，将目标方法的返回值作为视图名，设置保存到返回modelAndView中的Views里，有多个实现，这里的具体实现在AnnotationMethodHandlerAdpater 类里。
				mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

				if (asyncManager.isConcurrentHandlingStarted()) {
					return;
				}
			//如果没有视图，就用默认视图
				applyDefaultViewName(processedRequest, mv);
				mappedHandler.applyPostHandle(processedRequest, response, mv);
			}
            		//转发到页面，根据modelAndView转发到页面， 而且ModelAndView中的数据可以从请求域中获取。 
			processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
		}
	}
```

doDisPatch流程：

1. 判断是否为Multipart类型(文件上传)，如果是，则用multipartResolver解析Request
2. 通过getHandler方法，在HandlerMapping找到请求对应的handler，**获得目标处理器类**
3. getHandlerAdapter方法找到handler对应的handlerAdapter处理适配器**（用途：使用适配器调用目标方法）**
4. HandlerAdapter执行目标方法处理请求，返回ModleAndView
6. 根据ModelAndView调用processDispatcherResult方法处理请求结果，封装到response方法。

---------

- HandlerMapping：是用来查找Handler的，在一个Spring MVC中会出现很多请求，如何根据不同的请求找到对应的Handler便是HandlerMappings的工作。
- Handler：处理器，它和Controller是对应的，它的具体表现形式可以是类也可以是方法，在Controller层中标注了@RequestMapping注解的方法都可以是一个Handler。
- HandlerAdapter：从字面意义上可以知道，这是针对Handler的一个适配器，因为Handler有各种不同的形式，但是Servlet中执行Handler的结构却是不变的，都是以request和response为参数的方法，HandlerAdapter作为中间的适配器连接了Servlet和Handler。
- 总结：使用handlerMapping找到handler，再根据handler生成其对应的handlerAdapter，让handlerAdapter调用handler处理请求，再将处理结果View展示给用户。

#### ha.handle细节

```java
	@Override
	public ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {

		Class<?> clazz = ClassUtils.getUserClass(handler);
		Boolean annotatedWithSessionAttributes = this.sessionAnnotatedClassesCache.get(clazz);
		if (annotatedWithSessionAttributes == null) {
			annotatedWithSessionAttributes = (AnnotationUtils.findAnnotation(clazz, SessionAttributes.class) != null);
			this.sessionAnnotatedClassesCache.put(clazz, annotatedWithSessionAttributes);
		}

		if (annotatedWithSessionAttributes) {
			checkAndPrepare(request, response, this.cacheSecondsForSessionAttributeHandlers, true);
		}
		else {
			checkAndPrepare(request, response, true);
		}

		// Execute invokeHandlerMethod in synchronized block if required.
		if (this.synchronizeOnSession) {
			HttpSession session = request.getSession(false);
			if (session != null) {
				Object mutex = WebUtils.getSessionMutex(session);
				synchronized (mutex) {
					return invokeHandlerMethod(request, response, handler);
				}
			}
		}

		return invokeHandlerMethod(request, response, handler);
	}
```





#### getHandler细节

```java
protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
		for (HandlerMapping hm : this.handlerMappings) {
			if (logger.isTraceEnabled()) {
				logger.trace(
						"Testing handler map [" + hm + "] in DispatcherServlet with name '" + getServletName() + "'");
			}
			HandlerExecutionChain handler = hm.getHandler(request);
			if (handler != null) {
				return handler;
			}
		}
		return null;
	}
```

handlerMappings：IOC容器一启动，就扫描每个requestMapping保存起来。

#### 目标方法执行细节

mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

**研究它是怎么利用反射获取目标方法，以及目标方法的参数。** 

```java 
protected ModelAndView invokeHandlerMethod(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
//1. 获取方法解析器 
		ServletHandlerMethodResolver methodResolver = getMethodResolver(handler);
// 2.方法解析器根据请求解析那个方法可以执行		
    Method handlerMethod = methodResolver.resolveHandlerMethod(request);
// 3.创建方法执行器		
    ServletHandlerMethodInvoker methodInvoker = new ServletHandlerMethodInvoker(methodResolver);
//4. 将request和response 包装成webRequset		
    ServletWebRequest webRequest = new ServletWebRequest(request, response);
    // 5.创建了 BindingAwareModelMap 隐含模型  
		ExtendedModelMap implicitModel = new BindingAwareModelMap();
// 7.【重点】：真正执行了目标方法
		Object result = methodInvoker.invokeHandlerMethod(handlerMethod, handler, webRequest, implicitModel);
		ModelAndView mav =
				methodInvoker.getModelAndView(handlerMethod, handler.getClass(), result, implicitModel, webRequest);
		methodInvoker.updateModelAttributes(handler, (mav != null ? mav.getModel() : null), implicitModel, webRequest);
		return mav;
	}
```

#### invokeHandlerMethod

```java 
// HandlerMethodInvoker.java 
public final Object invokeHandlerMethod(Method handlerMethod, Object handler,
			NativeWebRequest webRequest, ExtendedModelMap implicitModel) throws Exception {

		Method handlerMethodToInvoke = BridgeMethodResolver.findBridgedMethod(handlerMethod);
		try {
			boolean debug = logger.isDebugEnabled();
			for (String attrName : this.methodResolver.getActualSessionAttributeNames()) {
				Object attrValue = this.sessionAttributeStore.retrieveAttribute(webRequest, attrName);
				if (attrValue != null) {
					implicitModel.addAttribute(attrName, attrValue);
				}
			}
                            //1. 找到所有标注了@ModelAttribute的方法
			for (Method attributeMethod : this.methodResolver.getModelAttributeMethods()) {

				Method attributeMethodToInvoke = BridgeMethodResolver.findBridgedMethod(attributeMethod);
                //2. 确定@ModelAttribte方法执行的参数，传入了隐含模型
				Object[] args = resolveHandlerArguments(attributeMethodToInvoke, handler, webRequest, implicitModel);
				if (debug) {
					logger.debug("Invoking model attribute method: " + attributeMethodToInvoke);
				}
				String attrName = AnnotationUtils.findAnnotation(attributeMethod, ModelAttribute.class).value();

				if (!"".equals(attrName) && implicitModel.containsAttribute(attrName)) {
					continue;
				}
				ReflectionUtils.makeAccessible(attributeMethodToInvoke);
                // 执行modelAttriubte方法
				Object attrValue = attributeMethodToInvoke.invoke(handler, args);
                                // 方法上的@ModelAttribute如果有value，那么attrName就等于这个value
                //如果没有标，attrName就是返回值的首字母小写，例如“void"
				if ("".equals(attrName)) {
					Class<?> resolvedType = GenericTypeResolver.resolveReturnType(attributeMethodToInvoke, handler.getClass());
					attrName = Conventions.getVariableNameForReturnType(attributeMethodToInvoke, resolvedType, attrValue);
				}
                //判断隐含模型中是否有attrName，如果没有，就放一个。
                //@ModelAttribute 的另一个作用，可以把方法运行后的返回值按照@ModelAttribute("abc")指定的key放到隐含模型中，如果没有指定，默认使用返回类型的首字母小写
				if (!implicitModel.containsAttribute(attrName)) {
					implicitModel.addAttribute(attrName, attrValue);
				}
			}
			Object[] args = resolveHandlerArguments(handlerMethodToInvoke, handler, webRequest, implicitModel);
			if (debug) {
				logger.debug("Invoking request handler method: " + handlerMethodToInvoke);
			}
			ReflectionUtils.makeAccessible(handlerMethodToInvoke);
            // 目标方法真正运行。
			return handlerMethodToInvoke.invoke(handler, args);
		}
	
	}
```



### 确定参数的值

```Java
//HandlerMethodInvoker.java  
private Object[] resolveHandlerArguments(Method handlerMethod, Object handler,
			NativeWebRequest webRequest, ExtendedModelMap implicitModel) 
 //1.先判断是否是原生API
	Object argValue = resolveCommonArgument(methodParam, webRequest);
   //2.再看是否是Model或者Map ，如果是，就把隐含模型赋值给它（参数
// 如果自定义类型参数没有注解，同1，2，再判断是否为其他类型。 
    if (Model.class.isAssignableFrom(paramType) || Map.class.isAssignableFrom(paramType)) {
        if (!paramType.isAssignableFrom(implicitModel.getClass())) {
            throw new IllegalStateException("Argument [" + paramType.getSimpleName() + "] is of type " +
                                            "Model or Map but is not assignable from the actual model. You may need to switch " +
                                            "newer MVC infrastructure classes to use this argument.");
        }
        args[i] = implicitModel;
    }
//判断是不是简单类型，如Integer
else if (BeanUtils.isSimpleProperty(paramType)) {
    paramName = "";
}
//如果是自定义类型，直接空串
else {
    attrName = "";
}
```

 自定义类型POJO：

1. 如果参数标注了ModelAttribute注解，attrName赋值为注解的value值
2. 如果没有ModelAttribute注解，就给attrName赋值""空串

 确定POJO的值；

```java  
else if (attrName != null) {
				WebDataBinder binder =
						resolveModelAttribute(attrName, methodParam, implicitModel, webRequest, handler);
				boolean assignBindingResult = (args.length > i + 1 && Errors.class.isAssignableFrom(paramTypes[i + 1]));
				if (binder.getTarget() != null) {
                    // 将对象和请求的数据一一绑定
					doBind(binder, webRequest, validate, validationHints, !assignBindingResult);
				}
				args[i] = binder.getTarget();
				if (assignBindingResult) {
					args[i + 1] = binder.getBindingResult();
					i++;
				}
				implicitModel.putAll(binder.getBindingResult().getModel());
			}
```



### SpringMVC工作机制

容器初始化时会建立所有url和controller的对应关系，保存到Map<url,Controller>中，tomcat启动后会通知SPring初始化容器，然后SPringmvc会遍历容器中的bean，获取controller中的所有方法访问的url，并将他们保存在map中。

这样，可以根据request快速定位controller。 



确定好处理请求的method后，接下来就是参数绑定，把request中参数绑定到目标方法的形参上。**这一步是整个请求处理过程中最复杂的一步：**springmvc提供两个两种request与方法形参的绑定方法：

1. 通过注解进行绑定@RequestParam
2. 通过参数名进行绑定

https://blog.csdn.net/win7system/article/details/90674757



 ![img](https://img-blog.csdnimg.cn/20190530112000912.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dpbjdzeXN0ZW0=,size_16,color_FFFFFF,t_70) 



### springMVC九大组件

SpringMVC在工作时，关键位置都是由这些组件来完成的。

```java 
// 文件上传解析器
private MultipartResolver multipartResolver;

	/** LocaleResolver used by this servlet区域信息解析器，国际化 */
	private LocaleResolver localeResolver;

	/** ThemeResolver used by this servlet 主题解析器*/
	private ThemeResolver themeResolver;

	/** List of HandlerMappings used by this servlet映射信息 */
	private List<HandlerMapping> handlerMappings;

	/** List of HandlerAdapters used by this servlet */
 // Handler的适配器 
	private List<HandlerAdapter> handlerAdapters;

	/** List of HandlerExceptionResolvers used by this servlet */
// 异常解析器 
	private List<HandlerExceptionResolver> handlerExceptionResolvers;

	/** RequestToViewNameTranslator used by this servlet */
	private RequestToViewNameTranslator viewNameTranslator;

	/** FlashMapManager used by this servlet */
// SPringMVc中允许重定向携带数据功能
	private FlashMapManager flashMapManager;

	/** 视图解析器 */
	private List<ViewResolver> viewResolvers;
```

九大组件都是接口，**接口就是规范** （拓展性好）

九大组件初始化：在DispatcherServlet接口中，

```java 
protected void initStrategies(ApplicationContext context) {
		initMultipartResolver(context);
		initLocaleResolver(context);
		initThemeResolver(context);
		initHandlerMappings(context);
		initHandlerAdapters(context);
		initHandlerExceptionResolvers(context);
		initRequestToViewNameTranslator(context);
		initViewResolvers(context);
		initFlashMapManager(context);
	}
```

 



### @RequestMapping

可以标注到类和方法上。

- 标注在类上，类中的方法都共享相同的根路径

- 标注在方法上用来确定某个指定方法



属性：

- method：限定http请求方式，有GET，POST，等等

- parms：规定请求参数

```java
@RequestMapping("/spring")
public class testcontroller {
    //url: http://localhost:8088/spring/test?username=tom 
    // 规定必须要有请求参数username
    @RequestMapping(value = "/test" , params = "username") 
    //1.指定了需要username为小明的 
     @RequestMapping(value = "/test" , params = "username=xiaoming")
    //2.指定了username不能为小明
    @RequestMapping(value = "/test" , params = "username=!xiaoming")
    //3.还可以指定多个，意思是必须要有username，pwd，pwd的值必须为12345，不能有pass
    //错误例子：http://localhost:8088/spring/test?username=tom&pwd=12345&pass=false
    @RequestMapping(value = "/test" , params = {"username","pwd=12345","!pass"})
    public String testRequest(String username){
        System.out.println("执行应用控制器方法，参数name为"+username );
        return "redirect:/html/info.html" ;
    }
}

```

- header：规定请求头，和param一样，可以规定简单请求头

例如可以规定只能让google浏览器访问，不让火狐浏览器访问：

```java
 @RequestMapping(value = "test2" , headers = {"User-Agent"="在goole上f12 network自己找"})
```

**param参数模糊匹配：**

RequestMapping-ant风格的url ，类型：

? ：能替代任意一个字符

*：能替代任意多个字符，和一层路径

**：能替代多层路径

```java

//    测试RequestMapping的模糊匹配
    @RequestMapping("AntTest01")
    public  String antTest(String name){
        return "redirect:/html/info.html" ;
    }

    @RequestMapping("AntTest0?")
    public  String antTest2(String name){
        System.out.println("test2");
        return "redirect:/html/info.html" ;
    }

    @RequestMapping("AntTest0*")
    public  String antTest3(String name){
        System.out.println("test3");
        return "redirect:/html/info.html" ;
    }
//    eg:http://localhost:8088/spring/AntTest0/ad/multMatch
    @RequestMapping("AntTest0/a*/multMatch")
    public  String antTest4(String name){
        System.out.println("一层路径模糊匹配0");
        return "redirect:/html/info.html" ;
    }

    //    eg:http://localhost:8088/spring/AntTest0/a/3/multMatch
//    只匹配一层路径
    @RequestMapping("AntTest0/a/*/multMatch")
    public  String antTest5(String name){
        System.out.println("多层路径模糊匹配1");
        return "redirect:/html/info.html" ;
    }

    //    eg:http://localhost:8088/spring/AntTest0/a/3/4/r/4/e/multMatch
//    这样可以匹配多层路径
    @RequestMapping("AntTest0/a/**/multMatch")
    public  String antTest6(String name){
        System.out.println("多层路径模糊匹配2");
        return "redirect:/html/info.html" ;
    }
```

### @PathVariable

路径占位符，通过@PathVariable可以将url中占位符参数绑定到控制器处理方法的入参中：

```java
 @RequestMapping(value = "user/{id}")
//http://localhost:8080/springmvc-crud/user/tomcat 
@pathVariable("xx")中的值和url的一样就好了，不需要和方法参数名一样 
    public String pathVariableTest(@PathVariable("id") String username) {
        System.out.println("id = " + username);
        return "redirect:/html/info.html";
    }
```





### 请求处理

#### @RequestParam 

获取请求参数的方式：

1. 直接给方法参数上写一个和请求参数名相同的变量，用来接收这个请求参数的值

2. 可以显示指定该参数的名称，使得方法参数和请求参数不用一致
   

@RequestParam用于将指定的请求参数赋值给方法中的形参。

@RequestParam有三个属性：

1. （必须配置）value：要获取的参数的key
2. require：指定该参数是否为必须， 默认必须
3. defalutValue：没有参数时的默认参数 

RequestParam和@PathVariable注解是用于从request中接收请求参数的并映射到功能方法的参数上。

两个都可以接收参数，不同的是@RequestParam是从request里取值，而@PathVariable是从一个uri模板里填充。

```java 
// http://localhost:8080/springmvc-crud/user1?username=xiaoming  
// username=xiaoming 会自动和形参username绑定。 
@RequestMapping(value = "user1")
    public String defaultTest(String username) {
        System.out.println("userName is" + username);
        return "hello";
    }

// @RequestParam 指定的value值就是url请求参数的名称！和形参名称没有关系
//http://localhost:8080/springmvc-crud/cat?kitty_Name=kitty
  @RequestMapping(value = "cat")
    public String RequestParamTest(@RequestParam("kitty_Name") String catName) {
        System.out.println(catName);
        return "hello";
    }
```





```java
   @RequestMapping(value = "user")
    /**
     * 获取请求参数的默认方式：直接给方法参数上写一个和请求参数名相同的变量，用来接收这个请求参数的值
     */
    public String handle02(@RequestParam(value = "user" ,required =false,defaultValue = "你没带")String username) {
        System.out.println(username);
        return "/html/info.html" ;
    }
```

和@pathVarialbe的区别

pathVarialbe获取的是请求路径上的值， 这个是获取请求参数的值

http://localhost:8088/{eee}/user?user=hhh

```java
  @RequestMapping(value = "user{id}")
    public String handle02(@RequestParam(value = "user" ,required =false,defaultValue = "你没带")String username, @PathVariable(name = "id")String id) {
        System.out.println(username);
        return "/html/info.html" ;
    }
```

#### @RequestHeader

使用@RequestHeader注解将请求标头绑定到控制器众的方法参数

```java
 @RequestMapping(value = "Header")
    public String pathVariableTest(@RequestHeader("User-Agent")String userAgent) {
        System.out.println("请求头是：" + userAgent);
        return "/html/welcome.html";
    } 
//请求头是：Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.71 Safari/537.36

```

如果请求头中没有会报错。 可以指定不要求和默认值

```java 
    @RequestMapping(value = "Header")
    public String pathVariableTest(@RequestHeader(value = "dd-Agent",required = false,defaultValue = "你没带")String userAgent) {
        System.out.println("请求头是：" + userAgent);
        return "/html/welcome.html";
    }
```

#### @CookieValue

获取请求中的某个Cookie

### 传入POJO自动封装

如果传入的参数是一个POJO，SPringMVC会自动地为这个Pojo进行赋值。

1、将POJO的每一个属性从request参数中尝试获取出来，并封装

2、可以级联封装 

3、POJO中的属性名要和input中的名称相同

```java 
<form action="book" method="post">
    书名: <input type="text" name="bookname"/> <br>
    作者: <input type="text" name="author"/> <br>
    价格: <input type="text" name="price"/> <br>
    库存: <input type="text" name="stock"/> <br>
    销量: <input type="text" name="sales"/> <br>
    作者的城市：<input type="text" name="address.city">
    作者的街道: <input type="text" name="address.road">
    作者的门牌号 :<input type="text" name="address.num">
    <input type="submit"/>
</form>
        
  @RequestMapping("/book")
    public String bookShop(book books){
        System.out.println(books);
        return "redirect:/html/info.html" ;
    }
```

### 数据输出

> 在传统的Servlet中，要想把数据带给页面，可以通过request域，Session域，ServletContext域来实现。 

https://www.cnblogs.com/2015110615L/p/5629461.html

SpringMVC通过@RequestMapping将请求引导到处理方法上，使用合适的方法将请求消息绑定到方法入参上，方法入参绑定是请求消息只是处理方法的第一步，还有更重要的任务要完成：根据入参执行相应的逻辑，产生模型数据，并导入到特定的视图中。 

如何将模型数据暴露给视图是 SpringMVC 框架的一项重要工作，SpringMVC 提供了多种途径输出模型数据：

1. **ModelAndView**：处理方法返回值类型为 ModelAndView时，方法体即可通过该对象添加模型数据。

2. **@ModelAttribute**：方法入参标注该注解后，入参的对象就会放到数据模型中。

3. **Map 及 Model：**入参为 org.springframework.ui.Model、org.soringframework.ui.ModelMap 或 java.util.Map 时，处理方法返回时，Map中的数据会自动添加到模型中。

4. **@SessonAttribute：**将模型中的某个属性暂时存到 HttpSession 中，以便多个请求之间可以共享这个属性。

#### model，modelMap，Map

>  SpringMVC 在调用方法前会创建一个隐含的模型对象，作为模型数据的存储容器，我们称之为 “隐含模型”。如果处理方法的入参为 Map 或 Model 类型，SpringMVC 会将隐含模型的引用传递给这些入参。在方法体内，开发者可以通过这个入参对象访问到模型中的所有数据，也可以向模型数据中添加新的属性数据。 

```java

@Controller
public class dataOutputController {
    @RequestMapping("/map")
    public  String MapTest(Map<String,Object> map) {
        map.put("msg","你好") ;
        System.out.println("map的类型为："+map.getClass());
        return "success" ;
    }

    @RequestMapping("model")
    public String ModelTest(Model model) {
        model.addAttribute("msg","model来的") ;
        System.out.println("model的类型为："+model.getClass());
        return "success";
    }

    @RequestMapping("modelMap")
    public String ModelMapTest(ModelMap modelMap) {
        modelMap.addAttribute("msg","来的是modelMap") ;
        System.out.println("modelMap的类型为："+modelMap.getClass());
        return "success";
    }

}

modelMap的类型为：class org.springframework.validation.support.BindingAwareModelMap
model的类型为：class org.springframework.validation.support.BindingAwareModelMap
map的类型为：class org.springframework.validation.support.BindingAwareModelMap
```

从输出可以看到三个最终的实现都是Spring中的BindingAwareModelMap类

#### modelAndView

控制器方法的返回值如果是modelAndView，则既包含视图信息，又包含模型数据信息。 这样springmvc就可以使用视图对模型数据进行渲染了，可以简单地将模型数据看作一个Map<String,Object>

**添加模型对象**

- MoelAndView addObject(String attributeName, Object attributeValue
- ModelAndView addAllObject(Map<String, ?> modelMap)

**设置视图:**

- void setView(View view)
- void setViewName(String viewName)
  也可以在对象创建时通过构造器设置视图

```java 
    @RequestMapping("modelAndView")
    public ModelAndView modelAndViewTest() {
      // 用有参构造器可以直接指定视图的名称
        //success.jsp页面
        ModelAndView modelAndView = new ModelAndView("success");
        //用无参构造器可以用set方法设置视图
        //   ModelAndView modelAndView2 = new ModelAndView();
        //modelAndView2.setView("success") ;
        modelAndView.addObject("msg","modelAndView");
        return modelAndView;
    }
```

#### @SessionAttributes

如果希望多个请求共用某一个模型属性数据，可以在控制器类上标注@SessionAttributes

标注了@SessionAttributes后，spring给BindingAwareModelMap中保存的数据，或者在modelAndView中的数据，都会同时在HTTPsession中放一份。

`@SessionAttributes(value =  {"msg"},types = String.class)`

value的值指定了要保存数据的key，只要key对应的值，session都会备份

type：只要保存的是该类型的数据，session都备份。 

```java
 @RequestMapping("model")
    public String ModelTest(Model model) {
        // session保存
        model.addAttribute("msg", "model来的");
//        保存的是整型值 ，session不保存
        model.addAttribute("haha", 18);
        //保存的是string类型，session保存
         model.addAttribute("haha", "哈哈");
        System.out.println("model的类型为："+model.getClass());
        return "success";
    }
```



#### modeAttribute

①、 被ModelAttribute注解的方法会在此controller每个方法执行前都被执行一次，因此对于一个controller映射多个url的用法来说，要谨慎。



②、

```Java
 @ModelAttribute 
public Account addAccount(@RequestParam String number) { 
     return accountManager.findAccount(number); 
 } 
```



### 视图解析

SpringMVC 定义了ViewResolver和View接口，可以在浏览器中呈现模型， 而无需与特定的视图技术联系在一起。 ViewResolver 提供视图名称和实际视图的页面。View接口负责准备请求，并将请求的渲染叫给某种具体的视图技术实现 



#### View

视图：作用是渲染模型数据，将模型里的数据以某种形式呈现给客户。

View是视图基础接口，它的各种实现类都是无状态的，所以是线程安全的。

视图对象由视图解析器负责实例化。



![image-20211113192803842](D:\Typora\自服务\javaWeb.assets\image-20211113192803842.png)

 **常用的视图实现类**

![image-20211113194712170](D:\Typora\自服务\javaWeb.assets\image-20211113194712170.png) 



#### ViewResolver

![image-20211113194254531](D:\Typora\自服务\javaWeb.assets\image-20211113194254531.png)

- springmvc为逻辑视图名的解析提供了不同的策略，可以在springweb上下文中配置一种或多种解析策略，并指定他们的先后顺序，每一种映射策略对应一个具体的视图解析器实现类。

- 所有视图解析器都必须实现ViewReslover接口

- 程序员可以选择一种视图解析器或混用多种视图解析器

- 每个视图解析器都实现了 Ordered 接口并开放出一个 order 属性，可以通过 order 属性指定解析器的优先顺序，order 越小优先级越高。

- SpringMVC 会按视图解析器顺序的优先顺序对逻辑视图名进行解析，直到解析成功并返回视图对象，否则将抛出 ServletException 异常

  

常用的视图解释器类 ![img](https://img2020.cnblogs.com/blog/1542615/202008/1542615-20200813164702080-1758718168.png) 



#### 重定向

一般情况下，控制器方法返回字串类型的值会被当作逻辑视图名处理

如果返回的字符串中带有forward：或redirect：前缀时，springmvc会对他们进行特殊处理。

- redirect:hello.jsp 会完成一个到success.jsp的重定向操作
- foward:hello.jsp会完成一个success.jsp的转发操作。

> 重定向指当浏览器请求一个URL时，服务器返回一个重定向指令，告诉浏览器地址已经变化了，麻烦使用新的URL再重新发送新请求。
>
> 重定向有两种：1是302响应，称为临时重定向，2是301响应，称为永久重定向，两者的区别是：如果服务器发送301永久重定向响应，浏览器会缓存这个重定向的关联，下次请求时，直接发送新地址。 
>
> Forward是指内部转发，当一个Servlet处理请求时，它可以决定自己不继续处理，转发给另外一个Servlet处理。
>
> 转发和重定向的区别在于：转发是在Web服务器内完成的，对于浏览器来说，它只发出了一个Http请求。 

#### 源码

①、controller任何方法的返回值，最终都会被封装成modelAndView

- SpringMVC借助视图解析器得到最终的视图对象View，最终的视图可以是JSP，Excel，JFreeChart等各种表现形式的视图。

```java
mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
```

②、来到页面的方法

```java
	processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
```

③、使用render函数渲染页面：

```java
//Render the given ModelAndView.
//This is the last stage in handling a request. It may involve resolving //the view by name
render(mv, request, response);
```

④、

```java 
//render函数内
View view;
            if (mv.isReference()) {
                // We need to resolve the view name.
                view = resolveViewName(mv.getViewName(), mv.getModelInternal(), locale, request);
                if (view == null) {
                    throw new ServletException("Could not resolve view with name '" + mv.getViewName() +
                            "' in servlet with name '" + getServletName() + "'");
                }
            }
```

⑤、

```java 
protected View resolveViewName(String viewName, Map<String, Object> model, Locale locale,
			HttpServletRequest request) throws Exception {
 	     // viewReslovers：SPring的9大组件之一。
		for (ViewResolver viewResolver : this.viewResolvers) {
            		// 根据视图解析器的返回值，返回view对象。
			View view = viewResolver.resolveViewName(viewName, locale);
			if (view != null) {
				return view;
			}
		}
		return null;
	}
```



创建一个view对象：

```java 
protected View createView(String viewName, Locale locale) throws Exception {
	
		// Check for special "redirect:" prefix.
    		// 以redirect为前缀的，创建一个RedirectView对象 
		if (viewName.startsWith(REDIRECT_URL_PREFIX)) {
			String redirectUrl = viewName.substring(REDIRECT_URL_PREFIX.length());
			RedirectView view = new RedirectView(redirectUrl, isRedirectContextRelative(), isRedirectHttp10Compatible());
			view.setHosts(getRedirectHosts());
			return applyLifecycleMethods(viewName, view);
		}
		// Check for special "forward:" prefix.
                       // 以forward为对象的，创建一个InternalResourceView对象
		if (viewName.startsWith(FORWARD_URL_PREFIX)) {
			String forwardUrl = viewName.substring(FORWARD_URL_PREFIX.length());
			return new InternalResourceView(forwardUrl);
		}
    		// 创建一个默认对象。
		// Else fall back to superclass implementation: calling loadView.
		return super.createView(viewName, locale);
	}
```



### REST风格

系统希望以简洁的url地址来发起请求，通过http的请求方式来区分。 

Representational State Transfer资源表现层状态转化，是一种互联网软件架构

思想：认为网络中一切请求都是请求资源（万物介资源），并操作资源，就是增删改查。

R：Resources，资源层，网络上所有的都是资源，URL就是资源的独一无二的标识符

R：Representation，表现层，资源的表现状态，如html格式，xml格式，json格式等

State Transfer：状态层，状态转化。 每发出一个请求，就代表客户端和服务器的一次交互过程，http是一个无状态协议，所有的状态都保存在服务器端，如果客户端想操作服务器，就必须要通过某种手段，让服务器发生状态转化。 而这种转化是建立在表现层上的，就是表现层状态转化。就是http协议里，四个表示操作方式的动词：GET，POST，PUT，DELETE。她们分别代表了获取资源，新建资源，更新资源





RESTful的优点：

> 传统的MVC框架，前后端的部署是一起的，服务器接收请求，去数据库查询然后计算，然后和jsp进行组装渲染，直接返回一个完整的html页面给浏览器，在RESTful结构下，前后端是可以分离的， 浏览器访问一个页面，前端根据需要向后端发送HTTP请求，后端接收请求后查询计算，再把结果封装为JSON报文返回给前端，前端把数据组装到静态页面中，生成最终展示给用户的页面
>
> 



### SpringMvc-CRUD



### springmvc表单

https://blog.csdn.net/J080624/article/details/56501910

SpringMvc提供了一个表单标签，可以实现模型数据中的属性（bean）和html表单元素属性绑定，以实现表单数据更便捷地编辑和表单回显。

使用：

导入：

```xml
<%@ taglib uri="http://www.springframework.org/tags/form" prefix="form"%>
指定了前缀为prefix 
```

### 番外：form标签

https://www.jianshu.com/p/6dee6181ac24

表单form可以把浏览器输入的数据传送到服务器端。 

属性：

- action：指定提交到服务器那个地方（springmvc一般提交到控制器处理）
- method：post和get两个取值 

```xml
<form:form action="emp" method="POST" modelAttribute="command">
```

🐍input标签中id和name的区别🐍

id的作用：

1. 用id选择相应的style sheet风格
2. <A..>链接的目的的
3. 脚本语言找到该id的标签

name原本作为标识作用，但现在根据规范， 都建议用id标识元素，但name在以下用途是无法替代的：

1. form表单的控件名，提交的数据都用控件的name而不是id来控制，因为有很多name同时对应多个控件，而id是唯一的。浏览器根据name来设定发送到服务器的request，因此如果有id，服务器是无法得到数据的。





------------

> 看不懂的

- modelAttribute: SpringMVC认为，表单数据中的每一项最终都是要回显的，

  path指定的每一个属性，都和请求域中的一个对象对应。这个对象默认为command 

  modelAttribute属性可以指定该from绑定的是那个model，当指定了对应的Model后就可以在from标签内部其他表单标签通过为path指定Moedl属性的名称来绑定Model中的数据。



```xml
 <form:input path="username"/> 
```

- 会生成一个tpye为text的html input标签. 通过path属性来指定要绑定的Model中的值。 

```xml 
<form:radiobutton path="ifno"  />
```

- 单选框组件标签，当标签bean对应的属性值和value值相等时，单选框被选中 

```xml 
<form:select path="department.id" items="${departments }" 
itemLabel="departmentName" itemValue="id"></form:select>
```

会生成一个html Select标签，绑定的items数据可以是数组，集合或map，根据items的内容生成select里面的option选项，

当path的值和items中的某条数据值相同时对应的option处于选定状态。

- path：属性
- items：数据
- itemLable：每一项的名字
- itemValue：选项的值，可为bean的属性 





```xml 
<%@page import="java.util.HashMap"%>
<%@page import="java.util.Map"%>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://www.springframework.org/tags/form" prefix="form"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>添加员工</title>
</head>
<body>
    <!--
        1.Why 使用 form 标签呢?
            可以更快速的开发出表单页面,而且可以更方便地进行表单值的回显.
        2.注意
            可以通过 modelAttribute 属性指定绑定的模型属性,
            若没有指定该属性,则默认从 request 域对象中读取 command 的表单bean
            如果该属性值也不存在,则会发生错误.
     -->
    <form:form action="emp" method="POST" modelAttribute="employee">
        <!-- path 属性对应 html 表单标签的 name 属性值 -->
        LastName:<form:input path="lastName" />
        <br>
        Email:<form:input path="email" />
        <br>
        <%
            Map<String, String> genders = new HashMap<>();
                genders.put("男", "Male");
                genders.put("女", "Female");
                request.setAttribute("genders", genders);
        %>
        Gender:<br>
        <form:radiobuttons path="gender" items="${genders}" delimiter="<br>" />
        <br>
        <!--
        items:指定要遍历的集合，自动遍历，每一个元素都是department对象
        itemsLabel：属性，指定遍历出的这个对象哪个属性作为option标签体的值。这里为departmentName
        itemValue:属性名：指定遍历出的这个对象哪个属性作为要提交的值。

        -->
        Department:<form:select path="department.id" items="${departments }"
            itemLabel="departmentName" itemValue="id"></form:select>
        <br>
        <input type="submit" value="Submit">
    </form:form>
</body>
</html>
```



#### post 👉get

https://www.jianshu.com/p/6dee6181ac24

本质上是语义的区别，根据http的规范，get的语义是请求获取指定的资源，get方法是安全的，幂等的，可缓存的。post语义是根据请求负荷（报文主体）对指定的资源做出处理，具体的处理方式看资源类型而不同，post不安全，不幂等，（大部分实现）不可缓存。 

> 具体差别如下：
>
> - get在后退刷新时是无害的，post会重新提交请求；
> - get参数通过URL传递，post放在Request body中；
> - get请求参数保留在浏览器历史记录中，post参数不会保留；
> - get产生的URL地址可以被存为书签，而post不可以；
> - 对参数的数据类型，get只接受ASCII字符，而post没有限制；
> - get比post更不安全，因为发送的数据显示在URL上，在发送密码或其他敏感信息时绝不要使用get;
> - get请求只能进行url编码，而post支持多种编码方式。

### 数据转换

Http请求传递的数据都是字符串String类型的。

```java
 @RequestMapping("hello")
    public String hello(Integer num,Date birth) { 
        return "hello" ;
    }
```

如上面的方法，SpringMVC会自动将num转换成integer对象，birth会自动转换Date对象（Date转换需要配置属性编辑器）

1. SpringMVC将Servlet对象及其处理方法的参数对象实例传递给WebDataBinderFactory实例，以创建DataBinder实例对象
2. DataBinder会调用装配在SpringWeb上下文中的ConversionService 组件进行数据类型转换，数据格式化工作，并将ServletRequest中的消息填充到参数对象中。 
3. 然后调用Vaildator组件对已经绑定了请求消息数据的参数对象进行数据合法性校验，并最终生成数据绑定结果BindingResult对象。

4. BindResult对象包含已完成数据绑定的参数对象，还包括相应的校验错误对象，SpringMVC抽取BindingResult中的参数对象以及校验错误对象，将他们赋值给处理方法的相应参数。 

SpringMVC 通过反射机制对目标处理方法进行解析，将请求消息绑定到处理方法的入参中。数据绑定的核心部件是DataBinder，运行机制如下： 

![image-20211107165211667](D:\Typora\自服务\javaWeb.assets\image-20211107165211667.png)

#### 自定义数据绑定

Spring定义了3种类型的转换器接口，实现任意一个转换器接口都可以作为自定义转换器注册到ConversionServiceFactoryBean中：

- Converter<S,T> 将S类型对象转换为T类型对象
- ConverterFactory：将相同系列多个同质Converter封装在一起，如果希望将一种类型的对象转换到另外一种类型及其子类的对象， 例如String->Number，可以使用该转换器工厂类。

mvc:annotation-driven的使用

> 在Spring3.0使用了了mvc:annotation-driven后，默认会帮我们注册默认处理请求，参数，和返回值的类，其中最主要的两个类： DefaultAnnotationHandlerMapping,和AnnotationMethodHandlerAdapter ，分别为HandlerMapping的实现类和HandlerAdater的实现类， 从3.1.x版本开始对应实现类改为了RequestMappingHandlerMapping和RequestMappingHandlerAdapter。 



#### 数据校验





### 文件上传

前置知识：🐺MIME类型：🐺

服务器需要将发送的多媒体数据的类型告诉浏览器，而告诉浏览器的手段就是告知多媒体的mime类型。

> SpringMVC为文件上传提供了直接的支持，这种支持是由MultipartResolver实现的。 SpringMVC使用Apache Commons FileUpload技术实现了一个`MultipartResolver`实现类：`CommonsMultipartResolver`。因此，SpringMVC的文件上传还需要依赖Apache Commons FileUpload的组件。 

使用步骤：

①、导入依赖

```xml 
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.3</version>
    </dependency>
    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.2</version>
    </dependency>
```

②、配置文件上传的bean

```xml 
    <!-- SpringMVC上传文件时，需要配置MultipartResolver处理器 -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="defaultEncoding" value="UTF-8" />
    </bean>
```

另：确定bean的ID是在DispatcherServlet中看的：在initStrategies#initMultipartResolver中`this.multipartResolver = context.getBean(MULTIPART_RESOLVER_BEAN_NAME, MultipartResolver.class);` MULTIPART_RESOLVER_BEAN_NAME的默认值就是multipartResolver 

③、准备表单：

```html 
<%
    pageContext.setAttribute("ctp",request.getContextPath());
%>
<form action="${ctp}/upLoadFile"  method="post" enctype="multipart/form-data">
    用户头像：<input type="file" name="headImg"/> <br/>
    用户名 : <input type="text" name="userName"/><br/>
    <input type="submit" >
</form>
<p>${msg}</p>
</body>

```

解释：

🍌 pageContext对象是javax.servlet.jsp.PageContext类的实例对象，用来代表整个jsp页面上下文，该对象主要用于访问jsp之间的共享数据，可以用来访问 page,request,session,application范围的变量。这里是用来访问当前项目的路径 

http://www.51gjie.com/javaweb/831.html

🍌enctype:设置为multipart/form-data时，浏览器将上传的每个文件或附件与“多部份边界”分开，“多部份边界”是定义每个部分开头和结尾的唯一标识符。

https://cloud.tencent.com/developer/ask/216486

③、准备控制器：

```java 
@Controller
public class UpLoadController {
    @RequestMapping("/upLoadFile")
    //springmvc会将表单的input标签的属性name提交到这里
    public String upLoadFile(Model model, @RequestParam(value="userName",required = false) String userName,
                             @RequestParam("headImg") MultipartFile file) {
        //获取file的一些属性。 
        System.out.println(file.getName()); //获取到的是input标签name属性的值，这里是headImg
        System.out.println(file.getOriginalFilename()); //获取到的是文件原名。
        try {
            //将文件保存到D盘，命名以文件原名的形式 
            file.transferTo(new File("D:\\"+file.getOriginalFilename()));
            model.addAttribute("msg","文件上传成功🚗");
        } catch (IOException e) {
            e.printStackTrace();
            model.addAttribute("msg","文件上传失败👴👴");
        }
        return "forward:/index.jsp";
    }
}
```

<img src="D:\Typora\自服务\javaWeb.assets\image-20211216162815638.png" alt="image-20211216162815638" style="zoom:50%;" />

🐕多文件上传🐕

```java 
弄成数组即可                     
@RequestParam("headImg") MultipartFile []file) {
        for(MultipartFile files:file){
            try {
                if(files.isEmpty()) continue ; 要判断是不是空的
                files.transferTo(new File("D:\\"+files.getOriginalFilename()));
                model.addAttribute("msg","文件上传成功🚗");
            } catch (IOException e) {
                e.printStackTrace();
                model.addAttribute("msg","文件上传失败👴👴");
            }
        }  
    ------------------------------------🍛🍛🍛🍛🍛🍛🍛🍛🍛🍛html————————————————————
    portrait<input type="file" name="headImg"/> <br/>
    portrait<input type="file" name="headImg"/> <br/>
    portrait<input type="file" name="headImg"/> <br/>
    portrait<input type="file" name="headImg"/> <br/>
    userName<input type="text" name="userName"/><br/>
```

### 异常处理

在DispatcherServletProperties文件中有：

```
org.springframework.web.servlet.HandlerExceptionResolver=
AnnotationMethodHandlerExceptionResolver,\
ResponseStatusExceptionResolver,   标有@ResponseStatus   
DefaultHandlerExceptionResolver  判断是否为SpringMVC自带的异常 
```

```java 
//编写控制器，如果传入的id是0，那么就会出异常。  
@RequestMapping("handle01")
    public String handler01(@RequestParam("id") Integer id ) {
        System.out.println("handler");
        System.out.println(10/id);
        return "success" ;
    }
```

**🌧调试分析🌧**

```Java 
//DispatcherServlet.java 文件
protected ModelAndView processHandlerException(HttpServletRequest request, HttpServletResponse response,
			Object handler, Exception ex) throws Exception {

		// Check registered HandlerExceptionResolvers...
		ModelAndView exMv = null;
		for (HandlerExceptionResolver handlerExceptionResolver : this.handlerExceptionResolvers) {
            //遍历所有的异常解析器，尝试用它们去处理，解析不了下一个。 
			exMv = handlerExceptionResolver.resolveException(request, response, handler, ex);
			if (exMv != null) {
				break;
			}
		}
    //如果不是空，也就是上面的能够处理，那就继续执行
		if (exMv != null) {
			if (exMv.isEmpty()) {
				request.setAttribute(EXCEPTION_ATTRIBUTE, ex);
				return null;
			}
			// We might still need view name translation for a plain error model...
			if (!exMv.hasView()) {
				exMv.setViewName(getDefaultViewName(request));
			}
			if (logger.isDebugEnabled()) {
				logger.debug("Handler execution resulted in exception - forwarding to resolved error view: " + exMv, ex);
			}
			WebUtils.exposeErrorRequestAttributes(request, ex, getServletName());
			return exMv;
		}
//如果是空的，也就是都不能处理，那就抛出去
		throw ex;
	}
```

#### 注解ExceptionHandler

注解ExceptionHandler作用对象为方法，告诉SpringMVC这个方法专门用于处理这个类的所有（特定）异常

```java 
 /**
     * 告诉SpringMVC这个方法专门用来处理这个类中的所有异常
     * 方法入参只能有Exception，只能识别这种
     * 用返回ModelAndView即可
     */
    @ExceptionHandler(value={ArithmeticException.class})
    public ModelAndView ExceptionHandler01(Exception e) {
        System.out.println(e);
        ModelAndView modelAndView = new ModelAndView("error");
        modelAndView.addObject("ex",e) ;
        return modelAndView;
    }
```





@ResponeStatus 

@Simple

### SpringMVC运行流程

1. **所有请求DispatcherServlet收到后调用doDispatch方法。**

2. **根据handlerMapping中保存的请求映射信息找到当前处理当前请求的处理器执行链。包含拦截器**
3. **根据当前处理器找到处理器适配器**
4. **拦截器的preHandler先执行**
5. **处理器适配器执行目标方法**
   1. ModelAttribute注解标注的方法提前运行
   2. 执行目标方法前（确定目标方法的参数
      1. 有注解
      2. 没有注解 
         1. 看是否为Model，Map以及其他的
         2. 如果是自定义类型
            1. 从隐含模型中看，如果有就从隐含模型中拿
            2. 如果没有，就看是否为SessionAttributes标注的属性，如果是，从Session中拿，否则报异常。
            3. 都不是，就利用反射对象
6. **拦截器的postHandler执行**
7.  **处理结果（页面渲染流程**
   1. **如果有异常使用异常解析器处理异常，处理完后还会返回ModelANdView** 
   2. **使用render进行页面渲染**
      1. 视图解析器根据视图名得到视图对象
      2. 视图对象调用render方法
   3. 执行拦截器的afterCompletion 





sh