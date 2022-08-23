







cd /d D:\DevTool\mysql\bin & net start mysql   & mysql -u root -pss123000 

# mybatis

Hibernate-数据库交互的框架（ORM），Object Relation Mapping , 只需要关心JavaBean。

缺点： 定制sql，没有办法写SQL语句。

全映射框架，查出来的是所有字段，部分字段映射很难。  

MyBatis-也是数据库交互框架（SQL框架）



 ![img](https://img2018.cnblogs.com/blog/1456626/201903/1456626-20190327200843495-1926482501.png) 



SqlSessionFactory是Mybatis的关键对象，它是数据库映射关系经过编译后的内存镜像，每一个MyBatis应用程序都以一个SqlSessionFactory对象的实例为核心。

SqlSession类似于jdbc中的Connection，是应用程序和持久层交互操作的单线程对象，SqlSession实例直接执行被映射的sql语句，每一个线程都应该有自己的SqlSession实例。 



### 整合

①、导入stater

```xml-dtd
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```

②、添加数据源配置：

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/students?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=utf8&useSSL=false
spring.datasource.username=root
spring.datasource.password=ss123000
```

springboot会自动加载spring.datasource*相关配置，数据源自动注入到sqlSessionFactory中



③、**在springboot的yaml中**配置mybatis的全局配置文件，sql映射文件的路径：

```yaml 
#配置mybatis
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
  configuration:
    map-underscore-to-camel-case: true
```

④、编写mapper文件，接口

```xml 
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wingchi.springboot3.service.StudentMapper">
    <select id="getStudentById" resultType="com.wingchi.springboot3.bean.Student">
        select * from t_student where id=#{id}
     </select>
</mapper>
```

```java 
@Mapper
public interface StudentMapper {
    public Student getStudentById(int id) ;
}
```

**接口上加mapper注解，表示该接口类对象叫给mybatis底层创建，然后再给spring框架管理**

或者直接在启动类上增加自动扫描包：

```java 
@MapperScan("com.neo.mapper")
```

两者缺一不可

- @MapperScan注解作用是扫描 *Mapper接口类，并生成对应的实现类*

- mybatis.mapper-locations 在springboot配置文件中使用，作用是**扫描mapper接口对应的xml文件** 

**⑤、注意事项**

- 在springboot的配置文件yaml中可以配置mybatis的选项

- 在mybatis的全局配置文件中也可以配置mybatis的选项

**而mybais的全局配置项只能全部配置在一个文件内！否则会引起出错！**

推荐只保留一个配置文件，因为一个文件可以实现全部配置，建议只留yaml文件

## myBatis配置文件

MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：

  

写配置文件的时候，一定要按照这个顺序写。 



![image-20211101143144393](D:\Typora\自服务\mybatis.assets\image-20211101143144393.png)

```xml 
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- settings -->
    <settings>
        <!-- 打开延迟加载的开关 -->
        <setting name="lazyLoadingEnabled" value="true"/>
        <!-- 将积极加载改为消极加载（即按需加载） -->
        <setting name="aggressiveLazyLoading" value="false"/>
        <!-- 打开全局缓存开关（二级缓存）默认值就是 true -->
        <setting name="cacheEnabled" value="true"/>
<!--        开启驼峰命名，在数据库中不区分大小写的字段，如addr_zn 查询回来对应的javabean自动转换为 addrZn -->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
<!--    加载映射文件-->
    <typeAliases>   
        <package name="com.wingchi.beans"/>
    </typeAliases>
    <mappers>
        <mapper resource="mybatis/mapper/Studentmapper.xml"/>
    </mappers>
</configuration>
```



- typeAliases标签可以为Java类型设置一个缩写名字,如果不指定alias，默认为不区分大小写的类名

```xml 
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
</typeAliases>
Blog可以用在任何使用domain.blog.Blog的地方。
```

也可以指定一个包名，MyBatis会在包名下面搜索需要的java Bean。

"com.wingchi.beans" 包里的java Bean，在没有注解的情况下， 会使用Bean的首字母小写的非限定类名作为它的别名，比如 com.wingchi.beans.Author 的别名就是 author， 如果有@Alias注解，则别名就是@Alias注解的值。 



### mappers 

告诉MyBatis到哪里去找到sql语句。可以使用类路径的资源引用，或全限定资源定位符（file：///) 或类名，包名等，例如：

```xml
<mappers>
    <!-- 使用相对于类路径的资源引用 -->
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
    <!-- 使用完全限定资源定位符（URL） -->
<mapper url="file:///var/mappers/AuthorMapper.xml"/>
    <!-- 使用映射器接口实现类的完全限定类名  把sql映射的xml文件放到接口同一个包下，并且两个文件要同名 -->
      <mapper class="org.mybatis.builder.AuthorMapper"/>
    <!-- 将包内的映射器接口实现全部注册为映射器 -->
      <package name="org.mybatis.builder"/>

</mappers>
```



### environments

mybatis可以配置成始应多种环境，例如开发，测试和生产环境的不同配置。 这种机制有利于将sql映射应用于多种数据库之中。

```xml 
配置一个具体的环境，都需要一个事务管理器和一个数据源。
<environments default="development">
  <environment id="development">
    <transactionManager type="JDBC">
      <property name="..." value="..."/>
    </transactionManager>
    <dataSource type="POOLED">
      <property name="driver" value="${driver}"/>
      <property name="url" value="${url}"/>
      <property name="username" value="${username}"/>
      <property name="password" value="${password}"/>
    </dataSource>
  </environment>
</environments>
```



### # 和 $ 取值 

#{属性名}：参数预编译的方式，参数的位置是用？代替，后来用预编译设置进去，安全，不会有sql注入问题

$：不是参数预编译，而是直接和sql语句进行拼串，不安全。 

`id=1 or 1=1 ` sql注入



$的用法：

`select  *from ${table_name }where id = #{id} `

sql语句只支持参数的预编译，当表名或者其他不是参数的值动态变化的时候，可以是用$符号。

## 基础

### 查select

```xml 
<select
  id="selectPerson"
  parameterType="int"
  parameterMap="deprecated"
  resultType="hashmap"
  resultMap="personResultMap"
  flushCache="false"
  useCache="true"
  timeout="10"
  fetchSize="256"
  statementType="PREPARED"
  resultSetType="FORWARD_ONLY">
```

| 属性            | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| `id`            | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType` | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。 |
| parameterMap    | 用于引用外部 parameterMap 的属性，**目前已被废弃**。请使用行内参数映射和 parameterType 属性。 |
| `resultType`    | 期望从这条语句中返回结果的类全限定名或别名。 注意，**如果返回的是集合，那应该设置为集合包含的类型，而不是集合本身的类型。** resultType 和 resultMap 之间只能同时使用一个。 |
| `resultMap`     | 对外部 resultMap 的命名引用。结果映射是 MyBatis 最强大的特性，如果你对其理解透彻，许多复杂的映射问题都能迎刃而解。 resultType 和 resultMap 之间只能同时使用一个。 |
| `flushCache`    | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：false。 |
| `useCache`      | 将其设置为 true 后，将会导致本条语句的结果被二级缓存缓存起来，默认值：对 select 元素为 true。 |
| `timeout`       | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。 |
| `fetchSize`     | 这是一个给驱动的建议值，尝试让驱动程序每次批量返回的结果行数等于这个设置值。 默认值为未设置（unset）（依赖驱动）。 |
| `statementType` | 可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `resultSetType` | FORWARD_ONLY，SCROLL_SENSITIVE, SCROLL_INSENSITIVE 或 DEFAULT（等价于 unset） 中的一个，默认值为 unset （依赖数据库驱动）。 |
| `databaseId`    | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。 |
| `resultOrdered` | 这个设置仅针对嵌套结果 select 语句：如果为 true，将会假设包含了嵌套结果集或是分组，当返回一个主结果行时，就不会产生对前面结果集的引用。 这就使得在获取嵌套结果集的时候不至于内存不够用。默认值：`false`。 |
| `resultSets`    | 这个设置仅适用于多结果集的情况。它将列出语句执行后返回的结果集并赋予每个结果集一个名称，多个名称之间以逗号分隔。 |

### 传入参数

#### 单个参数

```xml 
<select id="selectPerson" parameterType="int" resultType="hashmap">
    #{} 里面的值可以随便写，反正只有一个 
  SELECT * FROM PERSON WHERE ID = #{id}
</select>
```

#### 传入多个参数

```java 
Student student = studentDao.queryStuByIdAndName(1013,"陈新") ;
```

**第一种：**

可用： arg0,arg1, param1,param2 ....

原理：只要传入了多个参数，mybatis自动封装成一个map 中，封装时使用的key就是参数的索引或者第几个的表示

("param1", 传入的值) 

("param2", 传入的值) 

```xml 
    <select id="queryStuByIdAndName" resultType="student">
        SELECT * from t_student where id = #{param1} and name = #{param2}
    </select>
```

**第二种：使用param注解**

接口方法使用注释指定key的名称

```java 
  Student queryStuByIdAndName( @Param("id") Integer id, @Param("name") String name) throws  Exception ;
```

```xml
  <select id="queryStuByIdAndName" resultType="student">
        SELECT * from t_student where id = #{id} and name = #{name}
    </select>
```

#### 传入pojo





#### 传入map 

取值： #{key} 

```xml 
<select id="queryStuByIdAndName2" resultType="student">
        SELECT * from t_student where id = #{id} and name = #{name}
    </select>
```

```java 
    Student queryStuByIdAndName2(HashMap<String,Object>map) ;
//测试
HashMap<String,Object> map =new HashMap<String,Object>() ;
map.put("id",1002) ;
map.put("name","王薇") ;
student = studentDao.queryStuByIdAndName2(map) ;
System.out.println("查到了》》》"+student);
```



#### 多行查询封装成list

```java 
List<Student> queryStuByName(String name) ;
```

```xml 
   <select id="queryStuByName" resultType="student" parameterType="string">
        select *from t_student where name like concat('%',#{name},'%')
    </select>
```

返回的是一个集合 ，**resultType 设置为集合包含的类型，而不是集合本身类型。** 

#### 单行查询封装成map

```java 
 Map<String ,Object> queryStuRetMap(Integer id) ;
Map<String ,Object> map  = studentDao.queryStuRetMap(1001) ;
System.out.println(map);
```

```xml 
  <select id="queryStuRetMap" resultType="map">
        select *from t_student where id = #{id}
    </select>
```

#### 多条记录封装map

| 注解    | 使用对象 | 描述                                                         |
| ------- | -------- | ------------------------------------------------------------ |
| @MapKey | 方法     | 这是一个用在返回值为 Map 的方法上的注解。它能够将存放对象的 List 转化为 key 值为对象的某一属性的 Map。属性有： value，填入的是对象的属性名，作为 Map 的 key 值 |





如果有很多行，每一行查询的结果最好使用**主键**作为map的key，其他值封装成java Bean：

```java 
   // 注解MapKey告诉mybatis用哪一列的值作为key 
  @MapKey("id") 
    Map<Integer,Student> queryAllStuRetMap() ;
```

```xml 
 <!--resultType指定成student，集合包装的类型-->
<select id="queryAllStuRetMap" resultType="student"> 
        select *from t_student
    </select>
```

#### 自定义结果集 

mybatis默认的自动封装结果集：

1. 按照列名和java Bean属性名一一对应（不区分大小写）
2. 不一一对应
   1. 开启驼峰模式:数据库中：aaa_bbb,javaBean：aaaBBB
   2. 起别名
   3. 自定义封装结果集，封装的是resultMap

```java 
    List<Student > queryStuByClassId (String class_id); 
```

```xml 
<resultMap id="stuList" type="student">
   // 为student自定义封装
    //主键列用id，其他列用result
        <id property="id" column="id"/>
        <result property="name" column="name"/>
        <result property="sex" column="sex"/>
        <result property="age" column="age"/>
        <result property="tel" column="tel"/>
        <result property="classId" column="class_id"/>
    </resultMap>
    <select id="queryStuByClassId" resultMap="stuList">
        select *from t_student where class_id =#{id}
    </select>
```

使用到了resultMap列表：resultMap内的属性有：

| 属性          | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| `id`          | 标识哪一个是主键                                             |
| `type`        | 指定要封装的类的完全限定名, 或者一个类型别名（关于内置的类型别名，可以参考上面的表格）。 |
| `autoMapping` | 如果设置这个属性，MyBatis 将会为本结果映射开启或者关闭自动映射。 这个属性会覆盖全局的属性 autoMappingBehavior。默认值：未设置（unset）。 |



### 增删改

标签属性 

| 属性               | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| `id`               | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType`    | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。 |
| `parameterMap`     | 用于引用外部 parameterMap 的属性，目前已被废弃。请使用行内参数映射和 parameterType 属性。 |
| `flushCache`       | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：（对 insert、update 和 delete 语句）true。 |
| `timeout`          | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。 |
| `statementType`    | 可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `useGeneratedKeys` | （仅适用于 insert 和 update）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系型数据库管理系统的自动递增字段），默认值：false。 |
| `keyProperty`      | （仅适用于 insert 和 update）此属性的作用是将插入或更新操作时的返回值赋值给PO类的某个属性，通常会设置为主键对应的属性，如果需要设置联合主键，可以在多个值之间用逗号隔开。 |
| `keyColumn`        | （仅适用于 insert 和 update）此属性用于设置第几列是主键，当主键列不是表中第一列时，此属性必须设置，在需要主键联合时，值可以用逗号隔开。 |
| `databaseId`       | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。 |



```xml 
   <insert id="insert_stu"
        parameterType = "Student"
           # 使用主键自增  
        useGeneratedKeys="true" 
           # 自增主键对应javaBean的属性 property 
           keyProperty="id">
        insert into  t_student(name,major,tel)
        values( #{name},#{major},#{tel})
    </insert>
```

**默认情况下，insert插入后的返回值是影响的行数**

如果需要返回insert后主键的值（自动增长的），需要指定两个属性：

useGeneratedKeys=”true“ 使用自增主键，**Mybatis使用JDBC的getGeneratedKeys方法来取出数据库内部生成的主键**

 keyProperty="id"  指定主键对应POJO的属性property，Mybatis会使用getGeneratedKeys来设置它的键值 （**也就是说执行完后，pojo的”id“属性就会被mybatis设置值。**

### resultMap

resultMap结果映射集是Mybatis的重要元素，它的作用是告诉mybatis从结果集中取出的数据转换为程序员需要的对象

​										**result的属性列表：**

| 属性          | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| `id`          | 当前命名空间中的一个唯一标识，用于标识一个结果映射。         |
| `type`        | 类的完全限定名, 或者一个类型别名（关于内置的类型别名，可以参考上面的表格）。 |
| `autoMapping` | 如果设置这个属性，MyBatis 将会为本结果映射开启或者关闭自动映射。 这个属性会覆盖全局的属性 autoMappingBehavior。默认值：未设置（unset）。 |



```xml
resultMap的子标签 
<resultMap> 
            <constructor>  用来将查询结果作为参数注入到实例构造方法中
                <idArg/>  标记结果作为id
                <arg/>  标记结果作为普通参数
            </constructor>  
	<id/>   一个id结果，标记结果作为id 
	<result/> 一个普通结果，标记结果普通结果
	<association>   关联其他对象
	</association> 
	<collection>  关联其他对象的集合
	<collection/>
           <discriminator>  鉴别器，根据结果值进行判断，决定如何映射
               		<case></case>  结果值的一种情况，将对应一种映射规则 
        </discriminator>
    </resultMap>
```

#### association

>  一个复杂类型的关联；许多结果将包装成这种类型
>
>  - 嵌套结果映射 – 关联可以是 `resultMap` 元素，或是对其它结果映射的引用



假设这样一个场景：student类中中有一个属性叫stuClass也是一个类， 两个类在数据库中对应student表和class表 

在数据库要查询一个学生的所有信息，要做级联查询，然后Mybatis自动封装两个类的结果时，也需要联合封装：

①、要求： 查询某个id的学生的全部信息，包括班级：

第一种可以使用 类属性.属性 

```xml 
   <resultMap id="stuAndClass" type="Student">
        <id property="Id" column="id"></id>
        <result property="name" column="name"></result>
        <result property="sex" column="sex"></result>
        <result property="age" column="age"></result>
        <result property="tel" column="tel"></result>

        <result property="stuClass.classId" column="classid"></result>
        <result property="stuClass.classNumber" column="class_number"></result>
        <result property="stuClass.headTeacher" column="head_teacher"></result>
        <result property="stuClass.major" column="major"></result>
    </resultMap>
    <select id="getStudent" resultMap="stuAndClass">
        select stu.id,stu.name,stu.sex,stu.age,stu.tel,stu.class_id, class.major,class.head_teacher,class.class_number,class.class_id as classid
        from t_student stu ,t_class class
        where stu.id=#{id} and  stu.class_id = class.class_id;
    </select>
```



用association：

```xml 
    <resultMap id="stuAndClass" type="Student">
        <id property="Id" column="id"></id>
        <result property="name" column="name"></result>
        <result property="sex" column="sex"></result>
        <result property="age" column="age"></result>
        <result property="tel" column="tel"></result>
        <association property="stuClass" javaType="com.wingchi.beans.t_class">
            <id property="classId" column="classid"></id>
            <result property="classNumber" column="class_number"></result>
            <result property="headTeacher" column="head_teacher"></result>
            <result property="major" column="major"></result>
        </association>
    </resultMap>
```



#### 练习

#### 场景1

 查询某个id的班级的信息，把该班上的同学也全部查询出来：

```java 
public class t_class {
    private String classId ;
    private Integer classNumber ;
    private String headTeacher;
    private String major;
    private List<Student> students ;  //关键要把查出来的student集合封装起来
    }
```

Dao接口：`  public t_class getClassById(Integer id) ;`

使用collection元素：

```xml 
  <resultMap id="myClassAndStu" type="com.wingchi.beans.t_class">
        <id property="classId" column="classid"></id>
        <result property="classNumber" column="class_number"></result>
        <result property="headTeacher" column="headTeacher"></result>
        <result property="major" column="major"></result>
<!--        用于定义集合元素的。
        property：指定那个属性是collection
        ofType:指定集合里面元素的类型-->
        <collection property="students" ofType="com.wingchi.beans.Student">
            <id property="Id" column="id"></id>
            <result property="name" column="name"></result>
            <result property="sex" column="sex"></result>
            <result property="age" column="age"></result>
            <result property="tel" column="tel"></result>
        </collection>
    </resultMap>
    <sele  ct id="getClassById"  parameterType="int" resultMap="myClassAndStu">
        select stu.id,stu.name,stu.sex,stu.age,stu.tel,stu.class_id, class.major,class.head_teacher,class.class_number,class.class_id as classid
        from t_student stu ,t_class class
        where class.class_id=#{id} and  stu.class_id = class.class_id;
    </select>
```

#### 场景2

使用Mybatis分布进行简单查询：根据学号查询学生信息，查到的stuClass类的时候用另外一个sql语句查询：

即用两条简单sql语句完成联合查询： 

```xml 
  <select id="getStudentById2" resultMap="myStudentMap">
        select *from t_student where id = #{id}
    </select>
    <resultMap id="myStudentMap" type="Student">
        <id property="Id" column="id"/>
        <result property="name" column="name"></result>
        <result property="sex" column="sex"></result>
        <result property="age" column="age"></result>
        <result property="tel" column="tel"></result>
<!--        select 指定一个查询sql的id  
告诉mybatis，查询stuClass属性的时候，用另外一条sql语句查。 -->
        <association property="stuClass" select="com.wingchi.Dao.ClassDao.getClassById2" column="class_id"></association>
    </resultMap>

🍙🍙🍙🍙🍙🍙🍙🍙🍙🍙🍙另外一个映射文件🍙🍙🍙🍙🍙🍙🍙🍙🍙🍙🍙🍙🍙🍙🍙🍙
  <select id="getClassById2" resultType="com.wingchi.beans.t_class" >
        select *from t_class where class_id = #{class_id}
    </select>
```

测试：

```java 
AdStudentDao adStudentDao = openSession.getMapper(AdStudentDao.class) ;
Student student = adStudentDao.getStudentById2(1001) ;
System.out.println(student);
```

引出问题，查询数据库时，实际上使用了两条Mysql语句，一条查询学生的基本属性，一条查询了班级的属性，当我们只需要学生的基本属性时，另外一条sql语句就造成了数据库性能的浪费：

解决： Mybatis的配置有aggressiveLazyLoading和LazyLoadingEnabled 

| 设置名                | 描述                                                         | 有效值        | 默认值           |
| :-------------------- | :----------------------------------------------------------- | :------------ | :--------------- |
| lazyLoadingEnabled    | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。 | true \| false | false            |
| aggressiveLazyLoading | 开启时，任一方法的调用都会加载该对象的所有延迟加载属性。 否则，每个延迟加载属性会按需加载（参考 `lazyLoadTriggerMethods`)。 | true \| false | false （在 3.4.1 |

#### 场景3

同理，从数据库中查询班级中的学生，也可以使用分步查询的方式：

```xml 
    <resultMap id="MyClassMap" type="com.wingchi.beans.t_class">
        <id property="classId" column="class_id"></id>
        <result property="classNumber" column="class_number"></result>
        <result property="headTeacher" column="headTeacher"></result>
        <result property="major" column="major"></result>
        告诉mybatis，students使用select的语句去查找
        property：t_class里的属性名
        select： sql语句的id标识
        column：用来传入sql语句中的变量 
        ofType：查出来的集合的元素的类型
        <collection property="students" select="com.wingchi.Dao.AdStudentDao.getStudentByClassId"
                     column="class_id" ofType="student"> </collection>
    </resultMap>
    <select id="getClassById2" resultMap="MyClassMap" >
        select *from t_class where class_id = #{class_id}
    </select>
```

```xml 
 <select id="getStudentByClassId" resultType="student" parameterType="int">
        select *from t_student where class_id=#{id}
</select>
```

collection和association是支持延迟加载的，这里的select属性表示要执行的sql语句，column表示执行sql要传递的参数



## 动态SQL

> 动态 SQL 是 MyBatis 的强大特性之一。如果你使用过 JDBC 或其它类似的框架，你应该能理解根据不同条件拼接 SQL 语句有多痛苦，例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL，可以彻底摆脱这种痛苦。
>
> 借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。 

| 原符号   | <    | <=   | >    | >=   | &    | '    | "    |
| -------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 替换符号 | `<`  | `<=` | `>`  | `>=` | `&`  | `'`  | `"`  |



https://segmentfault.com/a/1190000039043522

#### if

在if标签是我们最常使用的。在查询，删除，更新的时候可能都会用到，必须结合test属性联合使用。

如果if中的test属性成立，那么就会执行标签内的语句。

例如：按照条件查询符合的学生：

```java 
 List<Student> getStudentOnCondition(Student student) ;  //Dao中的接口 
 

// 主程序 
DynamicStuDao dynamicStuDao = openSession.getMapper(DynamicStuDao.class) ;
Student s1 = new Student() ;
s1.setName("%王%");
s1.setAge(18)  ;
//查找姓名中有王的，和年龄为18岁的。 
List<Student> list = dynamicStuDao.getStudentOnCondition(s1) ;
for(Student student :list){
    System.out.println(student);
}
```



```xml 
    <select id="getStudentOnCondition" resultMap="map">
        select *from t_student
        <where>
            <if test="id!=null">
                id>#{id}
            </if>
            <!--        "" 和 and 是都是xml中的转义字符！-->
            <if test="name!=null  and name!='' ">
                and name like #{name}
            </if>
            <if test="age!=null">
                and age =#{age}
            </if>
        </where>
    </select>
```

MyBatis自动帮我们拼接SQL语句。 注意到如果if条件一个都不通过的话，会导致出现这样的sql语句：

select *from t_student where ， 所以在编写时，要加上where标签或者写成 ：select *from t_student where 1=1。如果写成1=1，数据量大时就会影响sql的查询效率

#### where

使用where标签，在查询语句时自动补上where子句，当没有查询条件时不会加上where子句，就避免了上面的问题。

剩下的就是 <if>标签的and子句，Mybatis会自动帮我们删掉不必要的and。

#### trim 

 trim 有四个属性：

1. prefix：为子SQL语句整体添加一个前缀
2. prifixOverrides： 去除整体字符串可能多出的字符
3. suffix：为整体添加一个后缀
4. suffixOverride：去除整体后面可能多出来的字符串

如果trim的子元素没有返回任何东西，prefix和suffix都不会添加。

```xml
    <select id="getStudentOnCondition" resultMap="map">
        select *from t_student
        <trim prefix="where" suffixOverrides="and">
            <if test="id!=null">
                id>#{id} and
            </if>
            <if test="name!=null  and name!='' ">
                name like #{name} and
            </if>
            <if test="age!=null">
                age =#{age}
            </if>
        </trim>
    </select>
```

#### foreach

动态sql的另一个常见场景就是对集合进行遍历，尤其是**在构建in条件语句的时候**

```xml 
<!--    List<Student> getStudentById(@Param("ids")List<Integer> id ) ;-->
        <select id="getStudentById" resultMap="map">
            select *from t_student where id in
<!--            collection:要遍历的集合,如果接口方法中没有指定名称，就用类型的首字母小写，这里用注解指定了名称。
                separator: 每次遍历的元素的分割符
                open:以什么符号开始
                close:以什么符号结束
                index：i 索引， 如果遍历的是list，就是保存该元素的索引
                如果遍历的是map，指定的变量就是保存了当前遍历的元素的key
                item ：如果是list，给遍历出的元素起一个变量名方便引用，如果是map，就是元素的value
--> 
            <foreach collection="ids" separator="," close=")" open="(" item="id_item">
                #{id_item}
            </foreach>
        </select>
```

#### choose

<if>  有点像java中的switch语句。使用choose只需要满足一个条件就进去，其他的条件不再判断：

```xml
<!--    List<Student> getStudentOnChoose(Student student) ;-->
        <select id="getStudentOnChoose" resultMap="map">
            select *from t_student
            <where>
                <choose >
                    <when test="id!=null">
                        id=#{id}
                    </when>
                    <when test="name!=null">
                        name like #{name}
                    </when>
                    <otherwise>
                        sex = 'f'
                    </otherwise>
                </choose>
            </where>
        </select>
```



#### set

用于动态更新语句的类似解决方案叫做 *set*。*set* 元素可以用于动态包含需要更新的列，忽略其它不更新的列。比如：

```xml 
 <update id="UpdateStudent">
            update t_student
            <set>
                <if test="age!=null">age=#{age},</if>
                <if test="sex!=null">sex=#{sex},</if>
                <if test="name!=null">name=#{name}</if>
                where id = #{id}
            </set>
        </update>
```

#### sql

用于抽取可重用的sql，和include一起使用。





### OGNL表达式

Object Graph Navigation Language对象图导航语言，一种强大的表达式语言，通过它可以非常方便地操作对象属性

### 分页查询展示

①、原生sql的limit分页

`LIMIT <N> OFFSET <M>`字句

limit 3 offset 0 ,表示从0号记录开始查询，最多取3条。（sql记录集索引是从0开始的）

limit <N> 表示每页最多 N条。

OFFSET<M> 表示数据库索引从第几条开始查起。  

分页查询的关键：先确定每页要展示的结果数量PageSize，然后根据当前页的索引pageIndex，从1开始（第一页），确定limit和offset的值。

- limit总是 = pageSize
- offset = pageSize * (pageIndex-1)   前面查了 PageIndex-1页，每页查了pageSize条。 



#### PageHelper插件

在调用mapper方法前，先调用一次PageHelper.startPage(pageNum,pageSzie) 来设置分页查询参数即可，其中pageNum为记录页数，pageSize为单页结果数。



https://blog.csdn.net/qq_2300688967/article/details/82192546



### 整合mybatis-plus

```xml 
   <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.0</version>
 </dependency>
```

**自动配置分析：**

- 配置类：MybatisPlusAutoConfiguration
- 自动配置了 SqlSessionFactory ，
- 自动配置：mapperLocation：默认值为`classpath*:/mapper/**/*.xml`，任意包的类路径下的所有mapper文件夹下的xml文件都是sql映射文件
- 自动配置 SqlSessionTemplate
- @Mapper标注的接口会被自动扫描，同时也支持@MapperScan

**注解**

- @TableName

表名注解，实体类对应的数据表名，标注在实体类上。

```java 
@TableName("user") // 表示数据库中的表叫user
public class User {
    private int id ;
}
```

- @TableId：标识主键注解，用在实体类的主键字段
- @TableField：字段注解（非主键）

| 属性  | 类型    | 必须指定 | 默认值 | 描述               |
| :---- | :------ | :------- | :----- | :----------------- |
| value | String  | 否       | ""     | 数据库字段名       |
| exist | boolean | 否       | true   | 是否为数据库表字段 |





# MyBaits-plus

@Mapper和 **@Repository** 的区别

@Mapper是mybaits的注解，作用： 1.不用写mapper映射文件(xml), 2.  告诉sprigng框架此接口的实现类由Mybatis负责创建，并将其实现类对象存储到spring容器中。3.如果有多个mapper接口，可以用MapperScaner指定扫描包，将所有mapper接口都扫描进来。 

【测试：写了Mapper不需要写Repository，但不知道怎么回事】







## 出现过的问题

mybatis-plus有问答页面，有很多问题，可以上去查看！



### 配置方面

mybatis-plus中的mapperLocations提供了默认的：

` mapperLocations ="classpath*:/mapper/**/*.xml" `



**mybatis-plus下使用自定义的xml文件，自定义sql语句：**

基本一样的： 接口可以继承BaseMapper，无碍，在接口下面自定义方法：

```java 
public interface ImageMapper extends BaseMapper<Image> {
    @Select("select *from  image where category = #{category}")
    ArrayList<Image> selectByCategory(@Param("category") String category) ;

    ArrayList<Image> selectBycategoryList(@Param("list") List<String> categoryList);
}
```

编写xml文件 ：*注意！xml文件不能和接口一样放在java文件，而是要放在resource目录， IDEA 默认不会编译源码文件夹中的 XML 文件，可以参照以下方式解决*

- xml文件放在resource目录
- 在pom文件指定mapper.xml文件的位置。 

![image-20220430112159825](D:\Typora\自服务\mybatis.assets\image-20220430112159825.png)







# driud

https://segmentfault.com/a/1190000039005979


