# springboot 上

### 自定义SpringMVC

> If you want to keep those Spring Boot MVC customizations自定义 and make more  (interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`. （不使用@EnableWebMvc注解，使用@Configuration+WebMvcConfigurer自定义规则
>
> If you want to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, and still keep the Spring Boot MVC customizations, you can declare a bean of type `WebMvcRegistrations` and use it to provide custom instances of those components.    使用WebMvcRegistrations改变默认底层组件
>
> If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`, or alternatively add your own `@Configuration`-annotated `DelegatingWebMvcConfiguration` as described in the Javadoc of `@EnableWebMvc`.    使用@EnableWebMvc+@Configuration+DelegatingWebMvcConfiguration全面接管springMVc

### 静态内容

在开发web应用中需要用到大量的js，css，image，html等静态资源

>  By default, Spring Boot serves static content from a directory called `/static` (or `/public` or `/resources` or `/META-INF/resources`) 
>
>  意思是只要类路径下有`/static` or 或 `/public `或` resources `或 `/META-INF/resources `,SpringBoot就会自动识别出来

在默认情况下我们只需要将静态资源放在以上几个目录下，就可以通过url直接访问到

静态资源的默认访问优先级：`/META-INF/resources/`>`/resources/`>`/static/`>`/public/` 

**自定义静态资源的路径：**

*可以在yaml文件中配置改变默认访问的规则：*

```yaml
#配置静态资源的前缀，访问的时候就要 ： localhost:8080/res/xxx.jpg 有个res 
spring:
  mvc:
    static-path-pattern : /res/**

#配置一个指定的自定义包，访问的时候只能访问该包内的资源。 
  resources:
    static-locations: [classpath:/abc/] 
```



### 欢迎页

> Spring Boot supports both static and templated welcome pages. It first looks for an `index.html` file in the configured static content locations. If one is not found, it then looks for an `index` template. If either is found, it is automatically used as the welcome page of the application.
>
> springBoot支持两种欢迎页： 一是静态资源下的index.html， 二是寻找一个index模板。

在静态资源路径下寻找一个 index.html 

#### favicon

springboot可以给你的网站增加一个图标，只需要将一个图标命名为 favicon.ico ，放在静态资源的目录下，springboot会自动给你加载。



#### 静态资源源码

拓展知识：配置类只有一个有参构造器时，说明这个类的所有参数都会从容器中找。 

```java 
 /**
 WebMvcProperties 获取和Spring.web.mvc绑定的所有值的对象
 WebProperties 获取Spring.web绑定的所有值的对象
HttpMessageConverters
 ListableBeanFactory spring中的beanFactory 
 */
	public WebMvcAutoConfigurationAdapter(WebProperties webProperties, WebMvcProperties mvcProperties,
				ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider,
				ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
				ObjectProvider<DispatcherServletPath> dispatcherServletPath,
				ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
			this.resourceProperties = webProperties.getResources();
			this.mvcProperties = mvcProperties;
			this.beanFactory = beanFactory;
			this.messageConvertersProvider = messageConvertersProvider;
			this.resourceHandlerRegistrationCustomizer = resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
			this.dispatcherServletPath = dispatcherServletPath;
			this.servletRegistrations = servletRegistrations;
			this.mvcProperties.checkConfiguration();
		}
```

WebMvcAutoConfigruation.java的方法. 

```java 
	@Override
		public void addResourceHandlers(ResourceHandlerRegistry registry) {
            //判断yaml中的spring.web.resource.addmapping 是否为false,如果是,那就是禁用掉所有的默认静态资源配置,直接return . 
			if (!this.resourceProperties.isAddMappings()) {
				logger.debug("Default resource handling disabled");
				return;
			}
            //webjars的相关规则,定义怎么去找webjars的资源:META-INF ..
			addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
            //静态资源的相关规则,定义了静态资源放的位置:
            //getStaticLocations ----> private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/" };
	
            addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
				registration.addResourceLocations(this.resourceProperties.getStaticLocations());
				if (this.servletContext != null) {
					ServletContextResource resource = new ServletContextResource(this.servletContext, SERVLET_LOCATION);
					registration.addResourceLocations(resource);
				}
			});
		}

```

```yaml 
spring:
  web:
    resources:
      add-mappings: false 
```

```java 
//欢迎页的HandlerMapping	
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
                                                           FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
            // 自己new了一个 
            WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
                new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
                this.mvcProperties.getStaticPathPattern());
            welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
            welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
            return welcomePageHandlerMapping;
		}

//构造方法
WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders,
                          ApplicationContext applicationContext, Resource welcomePage, String staticPathPattern) {
    if (welcomePage != null && "/**".equals(staticPathPattern)) {
        logger.info("Adding welcome page: " + welcomePage);
        setRootViewName("forward:index.html");
    }
    else if (welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
        logger.info("Adding welcome page template: index");
        setRootViewName("index");
    }
	}
```



### 请求处理

>  访问映射：在浏览器中输入http://localhost:8080/index，其中/index的请求映射到某方法进行处理或者映射到某个页面
>  访问映射又两种方式，控制器和配置类。



#### 源码

**Rest风格的开启 ：** 

```java
//WebMvcAutoConfiguration.java 
@Bean
@ConditionalOnMissingBean(HiddenHttpMethodFilter.class) //默认为没有配这个class就生效
	@ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled", matchIfMissing = false) //就是需要在配置文件中开启才有，默认关闭,所以要在yml文件配置
	public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
		return new OrderedHiddenHttpMethodFilter();
	}

```

 

Rest原理（表单提交要用REST的时候）：

1.  表单提交带上_method = PUT
2.  请求会被HiddenHttpMethodFilter拦截
3.  检查是否为Post且无异常，获得_method的值
    1. 原生request(post) ,使用了包装模式重写了getMethod方法

![image-20210131102255582](D:\Typora\自服务\javaWeb.assets\image-20210131102255582.png)

#### 示例

前端：提交表单

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

分析源码可以知道，想要实现delete和put功能，在前端必须将请求方法写成post，并且input的name属性的值为_method 

后端： Controller类

```java
@RestController
public class HelloController {

    @RequestMapping("/sky.jpg")
    public String hello(){
        return "hello";
    }
//    @RequestMapping(value = "/user" ,method = RequestMethod.GET)
    @GetMapping()  // 这个注解可以替换上面的那个 
    public String getUser() {
        return "GET_张三";
    }
    //    @RequestMapping(value = "/user",method = RequestMethod.POST)
    @PostMapping()
    public String SaveUser() {
        return "POST张三";
    }
    @PutMapping()
    public String putUser(){
        return "PUT-张三";
    }
    @DeleteMapping()
    public String deleteUser(){
        return "DELETE-张三";
    }
}

```

```yaml
#后端要开启功能，默认为false
spring:
  mvc:
    hiddenmethod:
      filter:
        enabled : true  #开启页面表单的Rest功能 
```

想要改变前端提交时默认的值可以自定义一个HiddenHttpMethodFilter类，通过set方法修改参数

```java
@Configuration(proxyBeanMethods = false)
public class WebConfig {
    public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();
        hiddenHttpMethodFilter.setMethodParam("_meet");
        return hiddenHttpMethodFilter;
    }
}
```



### 一些注解

#### @PathVariable

@PathVariable 映射 URL 绑定的占位符

- 带占位符的 **URL** 是 **Spring3.0** 新增的功能，**该功能在SpringMVC 向 REST 目标挺进发展过程中具有里程碑的意义**
- 通过 **@PathVariable** 可以将 **URL** 中占位符参数绑定到控制器处理方法的入参中：URL 中的 {**xxx**} 占位符可以通过@PathVariable(“**xxx**“) 绑定到操作方法的入参中。

示例：

```java
   //在浏览器中 URL中访问：car/2/owner/zhangsan
   //或者前端连接： <a href="car/2/owner/zhangsan">有这种事?</a>
@RestController
public class pathController {
    //方法参数和占位符一一对应
  @GetMapping("/car/{id}/owner/{username}")
    public Map<String,Object> getCar(@PathVariable("id") Integer id,
                                     @PathVariable("username") String username,
                                     @PathVariable Map<String,String> kv) {
        HashMap<String ,Object> map = new HashMap<>();
        map.put("id",id);
        map.put(username,username);
        map.put("kvmap",kv);
        return map ;
    }
/**
 * 最后一个Map<String,String>是该注解提供一个将所有参数
 * 放到一个map的功能 , 详见源码注释
 */
```

#### @RequestHeader

Spring MVC提供@RequestHeader注解方便我们获取请求头信息。

```java
@RestController
public class pathController {
    @GetMapping("/car")
    public Map<String,Object> getCar(         
                                     @RequestHeader ("User-Agent") String userAgent,
                                     @RequestHeader Map<String,String> header) {
        HashMap<String ,Object> map = new HashMap<>();
        map.put("userAgent", userAgent) ; // 用户代理
        map.put("header", header) ; //所有的请求头，详见源码注释 
        return map ;
    }
}
```

#### @RequestParam

传递参数 用于将请求参数区数据映射到功能处理方法的参数上

```java
 public Map<String,Object> getCar(
                                     @RequestParam ("age" )Integer age ,
                                     @RequestParam ("hobbies") List<String> hobbies,
                                     @RequestParam Map<String,String> parmmap
                                     ) {
        HashMap<String ,Object> map = new HashMap<>();
        map.put("age",age) ;
        map.put("hobbies",hobbies) ;
        map.put("map",parmmap);
        return map ;
    }

在前端/url中可以搜索：
 <a href="car/2/owner/zhangsan?age=17&hobbies=reading&hobbies=game">点我</a>
```

#### @CookieValue

用于将请求的Cookie数据映射到功能处理方法的参数上

```java
 public Map<String,Object> getCar(
                                     @CookieValue ("_ga") Cookie cookie
    		         @CookieValue ("_ga") String  _cookie
                                     ) {
    
        System.out.println(cookie);
           System.out.println(_cookie);
        return map ;
    } 
// 两种方式，见源码注释、用法 
```



#### @RequestBody

>  @RequestBody主要用来接收前端传递给后端的json字符串中的数据的(请求体中的数据的)；
>
>  GET方式无请求体，所以使用@RequestBody接收数据时，前端不能使用GET方式提交数据，而是用POST方式进行提交。
>
>  在后端的同一个接收方法里，@RequestBody与@RequestParam()可以同时使用，@RequestBody最多只能有一个，而@RequestParam()可以有多个。 
>
>  注：一个请求，只有一个RequestBody；一个请求，可以有多个RequestParam。

https://blog.csdn.net/justry_deng/article/details/80972817

```java 
   @PostMapping("/save")
    public Map<String,Object> getPost(@RequestBody String content) {
        HashMap<String ,Object> map = new HashMap<>();
        map.put("content",content);
        return map ;
    }
```



```html 
<form action="/save" method="post">
    测试 @RequestBody 获取数据</br>
    用户名 : <input name="userName"/> <br/>
    邮箱 : <input name="email" />
    <input type="submit" value="提交"/>
</form>
```



#### @RequestAttribute 

> @RequestAttribute用在方法入参上，作用：从request中取对应的值，主要是转发时用的。
>
> 至于request中是怎么存在该属性的，方式多种多样，可以是拦截器中预存、ModelAttribute注解预存、请求转发带过来的；

https://segmentfault.com/a/1190000020013935

```java
@Controller
public class RequsetController {
    @GetMapping("/goto")
    public String goToPage(HttpServletRequest request ){
        request.setAttribute("msg","成功了！");
        request.setAttribute("code",201);
        return "forward:/success" ;
    }

    @ResponseBody
    @GetMapping("/success")
    public Map success(@RequestAttribute("msg") String msg,
                       @RequestAttribute("code") Integer code,
                       HttpServletRequest request) {
        Map<String,Object> map=new HashMap();
        Object msg1 = request.getAttribute("msg");

        map.put("msg1",msg1);
        map.put("code",code) ;
        return map;
    }
}

```

#### @MatrixVariable

>  RFC3986 定义了在 URI 中包含 name-value 的规范。随之在 Spring MVC 3.2 中出现了 @MatrixVariable 注解，该注解的出现使得开发人员能够将请求中的矩阵变量（MatrixVariable）绑定到处理器的方法参数中。而 Spring 4.0 更全面地支持这个规范，这也是 Spring 4.0 众多吸引人的新特性之一。接下来我们就一起来了解这个新特性的使用方式。 

矩阵变量需要在SpringBoot中手动开启。

根据规范，矩阵变量应该绑定在路径变量中，如果有多个矩阵变量，应该是用分号；进行分割。 

```html 
<a href="/cars/sell;low=34;brand=byd,audi,yi">@MartixVariable矩阵变量1</a></br>
<a href="/cars/sell;low=34;brand=byd;brand=audi;brand=yi">@MartixVariable矩阵变量2</a></br>
上面两种写法都可以。
<a href="/boss/1;age=20/2;age=20">@MartixVariable矩阵变量3</a>
```

如何看：

- /boss/{id};age=20中应该把/{id};age=20看作一个整体;即一个路径变量应该和它后面带的矩阵变量作为整体来看。 
- /sell;low=20;brand=byd,audi 多个矩阵变量用;分割

```java 
    //    /cars/sell;low=34;brand=byd,audi,yi
    @GetMapping("cars/{path}")
    public Map SellCars(@MatrixVariable("low") Integer low ,
                        @MatrixVariable ("brand") List<String> brands,
                        @PathVariable("path") String path) {
        HashMap<String ,Object> map=new HashMap<>() ;
        map.put("low",low) ;
        map.put("brand",brands);
        map.put("path",path);
        return map ;
    }

//    <a href="/boss/1;age=20/2;age=20">@MartixVariable矩阵变量3</a>
    @ResponseBody
    @GetMapping("/boss/{bossId}/{EmpID}")
// 如果矩阵变量有重名的，就指定pathVar指明要获取的矩阵变量是那个路径变量后面的。 
    public Map getBoss(@PathVariable("bossId") Integer bossid ,
                       @PathVariable("EmpID") Integer empid,
                       @MatrixVariable(value = "age",pathVar = "bossId") Integer bossage,
                       @MatrixVariable(value="age" ,pathVar = "EmpID") Integer empAge){
        HashMap<String ,Object> map=new HashMap<>() ;
        map.put("bossId",bossid) ;
        map.put("empId",empid) ;
        map.put("bossage",bossage) ;
        map.put("empAge",empAge) ;
        return map ;
    }
```

![image-20211203222702837](D:\Typora\自服务\SpringBoot.assets\image-20211203222702837.png)

在SpringBoot开启矩阵变量的支持，自定义一个WEbMVCCOnfiguration

```Java 
两种方式：1. 自己定义配置类实现WebMvcConfigurer接口，然后重写configurePathMatch方法，添加自定义UrlPathHelper组件
    2. 自己添加WebMvcConfigurer组件，使用springboot的默认实现方式，同样重写configurePathMatch方法，设置。
@Configuration
public class WebConfig   {//implements WebMvcConfigurer

    @Bean
    public WebMvcConfigurer webMvcConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void configurePathMatch(PathMatchConfigurer configurer) {
                UrlPathHelper urlPathHelper=new UrlPathHelper();
                urlPathHelper.setRemoveSemicolonContent(false);
                configurer.setUrlPathHelper(urlPathHelper) ;
            }
        };
    }

//    //开启SpringBOOt对矩阵变量的支持
//    @Override
//    public void configurePathMatch(PathMatchConfigurer configurer) {
//
//        UrlPathHelper urlPathHelper = new UrlPathHelper();
//        urlPathHelper.setRemoveSemicolonContent(false);
//        configurer.setUrlPathHelper(urlPathHelper);
//
//    }
}
```



#### 小说明

前端的表单和后端SpringBoot Controller的交互：

```html
//前端
<form action="/hello" >   #将被提交到hello Controller 
    你好
    <input type="submit" value="提交"/> submit 就是提交到服务器 value是按钮显示的值 
</form>
```

```java
//后端
 @RequestMapping("/hello")
    public String hello() {
        return "hi";
    }
```

### 请求处理源码

#### 1.1invokeHandlerMethod

https://blog.csdn.net/andy_zhang2007/article/details/99689573

在RequestMappingHandlerAdapter类中的invokeHandlerMethod方法中有调用

```java 
//RequestMappingHandlerAdapter.java  
@Nullable
    protected ModelAndView invokeHandlerMethod(HttpServletRequest request, HttpServletResponse response, HandlerMethod handlerMethod) throws Exception {
        // 1. HttpServletRequest/Resopense对象包装成一个ServletWebRequest对象
        ServletWebRequest webRequest = new ServletWebRequest(request, response);

        ModelAndView var15;
        try {
            WebDataBinderFactory binderFactory = this.getDataBinderFactory(handlerMethod);
            ModelFactory modelFactory = this.getModelFactory(handlerMethod, binderFactory);
            ServletInvocableHandlerMethod invocableMethod = this.createInvocableHandlerMethod(handlerMethod);
       //参数解析器
            if (this.argumentResolvers != null) {
                invocableMethod.setHandlerMethodArgumentResolvers(this.argumentResolvers);
            }
// 返回值处理器。
            if (this.returnValueHandlers != null) {
                invocableMethod.setHandlerMethodReturnValueHandlers(this.returnValueHandlers);
            }
	//重点： invokeAndHandle ，真正执行目标方法。 
            invocableMethod.invokeAndHandle(webRequest, mavContainer, new Object[0]);
    
        return var15;
    }
```

参数解析器 argumentResolvers 最终的接口：有两个方法 1. 是否支持当前参数 2. 如果支持，调用第二个方法解析参数。

![image-20211204194809486](D:\Typora\自服务\SpringBoot.assets\image-20211204194809486.png)



#### 1.2&1.3invokeAndHandle 

```java 
//ServletInvocableHandlerMethod.class
public void invokeAndHandle(ServletWebRequest webRequest, ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {
    //真正调用目标控制器方法，这一过程做了两件事情：
    //1. 从请求分析参数值并转换成可用于目标控制器方法的结构和类型
    //2.调用开发人员提供的目标控制器方法。 
        Object returnValue = this.invokeForRequest(webRequest, mavContainer, providedArgs);
//返回的对象是Object，是因为框架不知道开发人员会设计什么返回类型
    
    this.setResponseStatus(webRequest);
        if (returnValue == null) {
            if (this.isRequestNotModified(webRequest) || this.getResponseStatus() != null || mavContainer.isRequestHandled()) {
                this.disableContentCachingIfNecessary(webRequest);
                mavContainer.setRequestHandled(true);
                return;
            }
        } else if (StringUtils.hasText(this.getResponseStatusReason())) {
            mavContainer.setRequestHandled(true);
            return;
        }

        mavContainer.setRequestHandled(false);
        Assert.state(this.returnValueHandlers != null, "No return value handlers");

        try {
            this.returnValueHandlers.handleReturnValue(returnValue, this.getReturnValueType(returnValue), mavContainer, webRequest);
        } catch (Exception var6) {
            if (logger.isTraceEnabled()) {
                logger.trace(this.formatErrorForReturnValue(returnValue), var6);
            }

            throw var6;
        }
    }
===================================
   @Nullable
    public Object invokeForRequest(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {
        //1.3 获取方法的参数值！
        Object[] args = this.getMethodArgumentValues(request, mavContainer, providedArgs);
        if (logger.isTraceEnabled()) {
            logger.trace("Arguments: " + Arrays.toString(args));
        }
//调用反射工具来执行目标方法
        return this.doInvoke(args);
    }
```

https://blog.csdn.net/andy_zhang2007/article/details/99719097

#### 1.4getMethodArgumentValues

```java 
//InvocablehandlerMethod.class
获取目标控制器方法的参数值 
protected Object[] getMethodArgumentValues(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {
        MethodParameter[] parameters = this.getMethodParameters(); //获取方法的所有参数
        if (ObjectUtils.isEmpty(parameters)) {
            return EMPTY_ARGS;
        } else {
            Object[] args = new Object[parameters.length];
            for(int i = 0; i < parameters.length; ++i) { //逐个解析参数
                MethodParameter parameter = parameters[i];
                parameter.initParameterNameDiscovery(this.parameterNameDiscoverer);
                args[i] = findProvidedArgument(parameter, providedArgs);
                if (args[i] == null) {
                    if (!this.resolvers.supportsParameter(parameter)) {
                        throw new IllegalStateException(formatArgumentError(parameter, "No suitable resolver"));
                    }

                    try {
                        //调用了参数解析器来解析当前参数
                        args[i] = this.resolvers.resolveArgument(parameter, mavContainer, request, this.dataBinderFactory);
                    } catch (Exception var10) {
                        if (logger.isDebugEnabled()) {
                            String exMsg = var10.getMessage();
                            if (exMsg != null && !exMsg.contains(parameter.getExecutable().toGenericString())) {
                                logger.debug(formatArgumentError(parameter, exMsg));
                            }
                        }
                        throw var10;
                    }
                }
            }
            return args;
        }
    }


 
```

#### 1.5resolveArgument

```java 
----------------------------1.5 resolveArgument------------
    @Nullable
    public Object resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer, NativeWebRequest webRequest, @Nullable WebDataBinderFactory binderFactory) throws Exception {
    //获取参数的参数解析器
        HandlerMethodArgumentResolver resolver = this.getArgumentResolver(parameter);
        if (resolver == null) {
            throw new IllegalArgumentException("Unsupported parameter type [" + parameter.getParameterType().getName() + "]. supportsParameter should be called first.");
        } else {
            return resolver.resolveArgument(parameter, mavContainer, webRequest, binderFactory);
        }
    }

```



#### 1.6getArgumentResolver

参数解析器的解析过程：

```Java 
//HandlerMethodArgumentResolverComposite.java
@Nullable
    private HandlerMethodArgumentResolver getArgumentResolver(MethodParameter parameter) {
        //springmvc的缓存机制。
        HandlerMethodArgumentResolver result = (HandlerMethodArgumentResolver)this.argumentResolverCache.get(parameter);
        if (result == null) {
            Iterator var3 = this.argumentResolvers.iterator();
            while(var3.hasNext()) {
                            //迭代每一个参数解析器，看有没有能解析的。
                HandlerMethodArgumentResolver resolver = (HandlerMethodArgumentResolver)var3.next();
                //判断当前参数解析器是否支持解析
                if (resolver.supportsParameter(parameter)) {
                    
                    result = resolver;
                    this.argumentResolverCache.put(parameter, resolver);
                    break;
                }
            }
        }
        return result;
    }
}
```

#### 2.原生ServletAPI

原生Servlet使用的参数解析器是：ServletRequestMethodArgumentResolver

#### 3.Model,Map

Model和Map里面的数据会被放在request请求域中，相当于调用了request.setAttribute()方法。 

测试：

```java
@GetMapping("/params")
public String testParam(Map<String, Object> map,
                        Model model,
                        HttpServletRequest request,
                        HttpServletResponse response) {
    map.put("hello","world666");
    model.addAttribute("world","hello666");
    request.setAttribute("message", "hellowrold");

    Cookie cookie = new Cookie("cookie","yummy");
    cookie.setDomain("localhost");
    response.addCookie(cookie);
    return "forward:/success";
}

@ResponseBody
@GetMapping("/success")
public Map<String,Object> success(@RequestAttribute(value = "msg" ,required = false) String msg,
                   @RequestAttribute(value = "code", required = false) Integer code,
                   HttpServletRequest request) {
    Map<String,Object> map=new HashMap<String,Object>();
    Object msg1 = request.getAttribute("msg");
    Object m1  =  request.getAttribute("hello");
    Object m2  =  request.getAttribute("message");
    Cookie[]  m3 =  request.getCookies() ;
    for(Cookie c:m3){
        System.out.println(c.getName() + " " + c.getValue());
    }
    map.put("msg1",msg1);
    map.put("msg2",m1);
    map.put("msg3",m2);
    map.put("code",code) ;
    return map;
}
```

##### 原理

调试上面代码：

①、解析第一个map

1. 在1.6中寻找能够支持他们的参数解析器
2. 在1.5 中：

```java 
    @Nullable
    public Object resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer, NativeWebRequest webRequest, @Nullable WebDataBinderFactory binderFactory) throws Exception {
        Assert.state(mavContainer != null, "ModelAndViewContainer is required for model exposure");
        //mavContainer就是 ModelAndViewContainer的缩写。
        return mavContainer.getModel();
    }

    private final ModelMap defaultModel = new BindingAwareModelMap();

   public ModelMap getModel() {
        if (this.useDefaultModel()) {
            //返回了这个：BindingAwareModelMap
            return this.defaultModel;
        } else {
            if (this.redirectModel == null) {
                this.redirectModel = new ModelMap();
            }

            return this.redirectModel;
        }
    }
```



<img src="D:\Typora\自服务\SpringBoot.assets\image-20211204215155565.png" alt="image-20211204215155565" style="zoom:50%;" />

BindingAwareModelMap既是Map，又是Model

②、解析第二个参数Model，代码路径和①一样。

发现：

<img src="D:\Typora\自服务\SpringBoot.assets\image-20211204215508998.png" alt="image-20211204215508998" style="zoom:50%;" />

- 无论是使用map还是model，底层都是返回mavContainer.getModel() --> BindingAwareModelMap 是Model也是Map （是同一个对象） 

③、解析完所有参数，回到1.2 ，然后再回到doDispath方法，进行视图渲染，并把Map和Model的数据都放到一个叫MergedOutputModel中，最后再把MergedOutputModel的数据都放在request请求域中





```Java
//InternalResourceView.java 
protected void renderMergedOutputModel(Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
    //重点方法： 将model中的所有数据都放在了request请求域中，是在springmvc渲染模型之前放的。 
        this.exposeModelAsRequestAttributes(model, request);
        this.exposeHelpers(request);
        String dispatcherPath = this.prepareForRendering(request, response);
        RequestDispatcher rd = this.getRequestDispatcher(request, dispatcherPath);
        if (rd == null) {
            throw new ServletException("Could not get RequestDispatcher for [" + this.getUrl() + "]: Check that the corresponding file exists within your web application archive!");
        } else {
            if (this.useInclude(request, response)) {
                response.setContentType(this.getContentType());
                if (this.logger.isDebugEnabled()) {
                    this.logger.debug("Including [" + this.getUrl() + "]");
                }

                rd.include(request, response);
            } else {
                if (this.logger.isDebugEnabled()) {
                    this.logger.debug("Forwarding to [" + this.getUrl() + "]");
                }

                rd.forward(request, response);
            }

        }
    }

  protected void exposeModelAsRequestAttributes(Map<String, Object> model, HttpServletRequest request) throws Exception {
        model.forEach((name, value) -> {
            if (value != null) {
                request.setAttribute(name, value);
            } else {
                request.removeAttribute(name);
            }

        });
    }


```





#### 4.POJO封装

将表单提交的数据封装成一个pojo 

**示例**

```java
    @PostMapping("/saveUser")
    public String saveUser(Person p) {
        return p.toString();
    }
```

```html
<form action="/saveUser" method="post">
    姓名      <input name="userName" value="zhangsan"/> <br/>
    年龄      <input name="age" value="12"/> <br/>
    出生日期  <input name="birth" value="2001/1/2"/> <br/>
    宠物-名字 <input name="pet.name" value="loki"/> <br/>
    宠物-年龄 <input name="pet.age" value="1"/><br/>
    <input type="submit" value="保存"/>
</form>
```



自定义类型的参数（即自定义类），由`ServletModelAttributeMethodProcessor`这个参数解析器来解析

- 确定参数解析中源码的判断：是否为简单类型（整型/字符串等）-->不是——>true

- ```java 
      public boolean supportsParameter(MethodParameter parameter) {
          return parameter.hasParameterAnnotation(ModelAttribute.class) || this.annotationNotRequired && !BeanUtils.isSimpleProperty(parameter.getParameterType()); 
      }
  ```

确定了参数解析器后，将会使用参数解析器来解析参数：

```Java
 @Nullable
    public final Object resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer, NativeWebRequest webRequest, @Nullable WebDataBinderFactory binderFactory) throws Exception {
        Assert.state(mavContainer != null, "ModelAttributeMethodProcessor requires ModelAndViewContainer");
        Assert.state(binderFactory != null, "ModelAttributeMethodProcessor requires WebDataBinderFactory");
        String name = ModelFactory.getNameForParameter(parameter);
        ModelAttribute ann = (ModelAttribute)parameter.getParameterAnnotation(ModelAttribute.class);
        if (ann != null) {
            mavContainer.setBinding(name, ann.binding());
        }

        Object attribute = null;
        BindingResult bindingResult = null;
        if (mavContainer.containsAttribute(name)) {
            attribute = mavContainer.getModel().get(name);
        } else {
            try {
                //这一步里面，就创建了自定义pojo的实例。 
                attribute = this.createAttribute(name, parameter, binderFactory, webRequest);
            } catch (BindException var10) {
                if (this.isBindExceptionRequired(parameter)) {
                    throw var10;
                }

                if (parameter.getParameterType() == Optional.class) {
                    attribute = Optional.empty();
                } else {
                    attribute = var10.getTarget();
                }

                bindingResult = var10.getBindingResult();
            }
        }

        if (bindingResult == null) {
            //创建一个webDataBinder ，web数据绑定器，作用：将请求参数的值绑定到JavaBean中。
            WebDataBinder binder = binderFactory.createBinder(webRequest, attribute, name);
            if (binder.getTarget() != null) {
                if (!mavContainer.isBindingDisabled(name)) {
                    //【关键！】将webRequest中的数据和binder绑定起来。
                    this.bindRequestParameters(binder, webRequest);
                }

                this.validateIfApplicable(binder, parameter);
                if (binder.getBindingResult().hasErrors() && this.isBindExceptionRequired(binder, parameter)) {
                    throw new BindException(binder.getBindingResult());
                }
            }

            if (!parameter.getParameterType().isInstance(attribute)) {
                attribute = binder.convertIfNecessary(binder.getTarget(), parameter.getParameterType(), parameter);
            }

            bindingResult = binder.getBindingResult();
        }

        Map<String, Object> bindingResultModel = bindingResult.getModel();
        mavContainer.removeAttributes(bindingResultModel);
        mavContainer.addAllAttributes(bindingResultModel);
        return attribute;
    }

```

SpringMVC使用WebDataBinder来获取请求的参数，它的主要功能是解析http请求上下文，在控制器调用之前转换参数并提供验证功能，为控制器方法做准备。 WebDataBinder会使用三种接口来进行数据转换，即Converter，Formatter和GenericConvertor。

![image-20211205195926535](D:\Typora\自服务\SpringBoot.assets\image-20211205195926535.png)

GenericConversionService:在设置每一个值的时候，找它里面所有的converter那个可以将这个数据类型（request带来参数的字符串）转换到指定类型

##### 自定义Converter

提交页面：

```html 
    宠物 <input name="pet" value="loki,3"></br>
```

通过WebMvcConfigurer中的addFormatters方法给容器添加一个converter，将string转换成pet类型。

```java 
  @Bean
     public WebMvcConfigurer webMvcConfigurer(){
         return new WebMvcConfigurer() {
             @Override
             public void addFormatters(FormatterRegistry registry) {
                 registry.addConverter(new Converter<String, Pet>() {

                     @Override
                     public Pet convert(String source) {
                         Pet pet =new Pet() ;
                         String[] str =source.split(",");
                         pet.setAge(Integer.parseInt(str[1]));
                         pet.setName(str[0]);
                         return pet;
                     }
                 });
                 WebMvcConfigurer.super.addFormatters(registry);
             }
         };
     }
```

### 数据响应源码

数据响应：JSON，XML，xls，图片，视频，自定义协议数据

响应JSON 

ReturnValueHandler原理

和参数解析器的逻辑一样，先判断那个支持解析，再用匹对的返回值处理器处理返回值。

![image-20211205210616147](D:\Typora\自服务\SpringBoot.assets\image-20211205210616147.png)

**1.1**

```java 
//在1.2的方法里， 
try {
   //使用返回值处理器 处理返回值
            this.returnValueHandlers.handleReturnValue(returnValue, this.getReturnValueType(returnValue), mavContainer, webRequest);
        }
```

**1.2**

```Java
    public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType, ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws Exception {
        //先找到能处理当前返回值的handler 
        HandlerMethodReturnValueHandler handler = this.selectHandler(returnValue, returnType);
        if (handler == null) {
            throw new IllegalArgumentException("Unknown return value type: " + returnType.getParameterType().getName());
        } else {
            handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);
        }
    }

```

**1.3** selectHandler 方法中：可以看到处理json数据的，使用的是**RequestResponseBodyMethodProcessor**这个处理器。 

![image-20211205211534096](D:\Typora\自服务\SpringBoot.assets\image-20211205211534096.png)

**1.4**    找到了之后，执行handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);

```java 
    public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType, ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws IOException, HttpMediaTypeNotAcceptableException, HttpMessageNotWritableException {
        mavContainer.setRequestHandled(true);
        ServletServerHttpRequest inputMessage = this.createInputMessage(webRequest);
        ServletServerHttpResponse outputMessage = this.createOutputMessage(webRequest);
        //使用messageConverter消息转换器进行写出
        this.writeWithMessageConverters(returnValue, returnType, inputMessage, outputMessage);
    }
```

**1.5messageConverter做了什么？**

```java 
    protected <T> void writeWithMessageConverters(@Nullable T value, MethodParameter returnType, ServletServerHttpRequest inputMessage, ServletServerHttpResponse outputMessage) throws IOException, HttpMediaTypeNotAcceptableException, HttpMessageNotWritableException {
        Object body;
        Class valueType;
        Object targetType;
        //先将返回值赋值给body
        if (value instanceof CharSequence) {
            body = value.toString();
            valueType = String.class;
            targetType = String.class;
        } else {
            body = value;
            valueType = this.getReturnValueType(value, returnType);
            targetType = GenericTypeResolver.resolveType(this.getGenericType(returnType), returnType.getContainingClass());
        }

     
//【1.获取当前响应头中 outputMessage.getHeaders().已经有媒体类型，如果有，就用响应头的媒体类型】
        MediaType selectedMediaType = null;
        MediaType contentType = outputMessage.getHeaders().getContentType();
        boolean isContentTypePreset = contentType != null && contentType.isConcrete();
        if (isContentTypePreset) {
            if (this.logger.isDebugEnabled()) {
                this.logger.debug("Found 'Content-Type:" + contentType + "' in response");
            }

            selectedMediaType = contentType;
        } 
        
        else {
            HttpServletRequest request = inputMessage.getServletRequest();

            List acceptableTypes;
            try {
                // 【2.如果没有】：获取浏览器能支持的MedaiType，能接受的媒体类型（内容）
                acceptableTypes = this.getAcceptableMediaTypes(request);
            }
            // 3. 获取服务器能产生的所有MediaType
            List<MediaType> producibleTypes = getProducibleMediaTypes(request, valueType, targetType);
          // 4. 最佳匹配，外循环是浏览器，内循环是服务器的， 把所有匹配的都加入一个mediaTypesToUse的list中。
            List<MediaType> mediaTypesToUse = new ArrayList<>();
            for (MediaType requestedType : acceptableTypes) {
                for (MediaType producibleType : producibleTypes) {
                    if (requestedType.isCompatibleWith(producibleType)) {
                        mediaTypesToUse.add(getMostSpecificMediaType(requestedType, producibleType));
                    }
                }
            }
         
  // 5. 
        if (selectedMediaType != null) {
            selectedMediaType = selectedMediaType.removeQualityValue();
            for (HttpMessageConverter<?> converter : this.messageConverters) {
                GenericHttpMessageConverter genericConverter = (converter instanceof GenericHttpMessageConverter ?
                                                                (GenericHttpMessageConverter<?>) converter : null);
                if (genericConverter != null ?
                    ((GenericHttpMessageConverter) converter).canWrite(targetType, valueType, selectedMediaType) :
                    converter.canWrite(valueType, selectedMediaType)) {
                    body = getAdvice().beforeBodyWrite(body, returnType, selectedMediaType,
                                                       (Class<? extends HttpMessageConverter<?>>) converter.getClass(),
                                                       inputMessage, outputMessage);
                    if (body != null) {
                        Object theBody = body;
                        LogFormatUtils.traceDebug(logger, traceOn ->
                                                  "Writing [" + LogFormatUtils.formatValue(theBody, !traceOn) + "]");
                        addContentDispositionHeader(inputMessage, outputMessage);
                        if (genericConverter != null) {
                            genericConverter.write(body, targetType, selectedMediaType, outputMessage);
                        }
                        else {
                            ((HttpMessageConverter) converter).write(body, selectedMediaType, outputMessage);
                        }
                    }
                    else {
                        if (logger.isDebugEnabled()) {
                            logger.debug("Nothing to write: null body");
                        }
                    }
                    return;
                }
            }
        }


```



内容协商：

浏览器默认会在请求头中就告诉服务器，它能接受什么类型的内容

服务器根据自己的能力生成什么样的内容。

![image-20211205215938692](D:\Typora\自服务\SpringBoot.assets\image-20211205215938692.png)

​     acceptableTypes = this.getAcceptableMediaTypes(request);上面函数获得的结果，就是浏览器能接收的内容

<img src="D:\Typora\自服务\SpringBoot.assets\image-20211205220919199.png" alt="image-20211205220919199" style="zoom:50%;" />

<img src="D:\Typora\自服务\SpringBoot.assets\image-20211205220947903.png" alt="image-20211205220947903" style="zoom:50%;" />





#### httpMessageConverter 

**①、**作用： httpMessageConverter 中的函数判断此Class对象能否转换成MediaType，如将person对象转换成json数据。

<img src="D:\Typora\自服务\SpringBoot.assets\image-20211205221539170.png" alt="image-20211205221539170" style="zoom:50%;" />



**②、protected final List<HttpMessageConverter<?>> messageConverters;** 所有的消息转换器。 如果处理的是Json数据，那么7号MappingJackson2HttpMessageConverter可以处理

<img src="D:\Typora\自服务\SpringBoot.assets\image-20211205221953102.png" alt="image-20211205221953102" style="zoom:50%;" />

### thymeleaf

1. spring boot 项目默认不允许访问template下的文件（受保护的），所以想要访问template下的html页面， 可以配置视图解析器

2. 如果想要用视图去展示，应该设置好视图展示页面，比如用一个模板语言来接受返回的数据，比如thymeleaf和freemarker，也可以用jsp



https://www.cnblogs.com/hjwublog/p/5051732.html#_label0

springboot不支持jsp

thymeleaf使用：

1、springboot 引入starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

**2、springboot源码分析：**

```java 
@Configuration(proxyBeanMethods = false)
//ThymeleafProperties配置文件，包含了thymeleaf的配置项
@EnableConfigurationProperties(ThymeleafProperties.class)
@ConditionalOnClass({ TemplateMode.class, SpringTemplateEngine.class })
@AutoConfigureAfter({ WebMvcAutoConfiguration.class, WebFluxAutoConfiguration.class })
public class ThymeleafAutoConfiguration {
    自动配好了SpringThymeleaf的模板引擎SpringTemplateEngine，视图解析器defaultTemplateResolver
}
```

```java 
//默认模板映射路径是：src/main/resoucres/templates
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {
//页面要放在这个位置。 
	public static final String DEFAULT_PREFIX = "classpath:/templates/";
	public static final String DEFAULT_SUFFIX = ".html";
```

**3、页面要放在 "classpath:/templates/" 路径下， 其他js，css资源放在static目录下**

4、在html中添加：

<html lang="en" xmlns:th="http://www.thymeleaf.org">  



#### 用法

①、获取变量值${...}

用于获取容器上下文环境中的变量。 

②、链接表达式：@{...}

用来配合link，src，href使用的语法，类似的标签有：th:href,th:src

```html 
<script th:src="@{/js/jquery-1.10.2.min.js}"></script>
```

③、th:each遍历

后台：

```Java
 @GetMapping("dynamic_table")
    public String dynamic_table(Model model) {
        List<User> user = Arrays.asList(new User("wingchi","123456"),
                new User("hangken","000000"),
                new User("leifengyang","111111"),
                new User("kimNahum","22222")) ;
        model.addAttribute("users",user);
        return "table/dynamic_table" ;
    }
```



```html 
<tbody>
    使用thymeleaf语法遍历放在session中的值user ，有两种方法：${...}和[[$...]]，其中status代表遍历的状态。 
            <tr class="gradeX" th:each="user,status:${users}">
                <td th:text="${status.count}">ID</td>
                <td th:text="${user.username}">USe</td>
                <td >[[${user.password}]]</td>
            </tr>
        </tbody>
```



### 拦截器 

Intercepter和Filter功能类似，都是面向切面编程的一种实现。

拦截器可以在请求到达Controller之前拦截请求并做一些处理。

使用场景：

- 日志记录：记录请求信息的日志，进行信息监控，信息统计，计算PV（Page View）等
- 权限检查：如登录检测
- 性能监控：通过拦截器在进入处理器之前记录开始时间，处理完后记录结束时间，从而得到请求处理的时间。
- 通用：读取Cookie得到用户信息并将用户对象放入请求，方便后续流程使用。

#### 自定义 Interceptor

如果你需要自定义 **Interceptor** 的话必须实现 `org.springframework.web.servlet.HandlerInterceptor`接口或继承 `org.springframework.web.servlet.handler.HandlerInterceptorAdapter`类，并且需要重写下面下面 3 个方法： 

1. `preHandler(HttpServletRequest request, HttpServletResponse response, Object handler)` 方法在请求处理之前被调用。该方法在 Interceptor 类中最先执行，用来进行一些前置初始化操作或是对当前请求做预处理，也可以进行一些判断来决定请求是否要继续进行下去。

   当它返回 false 时，表示请求结束，后续的 Interceptor 和 Controller 都不会再执行；当它返回为 true 时会继续调用下一个 Interceptor 的 preHandle 方法，如果已经是最后一个 Interceptor 的时候就会调用当前请求的 Controller 方法。

2. `postHandler(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)` 方法在当前请求处理完成之后，也就是 Controller 方法调用之后执行，但是它会在  DispatcherServlet  进行视图返回渲染之前被调用，所以我们可以在这个方法中对 Controller 处理之后的 ModelAndView 对象进行操作。

3. `afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handle, Exception ex)` 方法需要在当前对应的 Interceptor 类的 preHandle 方法返回值为 true 时才会执行。顾名思义，该方法将在整个请求结束之后，也就是在 DispatcherServlet  渲染了对应的视图之后执行。此方法主要用来进行资源清理。



#### HandlerInterceptor

定义了3个方法 ：分别是处理之前，处理之后，页面渲染之后

![image-20211216102942169](D:\Typora\自服务\SpringBoot.assets\image-20211216102942169.png)

使用方法：

1. 编写一个拦截器，实现HandlerInterceptor的接口
2. 将拦截器注册到容器中，实现WebMvcConfigurer 
3. 配置那些路径放行，那些路径拦截。 

需求：后台管理系统访问其他页面时，需要检查是否先登录，原先是在单个控制器中检查是否有user对象被提交，麻烦，使用拦截器：



```java 
/**
 * 1.配置好拦截器要配置哪些请求
 * 2.把这些配置放在容器中。
 */
public class LoginInterceptor implements HandlerInterceptor {
    在请求处理前拦截,如果返回值为false,表示拦截请求,不再向下执行,如果返回true,放行. 
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //获取session，看里面有没有user对象
        HttpSession httpSession = request.getSession();
        Object User = httpSession.getAttribute("user") ;
        //如果为空，没有，添加提示信息并且转发到登录页面，如果用request，无法获取提示消息（不懂）
        if(User==null) {
            request.setAttribute("msg","先登录");
            request.getRequestDispatcher("/").forward(request,response); 
            // httpSession.setAttribute("msg","先登录");
            //response.sendRedirect("/");
            return false  ;
        }
        return true ;
    }
}

```

```java
//将拦截器放到容器中。
@Configuration
public class WebAdminConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor()).
//                添加要拦截的请求，这里是所有请求，包括静态资源
                addPathPatterns("/**").
//                添加放行的请求
              excludePathPatterns("/","/login","/","/css/**","/js/**","/fonts/**","/images/**","js/**") ;
    }
}
```



#### 调试分析

以main.html请求为例:

```java
@GetMapping("/main.html")
public String mainPage(HttpSession httpSession,Model model )
{    return "main" ;}
```

①. 在doDispatch#getHandler中 ,找到了可以处理的`handler`和以及`handler`的所有拦截器`interceptorList `![image-20211216111153766](D:\Typora\自服务\SpringBoot.assets\image-20211216111153766.png)

②. 在handle方法执行前,先调用applyPreHandle方法

```java 
if (!mappedHandler.applyPreHandle(processedRequest, response)) {
    return;
}
// Actually invoke the handler.
mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
--------------------------------------------------------------------------------------------------
  // ③.
boolean applyPreHandle(HttpServletRequest request, HttpServletResponse response) throws Exception {
		for (int i = 0; i < this.interceptorList.size(); i++) {
			HandlerInterceptor interceptor = this.interceptorList.get(i);
               //每个拦截器调用自己的prohandler方法
			if (!interceptor.preHandle(request, response, this.handler)) {
				//如果拦截器返回的值为false,执行triggerAfterCompletion	
                		triggerAfterCompletion(request, response, null);
				return false;
			}
			this.interceptorIndex = i;
		}
		return true;
	}
------------------------------------------------------------------
    //🎈④. 
    void triggerAfterCompletion(HttpServletRequest request, HttpServletResponse response, @Nullable Exception ex) {
		for (int i = this.interceptorIndex; i >= 0; i--) 
            //这里🎈倒序执行🎈已经执行了的拦截器的afterCompletion方法, 
			HandlerInterceptor interceptor = this.interceptorList.get(i);
			try {
				interceptor.afterCompletion(request, response, this.handler, ex);
			}
			catch (Throwable ex2) {
				logger.error("HandlerInterceptor.afterCompletion threw exception", ex2);
			}
		}
	}
```

单个拦截器的执行流程:有顺序的执行! 

 ![img](https://segmentfault.com/img/remote/1460000024464171) 

🎅多个拦截器的执行流程谈好🎅

每个拦截器都按照提前配置好的顺序执行,它们内部的执行规律并不像多个普通java类一样,它们的设计模式是基于"责任链"的模式.

 ![img](https://segmentfault.com/img/remote/1460000024464173) 

https://segmentfault.com/a/1190000024464165



### 文件上传





🥑原理：

SpringBoot的文件自动配置类：MultipartAutoConfiguration-MultipartProperties 

代码调试：

1、 使用文件上传解析器isMultipart判断并封装成（返回）MultipartHttpServletRequest 

```java
  
protected HttpServletRequest checkMultipart(HttpServletRequest request) throws MultipartException {
    if (this.multipartResolver != null && this.multipartResolver.isMultipart(request)) {
        try {
            return this.multipartResolver.resolveMultipart(request);
        }
    }
------------------------------🥑isMultipart方法🥑---------------------

public boolean isMultipart(HttpServletRequest request) {
        return StringUtils.startsWithIgnoreCase(request.getContentType(), this.strictServletCompliance ? "multipart/form-data" : "multipart/");
    }
    ----------------------------------🥑resolveMultipart方法🥑------------------------
 public MultipartHttpServletRequest resolveMultipart(HttpServletRequest request) throws MultipartException {
        return new StandardMultipartHttpServletRequest(request, this.resolveLazily);
    }
```

2、 参数解析器解析请求中的文件内容，封装成MultipartFile

3、 将request中文件信息封装成一个Map,MultiValueMap<String,MultipartFile>

