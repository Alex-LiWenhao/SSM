

MyBatis框架简介

\1.    持久层框架 （和数据库进行交互）

a)     Spring为容器框架

b)     SpringMVC和前端交互框架

\2.    轻量级框架

## JDBC

\1.    与数据库交互的补助

a)     加载驱动

b)     创建数据库连接

c)     创建一个preparedStatem 对象

d)     编写SQL语句

e)     封装结果集

f)     对异常处理，关闭资源

## DBUtils

1.封装了JDBC，提供结果集自动映射

## MyBatis的优点

\1.    结果集自动映射

\2.    异常处理（异常的细分）

\3.    缓存

\4.    事务管理

\5.    SQL解耦到了配置文件中

​    1.易于上手和掌握。

​	2.sq|写在xml里,便于统-管理和优化。

​	3.解除sq|与程序代码的耦合。

​    4.提供映射标签，支持对象与数据库的orm字段关系映射

​	5.提供对象关系映射标签，支持对象关系组建维护

​	6.提供xm标签,支持编写动态sql。

## MyBatis的缺点

​		1.sql工作量很大,尤其是字段多、关联表多时，更是如此。

​		2.sq|依赖于数据库，导致数据库移植性差。

   	3.由于xml里标签id必须唯一，导致DAO中方法不支持方法重载。

   	4.字段映射标签和对象关系映射标签仅仅是对映射关系的描述，具体实现仍然依赖于sql。(比如配置 了一对多Collection标签, 如果sql里没有join子表或查询子表的话，查询后返回的对象是不具备对象关系的，即Collection的对象为null)

​		5.DAO层过于简单，对象组装的工作量较大。

​		6.不支持级联更新、级联删除。

  	 7.编写动态sq|时不方便调试，尤其逻辑复杂时。

  	 8.提供的写动态sql的xm标签功能简单(连struts都比不上) ，编写动态sq|仍然受限，且可读性低。

​		9.使用不当,容易导致N+1的sq|性能问题。

​		10.使用不当,关联查询时容易产生分页bug.

​		 11.若不查询主键字段,容易造成查询出的对象有覆盖'现象。

​    	  12.参数的数据类型支持不完善。(如参数为Date类型时， 容易报没有get. set方法，需在参数上加@param)

 		  13.多参数时，使用不方便，功能不够强大。(目前支持的方法有map、 对象、注解@param以及默认采用012索弓|位的方式)

​			14.缓存使用不当，容易产生脏数据。 

## 工具和框架的区别

\1.    MyBatis框架提供了与数据库交互的一整套解决方案

\2.    工具只提供一些单一的功能

## 项目中为啥不使用jdbc和dbutils的原因

1． JDBC的步骤繁锁，结果集需要自己处理（浪费大量时间 ）JDBC速度快，封装层次越高 性能损失越高

2． DBUtils虽然提供了了结果集自动映射，但是SQL语句硬编写在程序中（SQL与代码耦合度太高，维护成本高）

```java
QueryRunner.query(“select username，sex from user”, new BeanHandler<User>(User.class));
```

## Hibernate框架简介

\1.    ORM框架（object Relation Mapping） 全自动对象关系映射

sql语句---对象  对象—对象

```java
public class user **{**

 

  private String name**;**

  private Integer id**;**

  private String password**;**

  private Integer money**;**

  private String sex**;**

**}**

session.load(User.class, 1)

select * from user where id = 1;

select name,id,password,sex form user where id = 1;
```

\2.    定制SQL（无法自己书写优化sql）

\3.    HQL SQL

## MyBatis实例

\1.    创建一个JavaMaven项目

\2.    创建测试表，以及封装用的pojo类，和操作数据库的dao接口

\3.    导入依赖

```xml
<!-- 数据库驱动依赖 -->

    <dependency>

      <groupId>mysql</groupId>

      <artifactId>mysql-connector-java</artifactId>

      <version>5.1.47</version>

    </dependency>

 

   <!-- 添加mybaits依赖 -->

    <dependency>

      <groupId>org.mybatis</groupId>

      <artifactId>mybatis</artifactId>

      <version>3.4.6</version>

     </dependency>

 

   <!-- 测试依赖 -->

   <dependency>

      <groupId>junit</groupId>

      <artifactId>junit</artifactId>

      <version>4.12</version>

      <scope>test</scope>

    </dependency>

 

    <!-- 日志依赖 -->
    <dependency>

      <groupId>log4j</groupId>

     <artifactId>log4j</artifactId>

      <version>1.2.17</version>

    </dependency>
```

\4.    写MyBatis全局配置（配置MyBatis如何运行，MyBatis连接哪个数据库）

```xml
<?xml version=**"1.0"** encoding=**"UTF-8"** ?>

<!DOCTYPE configuration

 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"

 "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

 <environments default=**"development"**>

  <environment id=**"development"**>

   <transactionManager type=**"JDBC"**/>

   <!-- 配置数据源 -->

   <dataSource type=**"POOLED"**>

    <property name=**"driver"** value=**"com.mysql.jdbc.Driver"**/>

    <property name=**"url"** value=**"jdbc:mysql://localhost:3306/demo"**/>

    <property name=**"username"** value=**"root"**/>

    <property name=**"password"** value=**"123456"**/>

   </dataSource>

  </environment>

 </environments>

 <!-- 引入sql配置文件 

  resource：也是类路径

 -->

 <mappers>

  <mapper resource=**"UserMapper.xml"**/>

 </mappers>

</configuration>
```

\5.    写sql配置文件（sql解耦放在xml配置文件中）

```xml
<?xml version=**"1.0"** encoding=**"UTF-8"** ?>

<!DOCTYPE mapper

 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

 <!-- namespace:名称空间： 写接口的全类名，相当于告诉MyBatis这个配置文件与那个接口绑定-->

<mapper namespace=**"com.tanzhou.dao.UserMapper"**>

  <!-- 

  select标签：用于定义一个查询操作

  id：对应方法名

  resultType：指定结果映射类型（查询必须有resultType标签，必须规定返回值类型）

  \#{属性名}：相当于占位符？并且通过名字与传入参数进行关联

   -->

 <select id=**"selectById"** resultType=**"com.tanzhou.pojo.User"**>

  **select \* from user where id = #{id}**

 </select>

</mapper>
```

\6.    测试

```java
  public void demoTest**()** **throws** Exception **{**

​    //1.读取全局配置文件创建SqlSessionFactory对象

​    //工厂模式，工厂负责生成产品的。sqlSessionFactory就是工厂，SqlSession是产品

​    //SqlSession：Sql会话，connection连接对象（与数据库的一次会话）

​    String resource **=** "MyBatis_Config.xml"**;**

​    InputStream inputStream **=** Resources**.**getResourceAsStream**(**resource**);**

​    SqlSessionFactory sqlSessionFactory **=** **new** SqlSessionFactoryBuilder**().**build**(**inputStream**);**

​    

​    //2.获取会话对象SqlSession对象

​    SqlSession Session **=** sqlSessionFactory**.**openSession**();**

​    

​    //3.通过SqlSession对象，获取Dao接口的实现。

​    UserMapper mapper **=** Session**.**getMapper**(**UserMapper**.**class**);**

​    User result **=** mapper**.**selectById**(**1**);**

​    

​    System**.**out**.**println**(**result**);**

​    

​    //4.释放资源

​    Session.close**();

  **
```

## 日志(log4j)

\1.    记录程序运行的关键步骤，配置好了之后，控制台就会打印日志（MyBatis执行的关键步骤）

\2.    日志依赖于log4j.properties配置文件（日志会自动在类路径下找log4j.properties配置文件）

```properties
  log4j.rootLogger=DEBUG,Console

  log4j.appender.Console=org.apache.log4j.ConsoleAppender

  log4j.appender.Console.layout=org.apache.log4j.PatternLayout

  log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n

  log4j.logger.org.apache=INFO
```

 日志配置相关说明：

- log4j.rootLogger：根日志，配置了日志级别为 INFO，预定义了名称为 console、file 两种附加器
- log4j.appender.console：console 附加器，日志输出位置在控制台
- log4j.appender.console.layout：console 附加器，采用匹配器布局模式
- log4j.appender.console.layout.ConversionPattern：console 附加器，日志输出格式为：日期 日志级别 [类名] - 消息换行符
- log4j.appender.file：file 附加器，每天产生一个日志文件
- log4j.appender.file.File：file 附加器，日志文件输出位置 logs/log.log
- log4j.appender.file.layout：file 附加器，采用匹配器布局模式
- log4j.appender.A3.MaxFileSize：日志文件最大值
- log4j.appender.A3.MaxBackupIndex：最多纪录文件数
- log4j.appender.file.layout.ConversionPattern：file 附加器，日志输出格式为：日期 日志级别 [类名] - 消息换行符

##  MyBatisCRUD

\1.   创建一个javaMaven项目

\2.   创建测试表，以及pojo类，和操作数据库的dao接口

```java
public interface UserDao {
    //根据id进行查询
    public User selectById(int id);
	//根据id进行删除
    public boolean delectById(int id);
    //根据传入user对象，进行更新
    public int updateOne(User user);
    //根据传入user对象，进行插入
    public int insertOne(User user);
}

```

 \3.   导入依赖

\4.   写Mybatis全局配置（配置Mybatis连接哪个数据库，规定Mybatis该如何运行）

\5.   sql配置文件（sql解耦放在xml配置文件中）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace：名称空间：写接口的全类名，相当与告诉MyBatis这个配置文件时实现哪个接口 -->
<mapper namespace="com.tanzhou.dao.UserDao">
    <!-- 
    select标签：用于定义一个查询操作 
    id：为对应方法名，相当于方法的实现 
    resultType：指定结果集映射类型（查询操作必须指定）
    #{属性名}：相当于？占位符，并且通过名字与传入参数的值关联
    -->
    
    <!-- 
        public User selectById(int id);
    
        public boolean delectById(int id);
        
        public int updateOne(User user);
    
        public int insertOne(User user);
     -->
    <select id="selectById" resultType="com.tanzhou.pojo.User" >
        select * from user where id=#{id}
    </select>
    
<!—-
不要加分号；
    返回参数类型不用写 
    增删改返回的参数，是影响的行数
    如果是接口需要返回时Boolean（影响0行自动封装false，否则true）
    -->
    <update id="updateOne">
        UPDATE `user` SET money=100 WHERE id=#{id}
    </update>
    <!-- 
    #{属性名}：如果传入的是对象，那么MyBatis会自动根据sql中的属性名拼接出get方法，从对象中获取对应的值
     -->
    <insert id="insertOne">
        INSERT INTO `user` (username,sex,money) VALUES (#{username},#{sex},#{money})
    </insert>
        
    <delete id="delectById">
        DELETE FROM `user` WHERE id=#{id}
    </delete>
</mapper>

```

\6.   代码测试

```java
7.	public class TestUser {
8.	
9.	    @Test
10.	    public void testMfybatis() throws Exception {
11.	        //1.根据全局配置文件创建出SqlSessionFactory对象
12.	        //工厂模式中，工厂负责生产产品。SqlSessionFactory时工厂，SqlSession是产品
13.	        String resource = "org/mybatis/example/mybatis-config.xml";
14.	        InputStream inputStream = Resources.getResourceAsStream(resource);
15.	        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
16.	        
17.	        //2.获取会话对象SqlSession对象
18.	        //SqlSession：sql会话（与数据库连接的一次会话）
19.	        SqlSession sqlSession = sqlSessionFactory.openSession();
20.	        
21.	        //3.通过SqlSession对象获取，dao接口的实现，通过代理模式进行创建实现
22.	        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
23.	        User result = userMapper.selectOne(1);
24.	        System.out.println(result);
25.			
26.			//4.释放资源
27.	sqlSession.close();
28.	    }
29.	}

```

## MyBatis全局配置文件

\1.   作用：规定MyBatis的运行方式的全局配置

XML 映射配置文件	

MyBatis 的配置文件包含了影响 MyBatis 行为甚深的设置（settings）和属性（properties）信息。

文档的顶层结构如下： 		

​					configuration 配置		

​					properties 属性		

​					settings 设置		

​					typeAliases 类型命名		

​					typeHandlers 类型处理器		

​					objectFactory 对象工厂		

​					plugins 插件		

​					environments 环境			

​										environment 环境变量				

​															transactionManager 事务管理器				

​															dataSource 数据源		

​					databaseIdProvider 数据库厂商标识		

​					mappers 映射器			

在配置 mybatis-config.xml 配置文件的时候，最关键的一点就是，必须按照上面标签的顺序进行配置。

### \2.   properties（属性）

```xml
<!-- mybatis可以使用properties来引入外部properties配置文件的内容
	 resource：引入类路径下的资源 
	 url:映入网络路径或者磁盘路径下的资源 -->
	<properties resource="jdbc.properties"></properties>
```

### \3.   settings（设置）

```xml
<!-- setting标签：这个标签中配置的属性会改变MyBatis的运行方式 -->
	<settings>
		<!-- name：配置项的key；value：配置项的值 -->
		<!--开启驼峰映射：otherName， other_name：在Mybatis框架中不区分大小写比如结果映射，对象的别名  -->
		<setting name="mapUnderscoreToCamelCase " value="true"/>
	</settings>

```

​		a)  驼峰映射其实就是将中间的“_”去除

​		b)  MyBatis和数据库都是不区分大小的

​	延迟加载：

| lazyLoadingEnabled    | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。                特定关联关系中可通过设置 `fetchType`                属性来覆盖该项的开关状态。 | true \| false |                             false |
| --------------------- | ------------------------------------------------------------ | ------------- | --------------------------------: |
| aggressiveLazyLoading | 当开启时，任何方法的调用都会加载该对象的所有属性。                否则，每个属性会按需加载（参考 `lazyLoadTriggerMethods`)。 | true \| false | false （在 3.4.1 及之前的版本默认 |

​	

### \4.   typeAliases（类型别名）

```xml
<!--typeAliases标签：为java类起别名，有别名了的java类，在SQL文件中就可以使用别名了  -->
	<typeAliases>
		<!-- typeAlias标签：为一个java类起别名，type属性：java类的全类名；alias：别名 -->
		<!-- 如果没有alias属性，默认别名为类名（不区分大小写）-->
		<typeAlias type="" alias=""/>
		<!-- package标签：为java类批量起别名时，为包名下所有java起别名：name属性：指定包名，默认别名为类名 -->
		<!-- 批量起别名时：如果需要单独使用设置别名，也可以使用@Alias注解 -->
		<package name=""/>
	</typeAliases>
```

​		a)  别名的作用：1.使配置更加简单，便捷；2.便于维护修改 

### \5.   typeHandlers（类型处理器）

```xml

<typeHandlers>
		<package name=""/>
		<typeHandler handler="" javaType="" jdbcType=""/>
</typeHandlers>
```

a)  作用：因为mysql与java变量的类型不同，类加载处理器进行类型转换，将数据库数据类型转化为Java的数据类型

### \6.   objectFactory（对象工厂）

```xml
<!-- 对象工厂 -->
	<objectFactory type="">
		<property name="" value=""/>
	</objectFactory>
```

a)  作用：根据sql配置文件与dao接口来进行代理对象的创建，根据dao接口创建代理类的

### \7.   plugins（插件）

```xml
<!-- 插件 -->
<plugins></plugins> 
```

a)  作用：通过AOP技术进行方法的增强，增强的代码就是插件

### \8.   environments（环境配置）



### \9.   environment（环境变量）

### \10. transactionManager（事务管理器）

```xml
<transactionManager type="JDBC" />
```

​            JDBC – 这个配置就是直接使用了 JDBC 的提交和回滚设置，它依赖于从数据源得到的连接来管理事务作用域。          

​            MANAGED – 这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）

### \11. dataSource（数据源）

```xml
<!-- 多个环境放在这个标签内：default属性：Mybatis当前运行的环境 -->
	<environments default="development">
		<!-- 配置一个环境。id：环境的唯一标识符。每一个环境都需要事物管理器和数据源 -->
		<environment id="development">
			<transactionManager type="JDBC" />
			<!-- 配置数据源 -->
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}" />
				<property name="url" value="${jdbc.url}" />
				<property name="username" value="${jdbc.username}" />
				<property name="password" value="${jdbc.password}" />
			</dataSource>
		</environment>
```

### \12. databaseIdProvider（数据库厂商标识）

```xml
<!-- 根据不同的数据库选择不同的sql语句 -->
	<databaseIdProvider type="DB_VENDOR">
	<!-- property标签：为不同的数据库厂商取名；那么：数据库标识符；value：别名 -->
		<property name="SQL Server" value="sqlserver"/>
 		<property name="DB2" value="db2"/>
  		<property name="Oracle" value="oracle" />
        <property name="MySQL" value="sql"/>
	</databaseIdProvider>
```

![image-20200219202925266](C:\Users\mac\AppData\Roaming\Typora\typora-user-images\image-20200219202925266.png)

### \13. mappers（映射器）

```xml
<!-- 引入sql配置文件 resource：也是类路径 -->
	<mappers>
		<!-- url:可以从磁盘或者网络中引入
			resource：从类路径引入
			class：直接引入接口全类名	
		 -->
		<mapper resource="UserMapper.xml" /> 
		<mapper class="com.tanzhou.dao.UserMapper" />
        
		<mapper url=""/> 
		<!-- 批量 name:配置dao的包路径-->
		<package name="com.tanzhou.dao"/>
	</mappers>
```

![](C:\Users\mac\AppData\Roaming\Typora\typora-user-images\image-20200219204222102.png)

### \14. 配置文件配置

```xml
<?xml version=**"1.0"** encoding=**"UTF-8"** ?>

<!DOCTYPE configuration

 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"

 "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

  <!-- properties标签：引入外部文件，与spring中的context:property-placeholder标签作用一样- -->

  <!-- resource：从类路径下开始引用；url：从磁盘路径或者网络路径引用 -->

  <properties resource=**"jdbc.properties"**></properties>

 

  <!-- settings标签：这个标签中配置的属性会改变MyBatis的运行方式 -->

  <settings>

​    <!-- name：配置项的key；value：配置项的值 -->

​    <!-- 开启驼峰映射：otherName，other_name；在MyBatis框架中不区分大小写（比如结果集映射，对象的别名） -->

​    <setting name=**"mapUnderscoreToCamelCase"** value=**"false"** />

  </settings>

 

  <!-- typeAliases标签：为java类起别名，有别名了的java类，在sql文件中就可以使用别名了 -->

  <typeAliases>

​    <!-- typeAlias标签：为一个java类起别名；type属性：java类的全类名；alias：别名 -->

​    <!-- 如果没有alias属性，默认别名为类名（不区分大小写） -->

​    <!-- <typeAlias type="com.tanzhou.pojo.User" alias="ser"/> -->

 

​    <!-- package标签：为java批量起别名，为包名下面所有java起别名；name属性：指定包名；默认别名为类名 -->

​    <!-- 批量起别名时；如果需要单独设置别名，可以使用@Alias注解 -->

​    <package name=**"com.tanzhou.pojo"** />

  </typeAliases>

 

  <!-- 类型处理器 -->

  <!-- <typeHandlers></typeHandlers> -->

  

  <!-- 对象工厂 -->

  <!-- <objectFactory type=""></objectFactory> -->

 

  <!-- 插件 -->

  <!-- <objectFactory type=""></objectFactory> -->

 

  <!-- environments标签：多个环境配置放在这个标签里 ；default属性：MyBatis当前使用的环境 -->

  <environments default=**"development"**>

​    <!-- environment标签：配置一个环境。都需要一个事物管理器和数据源；id属性：环境的唯一标识符 -->

​    <environment id=**"development"**>

​      <!-- 配置事物管理器 -->

​      <transactionManager type=**"JDBC"** />

​      <!-- 配置数据源 -->

​      <dataSource type=**"POOLED"**>

​        <!-- ${key值}：MyBatis看到${}符号会自动去引入的配置文件中寻找，通过key确定位置 -->

​        <property name=**"driver"** value=**"${jdbc.driver}"** />

​        <property name=**"url"** value=**"${jdbc.url}"** />

​        <property name=**"username"** value=**"${jdbc.username}"** />

​        <property name=**"password"** value=**"${jdbc.password}"** />

​      </dataSource>

​    </environment>

 

​    <environment id=**"demo"**>

​      <!-- 配置事物管理器 -->

​      <transactionManager type=**"JDBC"** />

​      <!-- 配置数据源 -->

​      <dataSource type=**"POOLED"**>

​        <!-- ${key值}：MyBatis看到${}符号会自动去引入的配置文件中寻找，通过key确定位置 -->

​        <property name=**"driver"** value=**"${jdbc.driver}"** />

​        <property name=**"url"** value=**"${jdbc.url}"** />

​        <property name=**"username"** value=**"xiaosan"** />

​        <property name=**"password"** value=**"${jdbc.password}"** />

​      </dataSource>

​    </environment>

  </environments>

 

  <!-- databaseIdProvider标签：根据不同的数据库选择不同的sql语句，项目的移植性更高 -->

  <databaseIdProvider type=**"DB_VENDOR"**>

​    <!-- property标签：为不同的数据库厂商起别名；name：数据库标识符；value：别名 -->

​    <property name=**"MySQL"** value=**"sql"** />

​    <property name=**"Oracle"** value=**"jgw"** />

  </databaseIdProvider>

 

  <!-- mappers标签：标签内可以引入多个sql配置 -->

  <mappers>

​    <!-- url：可以从磁盘或者网络引入；resource：从类路径引入；class：直接引入接口的全类名 -->

​    <mapper class=**"com.tanzhou.dao.UserDao"**/>

  </mappers>

</configuration>
```

##  MyBatisSql映射文件

\1.   insert标签中的useGeneratedKeys="true"属性与keyProperty="成员变量名"

a)  作用：在插入语句中：获取插入语句的数据库中的自增id，并且赋值给传入java类的成员变量

b)  具体过程：useGeneratedKeys="true"：调用jdbc的getGeneratedKeys方法获取自增主键；keyProperty="属性名"：将获取的值，封装到java类的哪个成员变量中

```xml
<insert id="insertOne" useGeneratedKeys="true" keyProperty="id">
		insert into user (username, sex, money, other_name)
		values (#{username}, #{sex}, #{money}, #{otherName})
</insert>
```

### #{}的取值过程

#### \1.   传入一个参数：

a)  java对象：#{成员变量名}--->通过拼接get方法或者反射调用，拿到传入对象的值

```xml
<insert id="insertOne" useGeneratedKeys="true" keyProperty="id">
		insert into user (username, sex, money, other_name)
		values (#{username}, #{sex}, #{money}, #{otherName})
	</insert>
```

b)  map集合：#{key}--->调用map中的get（key）方法获取对应的value值

```xml
<!-- public User insertByMap(Map<String, Object> map); -->
	<insert id="insertByMap">
		insert into user (username, sex, money, other_name)
		values (#{username}, #{sex}, #{money}, #{otherName})
	</insert>
```

c)   基本变量：别无选择

```xml
<!-- public Boolean delectById(int id); -->
	<delete id="delectById" >
		delete from user where id=#{id}
	</delete>
```

d) oracle数据库不支持自增，oracle使用序列来完成模拟自增
  每次插入的数据的主键值是从序列中拿到的值，如何获取这个值？

 

```xml
  <insert id="save" parameterType="com.tanzhou.domain.Employee" databaseId="oracle">
   		<selectKey keyProperty="id" order="BEFORE" resultType="integer">
   			编写查询主键的sql语句 :keyProperty:查出主键值封装给java对象的哪个属性
   			resultType:查出的数据返回值类型
   			order="BEFORE":先运行selectKey查询id的sql，查询出id值封装给java对象的id属性，再运行插入的sql，就可以从对象中取出id属性所对应的值
             order="AFTER":先运行插入的sql，从序列中取出新值做为id，再运行selectKey查询id的，设置到对象中的id属性中去	
   			select s_employee_id.nextval from dual 
   		</selectKey>
  		insert into employee(id,name,sex,email)values(#{id},#{empName},#{sex},#{email})
  </insert> 



<insert id="save" parameterType="com.tanzhou.domain.Employee" databaseId="oracle">
   		<selectKey keyProperty="id" order="AFTER" resultType="integer">
   		<!-- 
   			 order="AFTER":先运行插入的sql，从序列中取出新值做为id，
   			 再运行selectKey查询id的，设置到对象中的id属性中去
   		 -->
   			select s_employee_id.currval from dual
   		</selectKey>
  		insert into employee(id,name,sex,email)values(s_employee_id.nextval,#{empName},#{sex},#{email})
  </insert>
```



#### \2.   传入多个参数：

a)  MyBatis会将传入多个参数封装成一个map集合，key值默认为：符合参数顺序的索引；

b)  如果需要自己制定key值：我们可以通过@Param注解来制定参数对应的key值；

c)   #{java对象名.属性名}#{user.username}：这个表达式的意思是，java对象名作为key在map中先找出java对象，然后通过拼接或者反射“成员变量名”获取这个成员变量的值；

d）如果传入的参数Collection（list，set）类型或者数组类型，MyBatis也会进行特殊处理会将这些传入的集合或者1数组封装到map集合中

如果是collection集合,map中的key对应的是collection（所有的集合都可以使用）

如果是List集合，可以使用的key：list

如果是数组，可以使用的key：array

#### \3.   拼接get方法与反射传入成员变量名获取参数

a)  拼接get方法，获取参数（getUsername()）（区分大小写）；

b)  通过反射，获取参数（属性名需与变量名相同（区分大小写）

### #{}与${}

#### \1.   差别

​		a)  #{}：是参数预编译的方式，相当于PreparedStatement，使用？替代，参数后来都是预编译进去的           

​		b)  ${}：不是参数预编译的方式，而是直接和sql语句进行拼串，发生sql注入攻击

#### \2.   ${}的使用场景

​		a)  sql语句中只有参数位置能够使用字符串，其他位置不能够使用字符串，不能够使用““号

### 结果集封装

#### \1.   resultType标签与dao接口返回值类型相同

​		a)  类型都为pojo对象：结果集会封装到pojo对象中，并且返回一个pojo对象

```xml
<!-- public User selectById(int id); -->
	<select id="selectById" resultType="com.tanzhou.pojo.User" >
		select * from user where id=#{id}
	</select>
```

​		b)  类型都为map集合：resutlType标签为map，接口返回也为map时，默认返回一条数据，map集合的key为字段名，value为字段值；

```xml
<!-- public Map<String, Object> selectMapById(int id); -->
	<select id="selectMapById" resultType="map" >
		select * from user where id=#{id}
	</select>
```

​		c)   类型都为List集合：结果集无法处理，封装失败

```xml
<!-- public List<Object> selectListById(int id);-->
	<select id="selectListById" resultType="list">
		select * from user where id=#{id}
	</select>
```

​		d)  类型都为map集合，并且在接口处使用@MapKey("成员属性名")时：默认返回多条数据；@MapKey("成员变量名")注解：规定key值为java对象的某个成员变量名；map的value值为java对象

```xml
<!-- @MapKey("id") 
		public Map<Integer, User> selectMapAll()
	-->
	<select id="selectMapAll" resultType="com.Alex.pojo.User">
		select * from user
	</select>
```

#### \2.   resultType标签与dao接口返回值类型不相同

​		a)  resultType标签为pojo对象，接口类型为List集合：返回一个List集合，resultType标签：选择的属性是List集合中存入对象的全类名；Mybatis会返回值类型，选择封装的集合类型

```xml
<!-- public List<User> selectAll(); -->
	<select id="selectAll" resultType="com.Alex.pojo.User">
		select * from user
	</select>
```

### SQL配置文件

```xml
<?xml version=**"1.0"** encoding=**"UTF-8"** ?>

<!DOCTYPE mapper

 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

 

<mapper namespace=**"com.tanzhou.dao.UserMapper"**>

  <!-- public boolean delectById(int id); -->

  <delete id=**"delectById"**>

​    **delete from user where id=#{id}**

  </delete>

 

  <!-- public int updateById(int id); -->

  <update id=**"updateById"**>

​    **update user set money=10000 where id=#{id}**

  </update>

 

  

  <!-- #{username}：1.拼接get方法，获取参数（getUsername()）（区分大小写）；2.通过反射，获取参数（属性名需与变量名相同（区分大小写） -->

  <!-- 

  在插入语句中：获取插入语句的数据库中的自增id，并且赋值给传入java类的成员变量

  useGeneratedKeys="true"：调用jdbc的getGeneratedKeys方法获取自增主键

  keyProperty="属性名"：将获取的值，封装到java类的哪个成员变量中

   -->

   <!-- 

  传入一个参数：

​    java对象：#{成员变量名}：通过拼接get方法或者反射调用，拿到传入对象的值

​    map集合：#{key}：调用map中的getValue方法获取对应的value值

​    基本变量：别无选择

  传入多个参数：

​    1、MyBatis会将传入多个参数封装成一个map集合，key值默认为：符合参数顺序的索引；

​    2、我们可以通过@Param注解来制定参数对应的key值；

​    3、#{java对象名.属性名}#{user.username}：这个表达式的意思是，java对象名作为key在map中先找出

​       java对象，然后通过拼接或者反射“成员变量名”获取这个成员变量的值； 

   -->

   <!-- 

   \#{}：是参数预编译的方式，相当于PreparedStatement，使用？替代，参数后来都是预编译进去的

   ${}：不是参数预编译的方式，而是直接和sql语句进行拼串，发生sql注入攻击

   sql语句中只有参数位置能够使用字符串，其他位置不能够使用字符串，不能够使用““号

   -->

  <!-- public int insertOne(User user); -->

  <insert id=**"insertOne"** useGeneratedKeys=**"true"** keyProperty=**"id"**>

​    **insert into user(id, username, sex, money)**

​    **values (#{id}, #{username}, #{sex}, #{money})**

  </insert> 

  

  <!-- public User selectById(int id); -->

  <select id=**"selectById"** resultType=**"com.tanzhou.pojo.User"**>

​    **select \* from user where**

​    **id=#{id}**

  </select>

  

  <!-- 

  返回一个List集合，resultType标签：选择的属性是List集合中存入对象的全类名

  Mybatis会返回值类型，选择封装的集合类型

   -->

  <!-- public List<User> selectAll(); -->

  <select id=**"selectAll"** resultType=**"com.tanzhou.pojo.User"**>

​    **select \* from user**

  </select>

  

  <!-- public Map<String, Object> selectMapById(int id); -->

  <!-- 

  resutlType标签为map，接口返回也为map时，默认返回一条数据，map集合的key为字段名，value为字段值；

   -->

  <select id=**"selectMapById"** resultType=**"map"**>

​    **select \* from user where id=#{id}**

  </select>

  

  <!-- 

  resultType标签为java对象，接口返回为Map集合时，默认返回多条数据

  @MapKey("成员变量名")注解：规定key值为java对象的某个成员变量名

  map的value值为java对象

   -->

  <!--

   @MapKey("id")

   public Map<Integer, User> selectMapAll()

   -->

  <select id=**"selectMapAll"** resultType=**"com.tanzhou.pojo.User"**>

​    **select \* from user**

  </select>

  

  <!-- public User selectByMapId(Map<String, Object> map); -->

  <select id=**"selectByMapId"** resultType=**"com.tanzhou.pojo.User"**>

​    **select \* from user where id=#{ids}**

  </select>

  

  <!-- public List<Object> selectListById(int id); -->

  <select id=**"selectListById"** resultType=**"list"**>

​    **select \* from user where id=#{id}**

  </select>

</mapper>
```

##  自定义和默认结果集映射

### \1.   默认MyBatis映射规则

a)  按照字段名与成员变量名一一对应的规则（不区分大小写）

b)  开启驼峰命名法（去掉字段名中的“_”，比如：otherName other_name）

### \2.   自定义映射规则

a)  resultMap标签： 自定义映射对应规则；type：制定封装的pojo对象，id：唯一标识符

b)  id标签：告诉MyBatis主键列；column属性：指定数据库字段名；property：指定对应的pojo成员变量名

c)   result标签：普通列，column属性：指定数据库字段名；property：指定对应的pojo成员变量名

```xml
<!-- resultMap标签：自定义映射对应规则；type:制定封装的pojo对象 id：唯一标识符-->
<resultMap type="com.Alex.pojo.UserMapper" id="usermapper">
	<!-- id标签：告诉MyBatis主键列 column属性：指定数据库字段名 Proporty：指定对应pojo类里面的成员变量名 -->
		<id column="id" property="did"/>
		<result column="username" property="dusername"/>
		<result column="sex" property="dsex"/>
		<result column="money" property="dmoney"/>
</resultMap>
```

### \3.   resultMap和resultType的区别

a)  resultType使用MyBatis默认的规则，resultMap使用自定义的映射规则

### 级联属性映射

#### \1.   使用场景：

当映射的java对象中含有对象的情况，并且查询出来的结果集需要封装到对象的对象中，称为嵌入结果映射，可以使用级联属性

#### \2.   作用：

将结果集返回的值，嵌入映射到对象的对象中 

#### \3.   property="doupoWifes.dUsername"：

doupoWifes为映射对象中的成员变量名，dUsername为嵌入对象的成员变量名，通过“.”告诉MyBatis，property后面的变量处理方式不同 

```xml
<!-- 自定义封装规则,使用级联属性封装联合查询出的结果 -->
	<resultMap type="com.Alex.pojo.DouPoOneN" id="DouPoOneN">
		<id column="id" property="did"/>
		<result column="sex" property="dsex"/>
		<result column="" property=""/>
		<!-- 通过这个“.”告诉Mybatis ，属性是个对象，讲这个字段封装到这个对象中 -->
		 <result column= " wifeld" property="doupoWifes.dld"/>
		<result column= "wifeUsername" property="doupoWifes.dUsername" />
		<result column="status" property="doupoWifes.dStatus"/>
	</resultMap>
```

### 数据库行与行之间的关系

#### \1.   一对一

a)  比如：药老对应一个青仙子

b)  联系字段存储方式：随便

c)   在pojo类中的设计：

d)  标签的使用：association标签

##### association标签

```xml
 <!-- association标签：使用这个标签，代表告诉MyBatis，DouPoOneO联合了一个对象，与这个对象的关系为一一对应 -->

​    <!-- property属性：成员变量名；javaType属性：对象的全类名 -->

​    <association property=**"doupoWifes"** javaType=**"com.tanzhou.pojo.DoupoWifes"**>

​      <!-- 定义doupoWifes成员变量，该如何 -->

​      <id column=**"wifeId"** property=**"dId"**/>

​      <result column=**"wifeUsername"** property=**"dUsername"**/>

​      <result column=**"status"** property=**"dStatus"**/>

​    </association>

  </resultMap>
```

分步查询：

```xml
	<resultMap id="IEmpByStep" type="com.tanzhou.domain.Employee">
		<!-- 
			1.先按照员工的id查询员工的信息
			2.根据查询员工信息的did值再去部门表查询部门信息
			3.把查询出来的部门设置到员工对象中去
		 -->
		<id column="id" property="id"></id>
		<result column="emp_name" property="empName"></result>
		<result column="sex" property="sex"></result>
		<result column="email" property="email"></result>
		<!-- select:表示当前指定的dept属性是调用select指定的方法查询出来的结果 
		column:指定将那一列的值传递给这个方法
		-->
		<association property="dept" select="com.tanzhou.dao.DepartmentMapper.getDeptById" column="d_id">
		</association>
	</resultMap>
```



#### \2.   一对多

a)  比如：消炎对应熏儿，云芝。

b)  联系字段存储方式：多的一方

c)   在pojo类中的设计：

d)  标签的使用：collection标签

##### collection标签

```xml
 <!-- 一对多 -->

  <resultMap type=**"com.tanzhou.pojo.DouPoOneN"** id=**"DouPoOneN"**>

​    <id column=**"id"** property=**"dId"**/>

​    <result column=**"username"** property=**"dUsername"**/>

​    <result column=**"grade"** property=**"dGrade"**/> 

​    <result column=**"sex"** property=**"dSex"**/>

​    <!-- collection标签：告诉Mybat封装的数据是一对多的关系，并且多的一方封装在集合中。除了这个集合，其他的都是封装的一的数据 -->

​    <!-- property属性：成员变量名，指定那个成员变量是集合；ofType：指定集合里面元素的类型；javaType：全类名，指定变量类型-->

​    <collection property=**"doupoWifes"** javaType=**"list"** ofType=**"com.tanzhou.pojo.DoupoWifes"**>

​      <id column=**"wifeId"** property=**"dId"**/>

​      <result column=**"wifeUsername"** property=**"dUsername"**/>

​      <result column=**"status"** property=**"dStatus"**/>

​    </collection>

  </resultMap>
```

分步查询：

```xml
	<resultMap type="com.tanzhou.domain.Department" id="IDeptStep">
		<id column="id" property="id"></id>
		<result column="deptName" property="deptName"/>
		<collection property="emps" select="com.tanzhou.dao.EmployeeMapper2.getEmpsByDeptId" 
		column="id" fetchType="lazy"></collection>
		<!--lazy：表示使用延迟加载，  eager：表示立即加载-->
		<!--如果分步查询需要传递多个参数，mybatis会封装到map中进行传递
		column="{key1=column1,key2=column2}"
		  -->
	</resultMap>
```

#### \3.   多对多

鉴别器：

```xml
<!-- 
		需求：
			如果查询出来员工的sex=男，那么就把部门信息查询出来，否则不查询部门信息
			如果查询出来员工的sex=女，那么就把emp_name这一列赋值给email
	 -->
	 <resultMap id="IEmpDis" type="com.tanzhou.domain.Employee">
	 	<id column="id" property="id"></id>
		<result column="emp_name" property="empName"></result>
		<result column="sex" property="sex"></result>
		<result column="email" property="email"></result>
		<!-- 鉴别器
			column:指定判断的列名，
			javaType:字段值所对应的java类型，
		 -->
		<discriminator javaType="string" column="sex">
			<case value="男" resultType="com.tanzhou.domain.Employee">
				<association property="dept" select="com.tanzhou.dao.DepartmentMapper.getDeptById" column="d_id"></association>
			</case>
			<case value="女" resultType="com.tanzhou.domain.Employee">
			<!-- 部门信息不查询，直接将查询出来的员工的姓名赋值到员工的emial上 -->
			<id column="id" property="id"></id>
			<result column="emp_name" property="empName"></result>
			<result column="emp_name" property="email"></result>
			<result column="sex" property="sex"></result>
		</case>
		</discriminator>
	 </resultMap>
```



### sql配置文件

\1.   DoupoManMapper

```xml
<?xml version=**"1.0"** encoding=**"UTF-8"** ?>

<!DOCTYPE mapper

 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace=**"com.tanzhou.dao.DoupoManMapper"**>

  <!-- resultMap属性：切换标签，告诉MyBatis需要切换运行方式 -->

  <!-- User selectById(int id); -->

  <select id=**"selectById"** resultMap=**"DoupoMan"**>

​    **select \* from doupo_man where id = #{id}**

  </select>

  

​    <!-- resultMap标签： 自定义映射对应规则；type：制定封装的pojo对象，id：唯一标识符-->

  <resultMap type=**"com.tanzhou.pojo.DoupoMan"** id=**"DoupoMan"**>

​    <!-- id标签：告诉MyBatis主键列；column属性：指定数据库字段名；property：指定对应的pojo成员变量名 -->

​    <id column=**"id"** property=**"dId"**/>

​    <result column=**"username"** property=**"dUsername"**/>

​     <result column=**"sex"** property=**"dSex"**/>

​    <result column=**"grade"** property=**"dGrade"**/> 

  </resultMap>

  

</mapper>

 

\2.   DoupoWifesMapper

<?xml version=**"1.0"** encoding=**"UTF-8"** ?>

<!DOCTYPE mapper

 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace=**"com.tanzhou.dao.DoupoWifesMapper"**>

  <!-- DoupoWifes selectWifesByUsername(String username); -->

 

  <select id=**"selectWifesByUsername"** resultMap=**"DoupoWifes"**>

​    **select \* from doupo_wife where man_username = #{username}**

  </select>

 

  <resultMap type=**"com.tanzhou.pojo.DoupoWifes"** id=**"DoupoWifes"**>

​    <id column=**"id"** property=**"dId"** />

​    <result column=**"username"** property=**"dUsername"** />

​    <result column=**"status"** property=**"dStatus"** />

  </resultMap>

</mapper>

 

\3.   DoupoMoreMapper

<?xml version=**"1.0"** encoding=**"UTF-8"** ?>

<!DOCTYPE mapper

 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace=**"com.tanzhou.dao.DoupoMoreMapper"**>

  <!-- DouPoOneO selectByIdOneO(int id); -->

  

  <select id=**"selectByIdOneO"** resultMap=**"DouPoOneO"**>

​    **SELECT man.id, man.username, man.grade, man.sex, wife.id wifeId, wife.username wifeUsername, wife.status** 

​    **from doupo_man man LEFT JOIN doupo_wife wife** 

​    **ON man.username=man_username** 

​    **WHERE man.id=#{id}**

  </select>

  

  <!-- 一对一 -->

  <resultMap type=**"com.tanzhou.pojo.DouPoOneO"** id=**"DouPoOneO"**>

​    <id column=**"id"** property=**"dId"**/>

​    <result column=**"username"** property=**"dUsername"**/>

​    <result column=**"grade"** property=**"dGrade"**/> 

​    <result column=**"sex"** property=**"dSex"**/>

​    <!-- association标签：使用这个标签，代表告诉MyBatis，DouPoOneO联合了一个对象，与这个对象的关系为一一对应 -->

​    <!-- property属性：成员变量名；javaType属性：对象的全类名 -->

​    <association property=**"doupoWifes"** javaType=**"com.tanzhou.pojo.DoupoWifes"**>

​      <!-- 定义doupoWifes成员变量，该如何 -->

​      <id column=**"wifeId"** property=**"dId"**/>

​      <result column=**"wifeUsername"** property=**"dUsername"**/>

​      <result column=**"status"** property=**"dStatus"**/>

​    </association>

  </resultMap>

  

  <!-- 自定义封装规则，使用级联属性封装联合查询出的结果 -->

  <!-- <resultMap type="com.tanzhou.pojo.DouPoOneN" id="DouPoOneN">

​    <id column="id" property="dId"/>

​    <result column="username" property="dUsername"/>

​    <result column="sex" property="dSex"/>

​    <result column="grade" property="dGrade"/> -->

​    <!-- 通过"."告诉MyBatis，属性是个对象，讲这个字段封装到这个对象中 -->

​    <!-- <result column="wifeId" property="doupoWifes.dId"/>

​    <result column="wifeUsername" property="doupoWifes.dUsername"/>

​    <result column="status" property="doupoWifes.dStatus"/>

  </resultMap> -->

  

​      <!-- DouPoAll selectByIdOneN(int id); -->

  <select id=**"selectByIdOneN"** resultMap=**"DouPoOneN"**>

​    **SELECT man.id, man.username, man.grade, man.sex, wife.id wifeId, wife.username wifeUsername, wife.status** 

​    **from doupo_man man LEFT JOIN doupo_wife wife** 

​    **ON man.username=man_username**

​    **WHERE man.id=#{id}**

  </select>

  

  <!-- 一对多 -->

  <resultMap type=**"com.tanzhou.pojo.DouPoOneN"** id=**"DouPoOneN"**>

​    <id column=**"id"** property=**"dId"**/>

​    <result column=**"username"** property=**"dUsername"**/>

​    <result column=**"grade"** property=**"dGrade"**/> 

​    <result column=**"sex"** property=**"dSex"**/>

​    <!-- collection标签：告诉Mybat封装的数据是一对多的关系，并且多的一方封装在集合中。除了这个集合，其他的都是封装的一的数据 -->

​    <!-- property属性：成员变量名，指定那个成员变量是集合；ofType：指定集合里面元素的类型；javaType：全类名，指定变量类型-->

​    <collection property=**"doupoWifes"** javaType=**"list"** ofType=**"com.tanzhou.pojo.DoupoWifes"**>

​      <id column=**"wifeId"** property=**"dId"**/>

​      <result column=**"wifeUsername"** property=**"dUsername"**/>

​      <result column=**"status"** property=**"dStatus"**/>

​    </collection>

  </resultMap>

  

  <!-- DouPoOneN selectByIdOneNStep(int id);-->

  <select id=**"selectByIdOneNStep"** resultMap=**"DoupoOneOStep"**>

​    **select username from doupo_man where id = #{id}**

  </select>

  

  <resultMap type=**"com.tanzhou.pojo.DouPoOneN"** id=**"DoupoOneOStep"**>

​    <id column=**"id"** property=**"dId"**/>

​    <result column=**"username"** property=**"dUsername"**/>

​    <result column=**"grade"** property=**"dGrade"**/> 

​    <result column=**"sex"** property=**"dSex"**/>

​    <!-- 告诉MyBatis这个成员变量的封装，需要调用其他的查询语句，然后将查询出来的结果集封装好之后，嵌入到这个property="doupoWifes"属性中 -->

​    <!-- select="sql语句的唯一标识符"：MyBatis需要的调用的语句 -->

​    <collection property=**"doupoWifes"** column=**"username"** select=**"com.tanzhou.dao.DoupoWifesMapper.selectWifesByUsername"** />

  </resultMap>

   

</mapper>
```

##  动态sql（Dynamic Sql）

### \1.   if标签

​	a)  概述：在xml中书写判断语句，根据条件拼接sql语句

​	b)  

```xml
  <!-- 

​     test属性： test="判断条件"；

​     编写判断条件为dId != null，取出传入java对象的属性值，判断是否等于null。

​     满足则拼接字符串，不满足则不拼接

​     -->

​      <if test=**"dId != null"**>

​        **id > #{dId}** 

​      </if>
```

### \2.   where标签

​	a)  概述：只能帮助我们去除前面多余的and，并且自动拼接where关键字

​		解决方案：

​		1.给where后面加上1=1 if里面的所有条件都加上and或者or

​		2.MyBatis中使用where关键字 替换成where标签所有的查询条件包括在内

​		3.MyBatis会将where标签中拼接的SQL，把第一个多出来的and或者or给去掉

​			如果一定要用where标签，就需要将and放在条件之前

### \3.   set标签

​	a)  概述：去除多余的分隔符“，”，只能去除后面的“，”

### \4.   trim标签

​	a)  概述：帮我们添加，或者截取字符串的前缀和后缀

​	b)  

```xml
<!-- trim标签：帮我们添加，或者截取字符串 -->

​    <!--

​      prefix：前缀，如果有条件拼接，那么拼接where，没有条件拼接，不拼接where

​      prefixOverrides：前缀重写，整个if标签体的内容去掉整个字符串前面多余的字符串

​      suffix：后缀，拼接后的字符串条件加上一个后缀

​      suffixOverrides：后缀重写，整个if标签体的内容去掉整个字符串后面多余的字符

​     -->

​    <trim prefix=**"where"** suffixOverrides=**"and"**>

```

### \5.   foreach标签

​	a)  概述：遍历集合，使用集合中的值帮我们拼接sql

​	b)  

```xml
 <!-- foreach标签：遍历集合，使用集合中的值帮我们拼接sql -->

​    <!-- 

​      collection：遍历集合的名字，map集合中的key

​      item：遍历拿到的值

​      open：拼接前缀

​      separator：

​      close：拼接后缀

​      index：在list中储存坐标，在map中储存key

​     -->

​    <foreach collection=**"ids"** item=**"itemIds"** open=**"("** separator=**","** close=**")"** >

​      **#{itemIds}**

​    </foreach>
```

### \6.   choose标签

​	a)  概述：分支选择，if---else

​	b)  

```xml
 <select id=**"selectByConditionInChoose"** resultMap=**"DoupoWifes"**>

​    **select \* from doupo_wife** 

​    <where>

​      <choose>

​        <when test=**"dId != null"**>

​          **id = #{dId}**

​        </when>

​        <otherwise>

​          **username = #{dUsername}**

​        </otherwise>

​      </choose>

​    </where>

  </select>

```

### \7.   sql标签与include标签

​	a)  概述：抽取重复的sql（片段复用）

​	b)  

```xml
  <sql id=**"tableAll"**>

​    **select \* from doupo_wife**

  </sql>

 

  <select id=**"selectWifesByUsername"** resultMap=**"DoupoWifes"**>

​    <include refid=**"tableAll"** />

​    **where man_username = #{username}**

  </select>
```



### 内置参数

​	_parameter:代表整个参数，方法传递了一个参数，__parameter就是这个参数如果传入了多个参数，参被封装成map，而_标签，那么__parameter就是代表这个map

​	_databaseId:如果全局配置了databaseIDProvider标签，那么这个参数就会有值,代表当前数据库别名

### xml中的特殊字符

\1.  

```
 &lt; < 小于号
```

\2.  

```
 &gt; > 大于号
```

\3.  

```
 &amp; & 和
```

\4.  

```
 &apos; ' 单引号
```

\5.  

```
 &quot; " 双引号
```

### OGNL表达式

\1.   概述

​	a)  OGNL是Object Graphic Navigation Language(对象图导航语言)的缩写，他是一	个开源项目。Struts框架使用OGNL作为默认的表达式语言。

​	b)  MyBatis中的test标签中使用的值也是OGNL表达式语言

\2.   使用

​	a)  OgnlContext对象可以看做一个带有解析功能的强大map集合

​	b)  OGNL表达式需要使用OgnlContext进行解析

​	c)   解析OGNL表达式时，需要使用的变量，都需要存在于解析时使用的OgnlContext对象中，变量名为key值，变量对应的值为value值。

​	d)  MyBatis会自动在OgnlContext对象中放入的参数

​        i.     传入的参数

​        ii.     _parameter：传入了单个参数，就代表这个参数；传入多个参数，就代表map集合的引用地址

​       iii.     _databaseId：代表当前连接的数据库

\3.   作用

​	a)  OgnlContext对象提供了，将符合规范的字符串表达式转化为可运算的代码和变量。为if语句的解耦提供了基础

### sql配置文件

```xml
<?xml version=**"1.0"** encoding=**"UTF-8"** ?>

<!DOCTYPE mapper

 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace=**"com.tanzhou.dao.DoupoWifesMapper"**>

  <!-- DoupoWifes selectWifesByUsername(String username); -->

  <sql id=**"tableAll"**>

​    **select \* from doupo_wife**

  </sql>

  <select id=**"selectWifesByUsername"** resultMap=**"DoupoWifes"**>

​    <include refid=**"tableAll"** />

​    **where man_username = #{username}**

  </select>

  <select id=**"selectById"** resultMap=**"DoupoWifes"**>

​    <include refid=**"tableAll"** />

​     **where id = #{id}**

  </select>

  <!-- List<DoupoWifes> selectByCondition(DoupoWifes doupoWifes); -->

  <select id=**"selectByCondition"** resultMap=**"DoupoWifes"**>

​    **select \* from doupo_wife** 

​    <!-- trim标签：帮我们添加，或者截取字符串 -->

​    <!--

​      prefix：前缀，如果有条件拼接，那么拼接where，没有条件拼接，不拼接where

​      prefixOverrides：覆盖截取条件语句前多余的字符串

​      suffix：后缀，同上

​      suffixOverrides：覆盖截取条件语句后多余的字符串

​     -->

​    <trim prefix=**"where"** suffixOverrides=**"and"**>

​    <!-- 帮助我们去除前面多余的and，并且自动拼接where关键字 -->

​    <!-- <where> -->

​     <!-- if标签：在xml中书写判断语句 -->

​     <!-- 

​     test属性： test="判断条件"；

​     编写判断条件为dId != null，取出传入java对象的属性值，判断是否等于null。

​     满足则拼接字符串，不满足则不拼接

​     -->

​      <if test=**"dId != null"**>

​        **id > #{dId}** 

​      </if>

​      <!-- 

​        OGNL表达式 · 

​        MyBatis：放入context中的东西

​        传入的参数

​        _parameter：传入了单个参数，就代表这个参数；传入多个参数，就代表map集合的引用地址

​        _databaseId：代表当前连接的数据库

​        &&：and  &amp;&amp;                                                                                     

​        ||：or

​        空串：&quot;&quot;

​       -->

​      <if test=**"dSex != null and dSex != """**>

​         **and sex = #{dSex}** 

​      </if>    

​    <!-- </where> -->

​    </trim>

  </select>


  <!-- int updateOne(DoupoWifes doupoWifes); -->

  <update id=**"updateOne"**>

​    **update doupo_wife**

​    <!-- set标签：去除多余的分隔符“，”，只能去除后面的“，” -->

​    <set>

​      <if test=**"dUsername != null and dUsername != """**>

​        **username = #{dUsername}**

​      </if>

​      <if test=**"dSex != null and !dSex.equals("")"**>

​        **,sex = #{dSex}**

​      </if> 

​      <if test=**"dStatus != null and dStatus != """**>

​        **,status = #{dStatus}**

​      </if>

​    </set> 

​    **where id = #{dId}**

  </update>


  <!-- List<DoupoWifes> selectByIds(List<Integer> ids); -->

  <select id=**"selectByIds"** resultMap=**"DoupoWifes"**>

​    **select \* from doupo_wife where id in** 

​    <!-- foreach标签：遍历集合，使用集合中的值帮我们拼接sql -->

​    <!-- 

​      collection：遍历集合的名字，map集合中的key

​      item：遍历拿到的值

​      open：拼接前缀

​      separator：

​      close：拼接后缀

​      index：在list中储存坐标，在map中储存key

​     -->


​    <foreach collection=**"ids"** item=**"itemIds"** open=**"("** separator=**","** close=**")"** >

​      **#{itemIds}**

​    </foreach>

  </select>


  <!-- List<DoupoWifes> selectByConditionInChoose(DoupoWifes doupoWifes); -->

  <select id=**"selectByConditionInChoose"** resultMap=**"DoupoWifes"**>

​    **select \* from doupo_wife** 

​    <where>

​      <choose>

​        <when test=**"dId != null"**>

​          **id = #{dId}**

​        </when>

​        <otherwise>

​          **username = #{dUsername}**

​        </otherwise>

​      </choose>

​    </where>

  </select>

  <resultMap type=**"com.tanzhou.pojo.DoupoWifes"** id=**"DoupoWifes"**>

​    <id column=**"id"** property=**"dId"** />

​    <result column=**"username"** property=**"dUsername"** />

​    <result column=**"sex"** property=**"dSex"** />

​    <result column=**"status"** property=**"dStatus"** />

  </resultMap>
</mapper>
```

## 缓存概述

\1.   缓存，数据储存在内存中；数据库，数据储存在磁盘中。

### 一级缓存

\1.   作用范围： SqlSession级别的缓存，默认存在

\2.   特性：只要是查询过的数据，MyBatis就会保存在一级缓存中，并且下次查询的时候，会先看缓存中是否有数据，有数据直接返回，不查询数据库

\3.   数据来源：来源于数据库中

\4.   缓存失效的情况：

a)  不同SqlSession执行相同的查询，不同SqlSession使用不同的一级缓存

b)  不同的查询语句

c)   执行任何一个增删改操作，一级缓存会自动情况，保证与数据库数据的一致性

d)  手动清空一级缓存

### 二级缓存

\1.   作用范围：namespace级别的缓存，默认关闭（可以在xml中开启）

\2.   开启二级缓存步骤：

a)  全局配置文件配置

b)  sql配置文件配置

c)   pojo类序列化

\3.   数据来源：所有数据来源于一级缓存，一级缓存提交或者关闭后，数据会复制到二级缓存中

### 查询顺序

\1.   先查询二级缓存，然后查询一级缓存，如果一二级缓存都没有，查询数据库。

\2.   最开始查询出来的数据会放在一级缓存中，并且一级缓存提交或者关闭之后，一级缓存中的数据才会放到二级缓存中

### 缓存的介绍

\1.   key要为唯一标识符

\2.   key的值，要程序能够知道

\3.   key值为唯一程序，value值为对应的返回值+

### cache标签属性

\1.   eviction=" LRU "  缓存的回收策率

a)  LRU – 最近最少使用：移除最长时间不被使用的对象。

b)  FIFO – 先进先出：按对象进入缓存的顺序来移除它们。

c)   SOFT – 软引用：基于垃圾回收器状态和软引用规则移除对象。

d)  WEAK – 弱引用：更积极地基于垃圾收集器状态和弱引用规则移除对象。

\2.   flushInterval="60000"：（刷新间隔）属性可以被设置为任意的正整数，设置的值应该是一个以毫秒为单位的合理时间量。 默认情况是不设置，也就是没有刷新间隔，缓存仅仅会在调用语句时刷新。

\3.   size="512"：（引用数目）属性可以被设置为任意正整数，要注意欲缓存对象的大小和运行环境中可用的内存资源。默认值是 1024。

\4.   readOnly="true”：（只读）属性可以被设置为 true 或 false。只读的缓存会给所有调用者返回缓存对象的相同实例。 因此这些对象不能被修改。这就提供了可观的性能提升。而可读写的缓存会（通过序列化）返回缓存对象的拷贝。 速度上会慢一些，但是更安全，因此默认值是 false。

### 增删改查标签cache属性

\1.   useCache：是否使用二级缓存开关，对一级缓存没有影响

\2.   flushCache：sql执行后，是否刷新一二级缓存。增删改默认开启，查询默认关闭

## 一线程对应一connection对象

\1.   connection对象控制事务

\2.   多线程使用一个connection对象，会造成事务控制没有明确的作用域，造成事务混乱的问题

\3.   一线程对应一connection对象，明确规范connection事务的作用域（connection.commit()方法提交的sql语句）

![多线程](D:\desktop\框架\img\多线程.png)

代码

```java
\1.   单例模式创建connection

/**

 \* 低效率懒汉模式单例模式

 \* **@author** LBDcondition

 */

public class GetConnection **{**

  

  private static Connection connection**;**

  

  //私有化构造方法，防止外部创建对象

  private GetConnection**()** **{**

  

  **}**

  

  //加上同步锁，保证多线程安全

  public static synchronized Connection getConnection**()** **{**

​    **if** **(**connection **==** **null****)** **{**

​      **try** **{**

​        //加载数据库驱动

​        Class**.**forName**(**"com.mysql.jdbc.Driver"**);**

​        String url **=** "jdbc:mysql://localhost:3306/demo"**;**

​        //获取与数据库的连接，创建一个tcp连接

​        connection **=** DriverManager**.**getConnection**(**url**,** "root"**,** "123456"**);**

​        //开启事物控制

​        connection**.**setAutoCommit**(****false****);**

​      // System.out.println(connection);

​      **}** **catch** **(**Exception e**)** **{**

​        e**.**printStackTrace**();**

​      **}**    

​    **}** 

​    **return** connection**;**

  **}**

 

**}**

\2.   测试多线程事物处理

public class testTrancation **{**

  

  public static void main**(**String**[]** args**)** **{**

​    

​    Runnable runnable **=** **new** Runnable**()** **{**

​      public void run**()** **{**

​        money**();**

​      **}**

​    **};**

​    //创建线程

​    Thread thread **=** **new** Thread**(**runnable**);**

​    //开启线程

​    thread**.**start**();**

​    

​    money02**();**

  **}**

  

  public static void money**()** **{**

​    //获取connection对象

​    Connection connection **=** GetConnection**.**getConnection**();**

​    **try** **{**

​      Statement statement **=** connection**.**createStatement**();**

​      

​      String sql **=** "update doupo_wife set money = money - 10 where username = '美杜莎' "**;**

​      statement**.**executeUpdate**(**sql**);**

​      Thread**.**sleep**(**1000**);**

 

​      sql **=** "update doupo_wife set money = money + 10 where username = '云芝' "**;**

​      statement**.**executeUpdate**(**sql**);**

​      

​      //提交事务

​      connection**.**commit**();**//commit作用域的问题

​    **}** **catch** **(**Exception e**)** **{**

​      // TODO Auto-generated catch block

​      e**.**printStackTrace**();**

​    **}**  

  **}**

  

  public static void money02**()** **{**

​    Connection connection **=** GetConnection**.**getConnection**();**

​    **try** **{**

​      Statement statement **=** connection**.**createStatement**();**

​      String sql **=** "update doupo_wife set money = money - 10 where username = '美杜莎' "**;**

​      statement**.**executeUpdate**(**sql**);**

​      Thread**.**sleep**(**3000**);**

​      int i **=** 0**/**0**;**

​      sql **=** "update doupo_wife set money = money + 10 where username = '青仙子' "**;**

​      statement**.**executeUpdate**(**sql**);**

​      

​      connection**.**commit**();**

​    **}** **catch** **(**Exception e**)** **{**

​      // TODO Auto-generated catch block

​      e**.**printStackTrace**();**

​    **}**  

  **}**

**}
```

## 代理模式

\1.   概述：为其他对象提供一种代理以控制对这个对象的访问

\2.   作用：为被代理对象，提供方法增强。并且责任清晰

### 静态代理

\1.   缺点：

a)  代码编写工作量大

b)  维护成本高，违反开闭原则

c)   代码耦合性高,增强代码硬编码在代码中

\2.   优点：

a)  可为一个类不同的方法，提供不同的增强；

b)  调用速度快；

\3.   代码

a)  接口

```java
package com**.**tanzhou**.**staticproxy**;**

//接口，规范代理类和被代理类的方法

public interface CarFactory **{**

  void makeCar**(**String username**);**

**}**
```

b)  被代理类

```java
package com**.**tanzhou**.**staticproxy**;**

//被代理类

public class Afactory **implements** CarFactory**{**

  

  @Override

  public void makeCar**(**String username**)** **{**

​    System**.**out**.**println**(**"制造一个" **+** username **+** "车" **);**

  **}**

 

**}**
```

c)   代理类

```java
package com**.**tanzhou**.**staticproxy**;**

 

//代理类需要和被代理类实现相同的接口

public class AStaticProxy **implements** CarFactory**{**

  

  Afactory afactory **=** **new** Afactory**();**

 

  @Override

  public void makeCar**(**String username**)** **{**

​    //前置增强

​    welcome**();**

​    afactory**.**makeCar**(**username**);**

​    //后置增强

​    getOut**();**

  **}**

 

  public void welcome**()** **{**

​    System**.**out**.**println**(**"欢迎，欢迎，奥利给"**);**

  **}**

  

  public void getOut**()** **{**

​    System**.**out**.**println**(**"喜提新车"**);**

  **}**

**}**
```

d)  测试

```java
package com**.**tanzhou**.**staticproxy**;**

 

**import** org**.**junit**.**Test**;**

 

public class TestProxy **{**

 

  @Test

  public void testProxy**()** **{**

​    AStaticProxy aStaticProxy **=** **new** AStaticProxy**();**

​    aStaticProxy**.**makeCar**(**"五红灵光"**);**  

  **}*

**}**
```



### jdk动态代理

\1.   优点：

a)  代码编写工作量小

b)  维护成本低，符合开闭原则

c)   代码耦合性低

\2.   缺点：

a)  只能为同一个类中的所有方法，提供相同的增强

b)  使用反射调用，调用速度慢

![静态代理](D:\desktop\框架\img\静态代理.png)

![动态代理](D:\desktop\框架\img\动态代理.png)

\3.   开闭原则

a)  开闭原则的定义: 一个软件实体,如类、模块和函数应该对扩展开放,对修改关闭.即一个软件实体应该通过扩展来实现变化,而不是通过修改已有的代码来实现变化.

\4.   代码

a)  接口

``` java
package com**.**tanzhou**.**staticproxy**;**

 

//接口，规范代理类和被代理类的方法

public interface CarFactory **{**

  

  void makeCar**(**String username**);**

  

**}
```



b)  被代理类

```java
package com**.**tanzhou**.**dynamic**.**proxy**;**

 

//使用jdk动态代理，必须实现接口。动态代理会根据接口class文件，创建出proxy代理类

public class Afactory **implements** CarFactory**{**

 

  @Override

  public void makeCar**(**String username**)** **{**

​    System**.**out**.**println**(**"制造一个" **+** username **+** "车" **);**

  **}**

 

**}
```

**

c)   创建proxy对象的对象

```java
package com**.**tanzhou**.**dynamic**.**proxy**;**

 

**import** java**.**lang**.**reflect**.**InvocationHandler**;**

**import** java**.**lang**.**reflect**.**Proxy**;**

//创建proxy对象，创建对象时，需要规定泛型，这个泛型proxy对象实现的接口

public class GetProxy**<**T**>{**

  

  public T getProxy**(**InvocationHandler handler**,** T object**){**

​    Object proxy **=** Proxy**.**newProxyInstance**(**object**.**getClass**().**getClassLoader**(),** object**.**getClass**().**getInterfaces**()** **,** handler**);**

​    //动态代理创建出来的都是object对象，这里根据传入的泛型进行强转

​    //因为proxy对象，会和被代理对象实现同样的接口，所以强转为实现接口的类型

​    **return** **(**T**)**proxy**;**

  **}**

  

**}
```

**

d)  调用处理器

```java
package com**.**tanzhou**.**dynamic**.**proxy**;**

 

**import** java**.**lang**.**reflect**.**InvocationHandler**;**

**import** java**.**lang**.**reflect**.**Method**;**

//调用处理器A，增强的代码写在此处

public class AHandler **implements** InvocationHandler**{**

  private Object obj**;**

  

  public AHandler**(**Object obj**)** **{**

​    **this****.**obj **=** obj**;**

  **}**

  

  @Override

  public Object invoke**(**Object proxy**,** Method method**,** Object**[]** args**)** **throws** Throwable **{**

​    welcome**();**

​    method**.**invoke**(**obj**,** args**);**

​    getOut**();**

​    **return** **null****;**

  **}**

  

  public void welcome**()** **{**

​    System**.**out**.**println**(**"欢迎，欢迎，奥利给"**);**

  **}**

  

  public void getOut**()** **{**

​    System**.**out**.**println**(**"喜提新车"**);**

  **}**

 

**}
```



 

```
package com**.**tanzhou**.**dynamic**.**proxy**;**

 

**import** java**.**lang**.**reflect**.**InvocationHandler**;**

**import** java**.**lang**.**reflect**.**Method**;**

 

//调用处理器B，增强的代码写在此处

public class BHandler **implements** InvocationHandler**{**

  private Object obj**;**

  

  public BHandler**(**Object obj**)** **{**

​    **this****.**obj **=** obj**;**

  **}**

  

  @Override

  public Object invoke**(**Object proxy**,** Method method**,** Object**[]** args**)** **throws** Throwable **{**

​    //前置增强

​    welcome**();**

​    method**.**invoke**(**obj**,** args**);**

​    //后置增强

​    getOut**();**

​    **return** **null****;**

  **}**

  

  public void welcome**()** **{**

​    System**.**out**.**println**(**"welcom to 北京"**);**

  **}**

  

  public void getOut**()** **{**

​    System**.**out**.**println**(**"滚滚滚"**);**

  **}**

 

**}
```



e)  配置文件

\##value值为：调用处理器全类名

```java
HandlerName**=**com.tanzhou.staticproxy.AHandler
```

f)   测试类

```java
package com**.**tanzhou**.**dynamic**.**proxy**;**

 

**import** java**.**io**.**InputStream**;**

**import** java**.**lang**.**reflect**.**Constructor**;**

**import** java**.**lang**.**reflect**.**InvocationHandler**;**

**import** java**.**util**.**Properties**;**

 

**import** org**.**junit**.**Test**;**

 

public class TestProxy **{**

 

  @Test

  public void testProxy**()** **{**

​    /*Wife proxy = new GetProxy<Wife>(new Afactory()).getProxy();

​    proxy.makeCar("五红灵光");*/

​    

  **}**

  

  @Test

  public void testProxy01**()** **throws** Exception **{**

​    Afactory afactory **=** **new** Afactory**();**

​    

​    //读取properties文件

​    Properties properties **=** **new** Properties**();**

​    //类路径

​    InputStream in **=** afactory**.**getClass**().**getClassLoader**().**getResourceAsStream**(**"handler.properties"**);**

​    properties**.**load**(**in**);**

​    //通过key获取value值

​    String value **=** properties**.**getProperty**(**"HandlerName"**);**

​    System**.**out**.**println**(**value**);**

​    

​    //通过反射获取class对象

​    Class**** clazz **=** Class**.**forName**(**value**);**

​    //获取有参构造

​    Constructor**** constructor **=** clazz**.**getConstructor**(**Object**.**class**);**

​    //调用有参构造，传入对象，创建调用处理器对象

​    InvocationHandler newInstance **=** **(**InvocationHandler**)** constructor**.**newInstance**(**afactory**);**

​    

​    //获取proxy对象

​    CarFactory proxy **=** **new** GetProxy**<**CarFactory**>().**getProxy**(**newInstance**,** afactory**);**

​    proxy**.**makeCar**(**"五红灵光"**);**

​    

  **}**

  

**}
```



## 缓存的设计

\1.   缓存产生的原因

a)  每次查询，都需要连接数据库进行查询，十分耗费性能

b)  相同的查询调用，调用不变，查询出来的结果也是不变的

c)   因此缓存应运而生，执行查询时，第一次调用查询会访问数据库，并且查询出来的数据放入缓存中，第二次相同的查询会直接从缓存中拿取数据

\2.   缓存的设计

a)  由于是相同的调用，导致相同的结果。那么缓存要为key-value的结构。

b)  key为调用，value为结果。第一次调用，查询数据库，并且将调用的请求作为key值，查询出来的结果作为value值，放入缓存中。第二次相同的调用，就会通过这个调用（key），在缓存中获取对应的value值

c)   缓存一致性的保证，只要执行修改数据库的操作，那么缓存就会被清空，保证与数据库的一致性

![cache](D:\desktop\框架\img\cache.png)

\3.   代码

a)  缓存调用调度器

```java
package com**.**tanzhou**.**util**;**

 

**import** java**.**lang**.**reflect**.**InvocationHandler**;**

**import** java**.**lang**.**reflect**.**Method**;**

**import** java**.**lang**.**reflect**.**Proxy**;**

**import** java**.**util**.**HashMap**;**

 

**import** com**.**tanzhou**.**service**.**impl**.**DoupoWifesServiceImpl**;**

/**

 \* jdk动态代理

 */

public class GetProxy**<**T**>** **implements** InvocationHandler**{**

  

  //被代理对象

  Object doupoWifesServiceImpl **=** **new** DoupoWifesServiceImpl**();**

  //缓存容器

  HashMap**<**String**,** Object**>** hashMap **=** **new** HashMap**<**String**,** Object**>();**

 

  @Override

  public Object invoke**(**Object proxy**,** Method method**,** Object**[]** args**)** **throws** Throwable **{**

​    //获取接口的全类名

​    String name **=** method**.**getDeclaringClass**().**getName**();**

​    //获取方法名

​    String methodName **=** method**.**getName**();**

​    //获取参数，并且转化为字符串

​    String parameter **=** ""**;**

​    **for** **(**Object object **:** args**)** **{**

​      String arg **=** String**.**valueOf**(**object**);**

​      parameter **=** parameter **+** arg**;**

​    **}**

​    //拼接全类名+方法名+参数作为缓存的key值

​    String key **=** name **+** methodName **+** parameter**;**

​    Object result **=** **null****;**

​    //方法调用过来，首先查询缓存，缓存没有在查询数据库

​    **if** **(**hashMap**.**get**(**key**)** **==** **null****)** **{**

​      //缓存没有的情况，执行目标方法，查询数据库

​      result **=** method**.**invoke**(**doupoWifesServiceImpl**,** args**);**  

​      //将查询出来的结果放入缓存中，key值为全类名+方法名+参数

​      hashMap**.**put**(**key**,** result**);**

​     **}****else** **{**

​      //缓存有的情况，直接返回查询出来的value值

​      result **=** hashMap**.**get**(**key**);**

​      System**.**out**.**println**(**"缓存查询"**);**

​    **}**

​    System**.**out**.**println**(**key**);**

​    **return** result**;**

  **}**

  

  //创建动态代理类

  public T getProxy**()** **{**

​    T proxy **=** **(**T**)**Proxy**.**newProxyInstance**(**doupoWifesServiceImpl**.**getClass**().**getClassLoader**(),** doupoWifesServiceImpl**.**getClass**().**getInterfaces**(),** **this****);**

​    **return** proxy**;**

  **}**

**}**
```

##  整合ehcache

### 	1.添加依赖

​		a)ehcache依赖文件

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.1.0</version>
</dependency>

```

​		b)添加slf4j日志依赖

​		

```xml
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-log4j12 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.25</version>
    <scope>test</scope>
</dependency>

```

### 2.配置mapper文件

```xml
<mapper namespace="org.acme.FooMapper">
  <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
  ...
</mapper>
```

### 3.配置ehcache.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <!--
       diskStore：为缓存路径，ehcache分为内存和磁盘两级，
       	此属性定义磁盘的缓存位置。参数解释如下：
     -->
    <diskStore path="F:\ehcache"/>
    <!--
       defaultCache：默认缓存策略
     -->
    <!--
      maxElementsInMemory:缓存最大数目
      maxElementsOnDisk：硬盘最大缓存个数。
      eternal:对象是否永久有效，一但设置了，timeout将不起作用。
      overflowToDisk:是否保存到磁盘，当系统当机时
      timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
      timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
      diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
      diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
      diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
      memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
      clearOnFlush：内存数量最大时是否清除。
      memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
      FIFO，first in first out，这个是大家最熟的，先进先出。
      LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
      LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
   -->
    <defaultCache
            eternal="false"
            maxElementsInMemory="10000"
            overflowToDisk="true"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="259200"
            memoryStoreEvictionPolicy="LRU"/>
</ehcache>
```

## 逆向工程

 1. ### 导入依赖

    ```xml
    <!-- https://mvnrepository.com/artifact/org.mybatis.generator/mybatis-generator-core -->
    <dependency>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-core</artifactId>
        <version>1.3.7</version>
    </dependency>
    
    ```

    ### 2.创建xml文件

    ​	

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE generatorConfiguration
    	  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
    	  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
    
    <generatorConfiguration>
    	<properties resource="jdbc.properties"/>
    	<!-- 指定逆向工程的运行环境-->
    	<!-- 生成简单查询的语句：<context id="mysqltest" targetRuntime="MyBatis3Simple"> -->
    	<!-- 生成复杂查询的语句：<context id="mysqltest" targetRuntime="MyBatis3"> -->
    		<context id="mysqltest" targetRuntime="MyBatis3Simple">
    	<!-- 连接数据库的属性 -->
    		<jdbcConnection
    			driverClass="${jdbc.driver}"
    			connectionURL="${jdbc.url}" userId="${jdbc.user}" password="${jdbc.password}">
    		</jdbcConnection>
    	<!-- java类型解析器 -->
    		<javaTypeResolver>
    			<!--把我们数据裤的整数类型解析为Integer类型  -->
    			<property name="forceBigDecimals" value="false" />
    		</javaTypeResolver>
    		<!-- 指定JavaBean的生成策略  targetPackage：生成实体类生成的目标包
    		targetProject:指定目标项目生成在哪里
    		-->
    		<javaModelGenerator targetPackage="com.Alex.pojo"
    			targetProject="src/main/java">
    			
    			<property name="enableSubPackages" value="true" />
    			<property name="trimStrings" value="true" />
    		</javaModelGenerator>
    	<!--sql生成策略，mapper.xml  -->
    		<sqlMapGenerator targetPackage="com.Alex.pojo"
    			targetProject="src/main/resources">
    			<property name="enableSubPackages" value="true" />
    		</sqlMapGenerator>
    	<!-- java客户端生成器，指定dao接口生成在哪里 -->
    		<javaClientGenerator type="XMLMAPPER"
    			targetPackage="com.Alex.pojo" targetProject="src/main/java">
    			<property name="enableSubPackages" value="true" />
    		</javaClientGenerator>
    
    	<!-- 根据数据库指定表生成文件 -->
    		<table tableName="student" domainObjectName="Student"></table>
    		<table tableName="sc" domainObjectName="Sc"></table>
    		<table tableName="course" domainObjectName="Course"></table>
    	</context>
    </generatorConfiguration>
    
    ```

    ### 3． 创建JavaTest测试类写入代码

    ```java
     List<String> warnings = new ArrayList<String>();
    
          boolean overwrite = true;
    
          File configFile = new File("generatorConfig.xml");//路径写类路径
    
          ConfigurationParser cp = new ConfigurationParser(warnings);
    
          Configuration config = cp.parseConfiguration(configFile);
    
          DefaultShellCallback callback = new DefaultShellCallback(overwrite);
    
          MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
    
          myBatisGenerator.generate(null);
    
    ​		
    ```

    

## 分页插件PageHelper

​	1.导入依赖

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.0.10</version>
</dependency>
```

​	2.配置插件

​		

```xml
<plugins>
    <!-- com.github.pagehelper为PageHelper类所在包名 -->
	    <plugin interceptor="com.github.pagehelper.PageInterceptor">
		</plugin>
	</plugins>
```

​	3.代码实现

​		

```java
@Test
	public void test3() throws IOException{
		InputStream in = Resources.getResourceAsStream("config.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
		SqlSession session = factory.openSession(true);
		EmployeeMapper mapper = session.getMapper(EmployeeMapper.class);
		//pageNum:页数 pageSize：每页显示的条数
		PageHelper.startPage(5, 10);//第5页 查10条
		List<Employee> list = mapper.selectByExample(null);
		//将查询的结果使用对象封装起来
		PageInfo<Employee> info = new PageInfo<Employee> (list);
		System.out.println("当前页数"+info.getPageNum());
		System.out.println("上页码"+info.getPrePage());
		System.out.println("下页码"+info.getNextPage());
		System.out.println("总页码"+info.getPages());
		System.out.println("总记录数"+info.getTotal());
		for(Employee e : info.getList()){
			System.out.println(e);
		}
		session.close();
	}
```

## SM整合

​	1.添加依赖文件

​		

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.alex</groupId>
  <artifactId>spring_mybatis</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  <dependencies>
  	<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
	<dependency>
	    <groupId>org.mybatis</groupId>
	    <artifactId>mybatis-spring</artifactId>
	    <version>2.0.2</version>
	</dependency>
	<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
	<dependency>
	    <groupId>org.mybatis</groupId>
	    <artifactId>mybatis</artifactId>
	    <version>3.5.2</version>
	</dependency>
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context</artifactId>
	    <version>5.0.8.RELEASE</version>
	</dependency>
	<dependency>
	    <groupId>mysql</groupId>
	    <artifactId>mysql-connector-java</artifactId>
	    <version>5.1.27</version>
	</dependency>
	  	<!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
	<dependency>
	    <groupId>com.mchange</groupId>
	    <artifactId>c3p0</artifactId>
	    <version>0.9.5.4</version>
	</dependency>
	<dependency>
		<groupId>log4j</groupId>
		<artifactId>log4j</artifactId>
		<version>1.2.17</version>
	</dependency>
	<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-jdbc</artifactId>
	    <version>5.0.8.RELEASE</version>
	</dependency>
  
  </dependencies>
</project>
```

2.写spring_config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
          http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        ">
        <!-- 引入jdbc.properties -->
        <context:property-placeholder location="classpath:jdbc.properties"/>
        <!-- 配置数据库连接信息，替代mybatis的配置文件config.xml -->
        <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        	<!-- 配置连接信息 -->
        	<property name="driverClass" value="${jdbc.driverClass}"></property>
        	<property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>
        	<property name="user" value="${jdbc.user}"></property>
        	<property name="password" value="${jdbc.password}"></property>
        </bean>
        <!-- 在spring容器中创建mybatis的核心对象SqlSessionFactroy对象 -->
        <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 给sqlsessionfactory配置连接信息，这是必须属性 -->
        	<property name="dataSource" ref="dataSource"></property>
        	<!-- 加载mybatis的配置文件 -->
<!--         	<property name="configLocation" value="classpath:config.xml"></property>
 -->        	
        	<!-- 加载mapper.xml文件路径 -->
        	<property name="mapperLocations" value="com/tanzhou/mapper/*.xml"></property>
        </bean>
        <!-- Spring产生mybatis最终需要的动态mapper对象，EmployeeMapper
        	1.在dao接口加上实现类，继承SqlSessionDaoSupport,获取sqlsession对象，
        	2.不需要自己写dao实现类，框架生成dao实现类
        	3.基于第二种方式，如果有多个dao。那么就要配置多个bean，太麻烦，所以直接全部配置好
         -->
         <!-- MapperScannerConfigurer：批量生产多个实现类对象 -->
         <!-- 批量生产dao对象在spring容器中id默认值为接口名首字母小写 -->
         <bean id="mappers" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
         	<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
         	<property name="basePackage" value="com.tanzhou.dao"></property>
         </bean>
         
       <!--   <bean id="employeeDao" class="org.mybatis.spring.mapper.MapperFactoryBean">
         通过MapperFactoryBean创建出来value所对应的接口实现类对象
         	<property name="mapperInterface" value="com.tanzhou.dao.EmployeeDao"></property>
         	<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
         </bean> -->
         
        <!--  <bean id="employeeDao" class="com.tanzhou.dao.impl.EmployeeDaoImpl">
         dao层访问数据库需要 sqlSessionFactory对象，所以需要进行关联
         	<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
         </bean> -->
         
         <bean id="employeeService" class="com.tanzhou.service.impl.EmployeeServiceImpl">
         	<property name="dao" ref="employeeDao"></property>
         </bean>
</beans>
```

