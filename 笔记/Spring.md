# 								SPRING

## Author：Alex

## 前言

框架：开发中的一套解决方案

​       S:Spring

​       S:SpringMVC

​       M:Mybatis

​       框架就是半成品

​              封装了许多的细节，开发者可以使用简单的方式去实现功能，大大的提高开发效率

​       三层架构：

​              视图层：展示数据-------》SpringMVC

​              业务层：用来处理业务数据，需求

​              持久层：和数据库交互的----》mybatis

​              Spring不属于任何一层



Maven：项目的管理工具，是项目对象模型，构架项目，依赖管理

​              Pom：project object model 工程对象模型

\1.      设置本地仓库路径（储存jar包）

​                                                  

```xml
<!-- localRepository

   | The path to the local repository maven will use to store artifacts.

   |

   | Default: ${user.home}/.m2/repository

  <localRepository>/path/to/local/repo</localRepository>

  -->

  <localRepository>D:\repository</localRepository>
```

\2.      设置镜像仓库（下载速度更快，节约时间）

 

```xml
  <mirror>

      <id>mirrorId</id>

     <mirrorOf>repositoryId</mirrorOf>

      <name>Human Readable Name for this Mirror.</name>

      <url>http://my.repository.com/repo/path</url>

    </mirror>

    -->

         <id>alimaven</id>

      <mirrorOf>central</mirrorOf>

     <name>aliyun maven</name>

     <url>http://maven.aliyun.com/nexus/content/groups/public/</url>

  </mirrors>
```

  

\3.      配置

```xml
  <id>jdk-1.8</id>

         <activation>

              <activeByDefault>true</activeByDefault>

              <jdk>1.8</jdk>

         </activation>

         <properties>

              <maven.compiler.source>1.8</maven.compiler.source>

              <maven.compiler.target>1.8</maven.compiler.target>

              <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>

         </properties>

  </profiles>
```

创建maven：

​              GroupID 组织名称 域名反写

使用spring：

\1.      需要在pom.xml中映入spring—context

\2.      在resource下创建xml 文件需要引入spring提供的约束

\3.      把对象创建交个spring进行管理，通过配置bean标签，需要向指定ID代表对象名字，class，代表对象的全类名

如何取出对象：

\1.      获取spring的IOC容器，并根据ID获取对象

\2.      文件地址：从类路径下去加载

什么时候用立即加载

​       在单例对象中使用

什么时候用延时加载

​       在多例对象中使用

Spring：

​       优势：

\1.      方便解耦，简化开发

\2.      AOP面向切面编程

\3.      声明式事务支持

\4.      方便程序测试

\5.      方便集成各种其他优秀个框架

​       Ejb：java企业级开发的一个标准

iOc:控制反转，把自己的权利给了别人，降低了程序之间依赖关系（解耦）不能解决增删改查

​       AOp：面向切面编程

耦合：程序之间的依赖关系（类与类之间的依赖关系）

解耦：降低程序之间的依赖关系

在实际开发中：

​       编译时期不依赖，运行时期才依赖

解耦的思路：使用反射创建对象，而避免直接使用new关键字。

​                            通过读取配置文件来获取要创建的对象的全类名。

Bean：可重用组件

Javabean：用java写的可重用组件

 

\1.      通过配置文件来配置service和dao

a)        配置文件内容：全类名和唯一标识   key===value

b)        配置文件的方式

Xml

Properties

​       Resource目录下创建properties文件

\2.      通过读取文件的配置内容，反射创建对象

a)        定义properties对象

 

局部变量跟随对象的出现而出现

 

单例：在多线程运行环境下，有线程安全问题，效率要高

多例：在多线程运行环境下，没有线程安全问题，但是每一个线程一来就需要创建对象，效率不高

 

Ctrl + 1 自动补齐返回类型

选中然后Ctrl+T就会出现全部实现类

Ctrl+shift+T搜索所有的类

## Spring-IOC 

\1.      导入依赖 《spring-context》

\2.      创建spring配置文件，

### 创建bean的三种方式

#### 1.1  使用默认的构造方法创建

使用bean标签，添加id和class，如果对应的class中没有提供默认构造方法，则无法创建对象

```xml
<bean id="userDao" class="com.tanzhou.dao.impl.UserDaoImpl"></bean>  
```

####  1.2使用类中的方法创建对象并且存入spring容器中

```xml
<bean id="userService3"  factory-method="getInstanceDemo" factory-bean="instanceDemo"></bean>

   <bean id="instanceDemo"  class="com.tanzhou.factory.InstanceDemo"></bean>
```

####     1.3使用类中的静态方法创建对象并且存入spring容器中

```xml
<bean id="userService4" class="com.tanzhou.factory.InstanceDemo" factory-method="getStaticInstanceDemo"></bean>
```

   

### Bean对象的作用范围

默认为的单例对象，

​       Scope：用于指定bean的作用范围

​              取值：singleton：单例

​                     Prototype：多例

​                     Request：web应用的请求范围

​                     Session：web应用的session范围

​                     Global-session：集群环境的session范围

 

### Bean对象的生命周期

单例

a.      创建：当容器创建时，对象出身

b.      使用：当容器在，对象一直在init方法

c.       销毁：容器销毁，对象死亡destory方法

多例

a.      创建：对象获取的时候，才会创建

b.      使用：只要对象在使用过程中就会一直活着

c.       销毁：当对象如果长时间不使用，且没有引用到这个对象的时候，java就会调用垃圾回收机制自动回收

总结：spring不会帮我们销毁多例对象

### ApplicationContext常用实现类：

​		ClassPathXmlApplicationContext：(最常用)加载类路径下的配置文件，如果		文	件不再，则加载不了

​		FileSystemXmlApplicationContext：加载磁盘任意路径下的配置文件，此文		件一定要有访问权限

​		AnnotationConfigApplicationContext：加载注解类直接写入文件的字节码对		象

```java
ApplicationContext ioc = new AnnotationConfigApplicationContext(SpringConfig.class);
```

FactoryBean和ApplicationContext的区别？

​     	  1.ApplicationContext：在创建核心容器时，创建对象采取的方式是立即加载的方式，只要读取完配文件，马上创建配置文件中所有的对象

   2. FactoryBean：在创建核心容器的时，创建对象采取的方式是延迟加载的方式，什么时候根据ID去获取对象了，才真正得去创建对象

      

### Spring依赖注入（DI）：

​       IOC：降低程序之间以依赖关系（依赖关系交给了spring）

​       依赖关系：在当前类中使用到了其他类的对象，由spring管理，在配置文件中进行配置

依赖关系的维护：依赖注入

​       依赖注入：能够注入的数据

\1.      基本类型和String

\2.      其他的bean类型

\3.      复杂类型（集合，数组）

#### 注入方式：

##### \1.      使用构造方法注入

使用的标签：constructor-arg标签

出现的位置：在bean内部 

注解：alt+ /

​       Type：用于指定要注入的数据类型（全类名），如果类型有相同所以区分不了给那个参数

​       Index：用于指定要注入的数据给构造方法中指定下标的参数进行赋值，从0开始。有可能类型不匹配

​       Name：用于指定给构造方法中指定的名字的参数进行赋值

​       Value：用于设置值

​       Ref：引用其他对象（在spring容器中出现的bean对象）

优点：在获取bean对象时，注入的数据是必须的操作，否则对象无法创建。如果这个类在创建的时候就初始化，南无就很好用

弊端：改变了bean对象的实例化方式，在创建对象的时候，如果没有用到数据，也必须要写入数据

```xml
<bean id="sim" class="java.text.SimpleDateFormat">
			<constructor-arg name="pattern" value="yyyy-MM-dd"></constructor-arg>
</bean>
<bean id="date" class="java.util.Date" factory-bean="sim" factory-method="parse">
		<constructor-arg value="1998-09-27"></constructor-arg>
</bean> 

<bean id="user" class="com.Alex.pojo.User">
		<constructor-arg type="java.lang.String" value="小王"></constructor-arg>
		<constructor-arg index="1" value="18"></constructor-arg>
		<constructor-arg name="birthday" ref="date"></constructor-arg>
</bean>
```

##### \2.      使用set方法注入

使用的标签：Property标签

出现的位置：bean内部

对应的属性：

​       Name：用于指定注入时所调用set方法名称

​              不是去看成员变量的名字，而是去看set方法的名字，假设方法名字为setUsername,注入的时候指定name=username

​       Ref：引用其他对象（在spring容器中出现的bean对象）

​       Value：用于提供基本类型和String类型

特点：没有明确的限制，，可以直接使用默认构造方法

弊端：如果这个对象某个成员必须要有值，那么set无法保正一定注入。

```xml
<!-- 日期的bean S-->
 <bean id=*"sim"* class=*"java.text.SimpleDateFormat"*>
        <constructor-arg name=*"pattern"* value=*"yyyy-MM-dd"*></constructor-arg>
 </bean>
 <bean id=*"date"* class=*"java.util.Date"* factory-bean=*"sim"* factory-method=*"parse"*>
     <constructor-arg value=*"1998-09-27"*></constructor-arg>
</bean>
 <!-- 日期的bean E-->
<bean id="user2" class="com.Alex.pojo.User">
			<property name="name" value="小马"></property>
			<property name="age" value="11"></property>
			<property name="birthday" ref="date"></property>
</bean>

```

 

##### \3.      使用p名称注入（先引入名称空间）

```xml
xmlns:p="http://www.springframework.org/schema/p"

<bean id="user3"  class="com.Alex.pojo.User"    p:name="小王" p:age="11" p:birthday-ref="date">
</bean>
```

​              复杂类型注入

```xml
			  <!-- 复杂类型注入
			private String[] strs;
			private List<String> list;
			private Set<User> set;
			private Map<String,String> map;
			private Properties pr;
		-->
		<bean id="userService" class="com.Alex.service.UserService">
			<property name="strs">
				<array>
					<value>abc</value>
					<value>abc</value>
				</array>
			</property>
			<property name="list">
				<list>
					<value>你好</value>
					<value>美好</value>
				</list>
			</property>
			<property name="set">
				<!-- Set<User> set = new HashSet<User>() ;
						User user = new User();
						user.setName = "coco"；
						user.setAge =  11;
						set.add(user);
				-->

			<set>
				<bean class="com.Alex.pojo.User">
					<property name="name" value="小马">						</property>
					<property name="age" value="11">							</property>
					<property name="birthday" ref="date">						</property>
				</bean>
				<ref bean="user3"/>
			</set>
		</property>
		
		<property name="map">
			<map>
				<entry key="one" value="1"></entry>
				<entry key="two" value="2"></entry>
			</map>
		</property>
		
		<property name="pr">
			<props>
				<prop key="url">mysql</prop>
				<prop key="uri">com.mysql</prop>
			</props>
		</property>
	</bean>
```

##### \4.      使用注解注入

目前注解主要是替换XML配置

\1.      用于创建对象：和配置文件中bean标签实现的效果是一样的

```xml
<bean id="userService" class="com.Alex.service.impl.UserServiceImpl"></bean>
```

###### @Component：

用于把当前类对象存入到spring容器中，如果没有指定bean的ID（就是value值），默认为当前类名，并且首字母小写，当前类对象存入spring中，用于不属于三层中的任何一层 

```java
@Component(value="userService")
@Component("userService")
@Component
```

告诉spring在创建容器时要扫描的包，需要引入context名称空间和约束

自动组件扫描： base-package：指定要扫描的包，会把当前指定的包以及下面的所有子包中加上了
    注解的类自动扫描到spring容器中

```xml
 <context:component-scan base-package="填写包名"></context:component-scan>
```

```xml 
context:component-scan的子标签：一次只能用一个

指定扫描包要包含的类
<context:exclude-filter type="annotation" expression=""/>
指定扫描包时不包含的类
<context:include-filter type="annotation" expression=""/>

type="annotation":指定排除规则，通过expression属性指定的类带上的注解Controller不要扫描 
<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>

type="assignable":指定排除某一个具体的类，按照类进行排除    	  
<context:exclude-filter type="assignable" expression="com.tanzhou.service.impl.UserServiceImpl"/>
```

三层：使三层架构对象更加清晰，

以下三个注解的使用和作用和@Component一样

@Controller：表现层

@Service：业务层

@Repository：持久层

总结：这四个放在类上

\2.      用于注入数据：和配置文件中bean标签内部写一个property标签实现的效果一样

###### @Autowired：

自动按照类型注入，只要容器中有唯一的bean对象类型和要注入的变量类型一致，就可以注入成功，反之注入失败（如果bean类型不唯一的时候就会依托于@Qualifier去实现）

​		书写位置：成员变量

​			如果使用注解方式配置，成员变量的set方法不再是必须得

如果IOC容器中有多个类型匹配时，首先先按照类型进行匹配，那么如果出现多个再接着使用成员变量名作为bean的ID去匹配多个对象中的key如果有一样的则可以注入成功

在注入成员时如果出现了相同类型的多个对象则使用成员变量名去

###### @Qualifier：

  	作用：再按照类中注入的基础上再按照名称注入，他在成员变量注入时，不能单独使用
  	
  	属性：value：用于指定bean的id

###### @Resource:

  	作用：直接按照bean的id注入，可以单独使用，一般是对象类型

​	  属性：name ：指定bean的ID。      复杂类型数据就用它

###### @Value：

​	成员变量属性注入值

  	作用：用于注入基本数据类型和String数据类型

​       属性：value：用于指定数据的值，可以直接给定值

​		写法：${表达式}

总结：这四个放在成员变量上

\3.      用于改变对象的作用范围：和配文件中bean标签上的scope属性实现的效果是一致的

###### @Scope：

​		在类上面去加的

​		作用：用于指定bean的作用范围

​		属性：value：用于指定范围，常用的取值：singleton，prototype，默认单例

\4.      和对象的生命周期有关，作用是和bean标签上面init-method destory-method属性实现的效果一致，书写在方法上

###### @PreDestory：

​			用于指定销毁方法

###### @PostConstruct：

​			用于指定初始化方法

##### \5.     整合Spring-ioc

​		1.xml配置：用啥配啥

```xml
 <context:property-placeholder location="classpath:jdbc.properties"/>
    
    <!-- 配置userservice对象 -->
    <bean id="userService" class="com.Alex.service.impl.UserServiceImpl">
    	<property name="userDao" ref="userDao"></property>
    </bean>
    <!-- 配置userdao -->
    <bean id="userDao" class="com.Alex.dao.impl.UserDaoImpl">
    	<property name="qr" ref="qr"></property>
    </bean>
    <!-- 配置QueryRunner -->
    <bean id="qr" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
    	<constructor-arg name="ds" ref="dataSource"></constructor-arg>
    </bean>
    <!-- 配置DataSource -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    	<property name="driverClass" value="${jdbc.driver}"></property>
   		<property name="jdbcUrl" value="${jdbc.url}"></property>
    	<property name="user" value="${jdbc.user}"></property>
    	<property name="password" value="${jdbc.password}"></property>
    </bean>
```
​		2.引入外部属性文件
​			location:指定配置文件的位置
​			classpath：表示类路径下的资源

​		3.配置数据库的连接信息
​			使用${}代表动态取出配置文件中某个key对应的值

​		4.配置类

```java
package com.Alex.config;

import javax.sql.DataSource;

import org.apache.commons.dbutils.QueryRunner;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.annotation.Scope;

import com.mchange.v2.c3p0.ComboPooledDataSource;

/**
 * 该类是一个配置类
 * @author Alex
 *
 */
@Configuration//指定当前类是一个配置类
@ComponentScan(basePackages="com.Alex")//用于指定spring在创建容器时要扫描的包，两个属性value和basePackages作用一样

@PropertySource(value="classpath:jdbc.properties")//用于指定properties文件的位置 ，classpath: 表示类路径下
public class SpringConfig {
	
	@Value("${jdbc.driver}")
	private String driverClass;
	@Value("${jdbc.url}")
	private String jdbcUrl;
	@Value("${jdbc.user}")
	private String user;
	@Value("${jdbc.password}")
	private String password;
	
	
	@Bean(name="queryRunner")//把当前方法的返回值作为bean对象存入spring容器中 ，属性：name:用于指定bean的id，当不指定时，id为当前的方法名
	@Scope("prototype")
	public QueryRunner CreatQr(DataSource ds) {
		return new QueryRunner(ds);
	}
	
	@Bean(name="dataSource")
	public DataSource createDs() {
		//ComboPooledDataSource
		//创建数据库连接池对象
		ComboPooledDataSource ds = new ComboPooledDataSource();
		try {
			ds.setDriverClass(driverClass);
			ds.setJdbcUrl(jdbcUrl);
			ds.setUser(user);
			ds.setPassword(password);
			return ds;
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException();
		}
		
	}
	
	
	
}

```

##### \6.整合spring-test

​	1.pom.xml文件中配置spring-test的依赖

​	2.使用Junit提供的注解，把原有的没有自动创建spring容器对象的注解给替换成·@RunWith

​	3.使用spring的注解要告诉spring，配置文件或者配置注解类所在的位置，@ContextConfiguration

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"classpath:spring_config.xml", "classpath:spring_mvc_config.xml"})

```



## 动态代理：

​		作用：在不改变源码的基础上，对已有的方法进行增强（aop思想的实现技术）

### 	1.基于接口的动态代理

​			要求：被代理类最少实现一个接口

​			提供者：jdk

​			涉及的类：Proxy

```java
//方法1: 该方法用于获取指定动态代理对象所关联的调用处理器
static InvocationHandler getInvocationHandler(Object proxy)
 
//方法2：该方法用于获取关联于指定类装载器和一组接口的动态代理对象
static Class getProxyClass(ClassLoader loader, Class[] interfaces)
 
//方法3：该方法用于判断指定类对象是否是一个动态代理类
static boolean isProxyClass(Class cl)
 
//方法4：该方法用于为指定类装载器、一组接口及调用处理器生成动态代理对象
static Object newProxyInstance(ClassLoader loader, Class[] interfaces, InvocationHandler h)
```

​		创建代理对象：new ProxyInstance
​	 			(ClassLoader loader，Class<?>[] interfaces，InvocationHandler h)
​	 			ClassLoader:类加载器，和被代理对象使用相同的类加载器
​	 			Class<?>[]:字节码对象数组，是代理类实现的接口
​	 			（要求代理对象和被代理对象具有相同的方法）
​	 			InvocationHandler:接口：用于我们提供增强代码，一般都是该接口的实					现类（匿名）
​	 			策略模式：达成目标的过程就是策略

​			执行被代理对象的任何方法都会先经过该方法，相当于这个方法有拦截功能
​				 参数：
​				 proxy：代理对象的引用
​				 Method：当前执行的方法反射出来的方法对象
​				 Object[] args：当前执行方法所需要的参数
​				
​				返回值：
​					Object：代表当前执行方法的返回值

```java
Object invoke(Object proxy, Method method, Object[] args)
```

```java
public static void main(String[] args) {
//		//找演员
		Actor actor = new Actor();
		actor.setName("蔡徐坤");
//		actor.basicAct(10000);
//		actor.dangerAct(20000);
		/*
		 	动态代理：
		 		作用：不改变源码的基础上，对已有的方法进行增强（aop思想的实现技术）
		 	1.基于接口的动态代理
		 		要求:被代理类最少实现一个接口
		 		提供者：jdk
		 		涉及的类：Proxy
		 		创建代理对象：newProxyInstance
		 			(ClassLoader loader，Class<?>[] interfaces，InvocationHandler h)
		 			ClassLoader:类加载器，和被代理对象使用相同的类加载器
		 			Class<?>[]:字节码对象数组，是代理类实现的接口
		 			（要求代理对象和被代理对象具有相同的方法）
		 			InvocationHandler:接口：用于我们提供增强代码，一般都是该接口的实现类（匿名）
		 			策略模式：达成目标的过程就是策略
		 */
	
		IActor proxyIActor = (IActor)Proxy.newProxyInstance(actor.getClass().getClassLoader(), 
				actor.getClass().getInterfaces(), new InvocationHandler(){
					/*执行被代理对象的任何方法都会先经过该方法，相当于这个方法有拦截功能，拦截调度器
					 参数：
					 proxy：代理对象的引用
					 Method：当前执行的方法反射出来的方法对象
					 Object[] args：当前执行方法所需要的参数
					  
					返回值：
						Object：代表当前执行方法的返回值
					  */
					@Override
					public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
						//经纪人在给明星接业务的时候，要看对方的价格够不够
						//如果是基本的演出，需要的价格是五万
						//如果是危险的演出，需要的价格是九万
						Object obj = null;
						//取出执行方法中的参数：给的money
						Integer money = (Integer)args[0];
						//判断当前执行的是什么方法
						if("basicAct".equals(method.getName())){
							//基本演出
							if(money>50000){
								//才会接业务，开始基本表演
								obj = method.invoke(actor, money/2);
							}
						}
						if("dangerAct".equals(method.getName())){
							//危险演出
							if(money>90000){
								//才会接业务。开始危险表演
								obj = method.invoke(actor, money/2);
							}
						}
						return obj;
					}
		});
		proxyIActor.basicAct(100000);
		proxyIActor.dangerAct(50000);
	}
```

- 指数：⭐⭐
- 场景：中间件开发、设计模式中代理模式和装饰器模式应用
- 点评：这种JDK自带的类代理方式是非常常用的一种，也是非常简单的一种。基本会在一些中间件代码里看到例如：数据库路由组件、Redis组件等，同时我们也可以使用这样的方式应用到设计模式中。

### 2.基于子类的动态代理

​		要求：被代理的类不能够是最终类

​		提供者：带三方；cglib

​		涉及的类：Enhancer

​		创建代理对象的方法：create（）；

​		参数：

​			create(Class, Callback);

​			Class：被代理对象的字节马文件对象

​			Callback:如何代理，一个接口，使用子接口：MethodInterceptor

​				Object proxy:代理对象的引用
​				Method method：当前执行的方法反射出来的方法对象
​				Object[] arg2：当前执行方法所需要的参数
​				MethodProxy moethodPrxoy：当前执行方法的代理对象

```java
Object intercept(Object proxy, Method method, Object[] args, MethodProxy moethodPrxoy)
```

```java
public static void main(String[] args) {
//		//找演员
		Actor actor = new Actor();
		actor.setName("蔡徐坤");

		/*
		 	2基于子类的动态代理：
		 		要求：被代理类不能够是最终类
		 		提供者：第三方：cglib
		 		涉及的类：Enhancer
		 		创建代理对象的方法：create();
		 		参数：
		 			create(Class,Callback);
		 			Class:被代理对象的字节码文件对象，
		 			Callback：如何代理，一个接口，使用的子接口：MethodInterceptor
		 */
		Actor cglig = (Actor)Enhancer.create(actor.getClass(), new MethodInterceptor(){
			//执行被代理对象的任何方法，都会先经过这个方法，和proxy的invoke方法一样	
			/*
				Object proxy:代理对象的引用
				Method method：当前执行的方法反射出来的方法对象
				Object[] arg2：当前执行方法所需要的参数
				MethodProxy moethodPrxoy：当前执行方法的代理对象
			 */
			@Override
			public Object intercept(Object proxy, Method method, Object[] arg2, MethodProxy moethodPrxoy) throws Throwable {
				Object obj = null;
				//如何对已有的方法来进行增强
				//签约公司在给明星接业务的时候，需要看对方给的钱够不够
				//如果是基本的演出，需要的演出费50000
				//如果是危险演出，需要的演出费90000
				Integer money = (Integer)arg2[0];//91000
				if("basicAct".equals(method.getName())){
					//基本演出
					if(money>50000){
						//执行方法
						obj = method.invoke(actor, money/2);
					}
				}else if("dangerAct".equals(method.getName())){
					//危险演出
					if(money>90000){
						//执行方法
						obj = method.invoke(actor, money/2);
					}
				}
				return obj;
			}
		});
		cglig.basicAct(60000);
		cglig.dangerAct(91000);
	}
```

- 指数：⭐⭐⭐
- 场景：Spring、AOP切面、鉴权服务、中间件开发、RPC框架等
- 点评：CGLIB不同于JDK，它的底层使用ASM字节码框架在类中修改指令码实现代理，所以这种代理方式也就不需要像JDK那样需要接口才能代理。同时得益于字节码框架的使用，所以这种代理方式也会比使用JDK代理的方式快1.5~2.0倍。

### 3.ASM代理

~~~java
public class ASMProxy extends ClassLoader {

    public static <T> T getProxy(Class clazz) throws Exception {

        ClassReader classReader = new ClassReader(clazz.getName());
        ClassWriter classWriter = new ClassWriter(classReader, ClassWriter.COMPUTE_MAXS);

        classReader.accept(new ClassVisitor(ASM5, classWriter) {
            @Override
            public MethodVisitor visitMethod(int access, final String name, String descriptor, String signature, String[] exceptions) {

                // 方法过滤
                if (!"queryUserInfo".equals(name))
                    return super.visitMethod(access, name, descriptor, signature, exceptions);

                final MethodVisitor methodVisitor = super.visitMethod(access, name, descriptor, signature, exceptions);

                return new AdviceAdapter(ASM5, methodVisitor, access, name, descriptor) {

                    @Override
                    protected void onMethodEnter() {
                        // 执行指令；获取静态属性
                        methodVisitor.visitFieldInsn(Opcodes.GETSTATIC, "java/lang/System", "out", "Ljava/io/PrintStream;");
                        // 加载常量 load constant
                        methodVisitor.visitLdcInsn(name + " 你被代理了，By ASM！");
                        // 调用方法
                        methodVisitor.visitMethodInsn(Opcodes.INVOKEVIRTUAL, "java/io/PrintStream", "println", "(Ljava/lang/String;)V", false);
                        super.onMethodEnter();
                    }
                };
            }
        }, ClassReader.EXPAND_FRAMES);

        byte[] bytes = classWriter.toByteArray();

        return (T) new ASMProxy().defineClass(clazz.getName(), bytes, 0, bytes.length).newInstance();
    }

}

@Test
public void test_ASMProxy() throws Exception {
    IUserApi userApi = ASMProxy.getProxy(UserApi.class);
    String invoke = userApi.queryUserInfo();
    logger.info("测试结果：{}", invoke);
}

/**
 * 测试结果：
 * 
 * queryUserInfo 你被代理了，By ASM！
 * Process finished with exit code 0
 */

~~~

- 指数：⭐⭐⭐⭐⭐
- 场景：全链路监控、破解工具包、CGLIB、Spring获取类元数据等
- 点评：这种代理就是使用字节码编程的方式进行处理，它的实现方式相对复杂，而且需要了解Java虚拟机规范相关的知识。因为你的每一步代理操作，都是在操作字节码指令，例如：`Opcodes.GETSTATIC`、`Opcodes.INVOKEVIRTUAL`，除了这些还有小200个常用的指令。但这种最接近底层的方式，也是最快的方式。所以在一些使用字节码插装的全链路监控中，会非常常见。

### 4.javassis代理

~~~java
public class JavassistProxy extends ClassLoader {

    public static <T> T getProxy(Class clazz) throws Exception {

        ClassPool pool = ClassPool.getDefault();
        // 获取类
        CtClass ctClass = pool.get(clazz.getName());
        // 获取方法
        CtMethod ctMethod = ctClass.getDeclaredMethod("queryUserInfo");
        // 方法前加强
        ctMethod.insertBefore("{System.out.println(\"" + ctMethod.getName() + " 你被代理了，By Javassist\");}");

        byte[] bytes = ctClass.toBytecode();

        return (T) new JavassistProxy().defineClass(clazz.getName(), bytes, 0, bytes.length).newInstance();
    }

}

@Test
public void test_JavassistProxy() throws Exception {
    IUserApi userApi = JavassistProxy.getProxy(UserApi.class)
    String invoke = userApi.queryUserInfo();
    logger.info("测试结果：{}", invoke);
}

/**
 * 测试结果：
 * 
 * queryUserInfo 你被代理了，By Javassist
 * Process finished with exit code 0
 */

~~~

- 指数：⭐⭐⭐⭐
- 场景：全链路监控、类代理、AOP
- 点评：`Javassist`  是一个使用非常广的字节码插装框架，几乎一大部分非入侵的全链路监控都是会选择使用这个框架。因为它不想ASM那样操作字节码导致风险，同时它的功能也非常齐全。另外，这个框架即可使用它所提供的方式直接编写插装代码，也可以使用字节码指令进行控制生成代码，所以综合来看也是一个非常不错的字节码框架。

### 5.byte-buddy代理

~~~java
public class ByteBuddyProxy {

    public static <T> T getProxy(Class clazz) throws Exception {

        DynamicType.Unloaded<?> dynamicType = new ByteBuddy()
                .subclass(clazz)
                .method(ElementMatchers.<MethodDescription>named("queryUserInfo"))
                .intercept(MethodDelegation.to(InvocationHandler.class))
                .make();

        return (T) dynamicType.load(Thread.currentThread().getContextClassLoader()).getLoaded().newInstance();
    }

}

@RuntimeType
public static Object intercept(@Origin Method method, @AllArguments Object[] args, @SuperCall Callable<?> callable) throws Exception {
    System.out.println(method.getName() + " 你被代理了，By Byte-Buddy！");
    return callable.call();
}

@Test
public void test_ByteBuddyProxy() throws Exception {
    IUserApi userApi = ByteBuddyProxy.getProxy(UserApi.class);
    String invoke = userApi.queryUserInfo();
    logger.info("测试结果：{}", invoke);
}

/**
 * 测试结果：
 * 
 * queryUserInfo 你被代理了，By Byte-Buddy！
 * Process finished with exit code 0
 */

~~~

- 指数：⭐⭐⭐⭐
- 场景：AOP切面、类代理、组件、监控、日志
- 点评：`Byte Buddy` 也是一个字节码操作的类库，但 `Byte Buddy` 的使用方式更加简单。无需理解字节码指令，即可使用简单的 API 就能很容易操作字节码，控制类和方法。比起JDK动态代理、cglib，Byte Buddy在性能上具有一定的优势。**另外**，2015年10月，Byte Buddy被 Oracle 授予了 Duke’s Choice大奖。该奖项对Byte Buddy的“ Java技术方面的巨大创新 ”表示赞赏。

### 总结

![image-20201222234947367](imgs/image-20201222234947367.png)



## Spring-AOP

​		Aspect oriented Programming:面向切面编程

​			作用：在程序运行期间，不修改源码对已有的方法进行增强

​			优势：减少重复代码，提高开发效率，维护方便

​			aop实现方式：动态代理技术

### AOP专业术语：

​			连接点：代表那些被拦截到的点（方法）

​			切入点：代表是要对那些连接点进行拦截（没有增强的方法就不属于切入        							点）

​			通知：代表拦截道连接点之后所要做的事情

​				通知分类型：

​						以执行方法obj = method.invoke(userService, args);作为中心
​                        前置通知：在method.invoke(userService, args);执行之									前的
​                        后置通知：method.invoke(userService, args);执行之后									的
​                        异常通知：在catch代码中执行的方法
​                        最终通知：在finlly代码中执行的方法
​                        环绕通知：代表整个方法
​            目标对象：被代理对象（userService）
​            织入：加入事务支持的过程
​            代理：代理对象（proxyService）
​            切面：切入点和通知的结合

​	用spring-aop通过配置方式实现刚刚所做的功能

### 		配置方式：

#### 			xml:

​				1.把通知bean交给spring进行管理
​    			2.使用aop:config标签表示开始aop的配置
   			 3.使用aop：config的子标签aop:aspect表示配置切面

​				 	id：给切面提供一个唯一的标识
​     				ref：给指定通知类的bean 的id

  			  4.在aop:aspect内使用对应标签来配置通知的类型 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
        
        <!-- 配置spring ioc，把service对象配置进来 -->
        <bean id="userService" class="com.tanzhou.service.impl.UserServiceImpl"></bean>
        
        <!-- spring基于xml的aop配置步骤
        1.把通知bean交给spring进行管理
        2.使用aop:config标签表示开始aop的配置
        3.使用aop：config的子标签aop:aspect表示配置切面
        4.在aop:aspect内使用对应标签来配置通知的类型 
         -->
         <!-- 1.把通知bean交给spring进行管理 -->
         <bean id="logUtils" class="com.tanzhou.utils.LogUtils"></bean>
         
         <!-- 2.使用aop:config标签表示开始aop的配置 -->
         <aop:config>
         	<!--   3.使用aop：config的子标签aop:aspect表示配置切面 
         	id：给切面提供一个唯一的标识
         	ref：给指定通知类的bean 的id
         	-->
         	<aop:aspect id="logAdvice" ref="logUtils">
         	<!-- 配置切入点表达式 -->
         	<aop:pointcut expression="execution(* com.tanzhou.service.impl.*.*(..))" id="pt"/>
         	<!-- 配置前置通知 method：用于指定logUtils类中哪个方法是属于前置通知
         	pointcut：用于指定切入点表达式，该表达式的含义代表对业务层service中那些方法进行增强
         	
         	写法：
         		execution(表达式)
         	表达式写法：
         		访问权限修饰符 返回值 包名.类名.方法名(参数)
         		public void com.tanzhou.service.impl.UserServiceImpl.saveUser()
         		访问权限修饰符可以省略
         		void com.tanzhou.service.impl.UserServiceImpl.saveUser()
         		返回值类型可以使用通配符，表示任意的返回值类型
         		* com.tanzhou.service.impl.UserServiceImpl.saveUser()
         		包名可以使用通配符，表示任意包，但是有几级包，就需要写几个*
         		* *.*.*.*.UserServiceImpl.saveUser()
         		包名可以使用*..表示当前包以及其子包
         		* *..UserServiceImpl.saveUser()
         		类名以及方法名可以使用*代替
         		* *..*.*()
         		参数列表可以直接写数据类型，基本数据类型直接写名称，引用数据类型写包名.类名
         		* *..*.*(com.tanzhou.domain.User)
         		全通配：
         			* *..*.*(..)
         		
         		切入点表达式：
         			* com.tanzhou.service.impl.*.*(..)
         		
         		
         		前置通知：
         			在切入点方法执行之前执行
         		后置通知：
         			在切入点方法正常执行完之后执行
         		异常通知：
         			在切入点方法执行出现异常后执行
         		最终通知：
         			无论切入点方法是否正常执行，都会执行		
         	-->
         	<!-- 配置通知的类型：并且建立通知方法和切入点方法的关联 -->
         	<!-- 	<aop:before method="logStart" pointcut-ref="pt"/>
         		配置后置通知：
         		<aop:after-returning method="logEnd" pointcut-ref="pt"/>
         		配置异常通知
         		<aop:after-throwing method="logException" pointcut-ref="pt"/>
         		配置最终通知
         		<aop:after method="logFinal"  pointcut-ref="pt"/> -->
         		
         		<!-- 配置环绕通知 -->
         		<aop:around method="logAround" pointcut-ref="pt"/>
         	</aop:aspect>
         </aop:config>
         
</beans>
```

#### 			注解：

​			通知类型：
​						  直接在方法上加上
​				      	前置通知：@Before
​					  	后置通知：@AfterReturning
​                      	异常通知：@AfterThrowing
​	                      最终通知：@After
​	                      环绕通知：@Around

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
        
      <!-- 配置spring容器时要扫描的包 -->  
        <context:component-scan base-package="com.tanzhou"></context:component-scan>
        
        <!-- 配置sprig开启注解aop的支持 -->
        <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```



``` java
package com.tanzhou.utils;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

/**
 * 用于输出日志记录，想要切入点（业务层）方法执行之前执行
 * @author xq
 *
 */
@Component("logUtils")
@Aspect //表示这个类切面类
public class LogUtils {
	/*
	  通知类型：
	  直接在方法上加上
	   	前置通知：@Before
		后置通知：@AfterReturning
		异常通知：@AfterThrowing
		最终通知：@After
		环绕通知：@Around
	 */
	@Pointcut("execution(* com.tanzhou.service.impl.*.*(..))")
	private void pt(){}
	
	
//	@Before("pt()")
	public void logStart(){
		System.out.println("前置通知：LogUtils类中logStart开始记录日志信息了");
	}
	
//	@AfterReturning("pt()")
	public void logEnd(){
		System.out.println("后置通知：LogUtils类中logEnd开始记录日志信息了");
	}
	
//	@AfterThrowing("pt()")
	public void logException(){
		System.out.println("异常通知：LogUtils类中logException开始记录日志信息了");
	}
	
//	@After("pt()")
	public void logFinal(){
		System.out.println("最终通知：LogUtils类中logFinal开始记录日志信息了");
	}
	
	/*
	 问题：当配置了环绕通知之后，切入点方法调用了却没有执行，而通知方法执行了
	 原因：
	 	通过对比之前的动态代理中的环绕通知代理，发现动态代理中的环绕通知有明确的业务层切入点方法调用
	 	现在的环绕的通知中没有，
	 解决：
	 	在spring中提供了一个接口ProceedingJoinPoint的一个proceed方法，这个方法相当于明确调用切入点方法
	 	这个接口可以作为环绕通知的方法参数，在程序执行时，spring框架为我们提供这个接口的实现类给程序使用
	 
	 */
	@Around("pt()")
	public Object logAround(ProceedingJoinPoint pjp){
		Object proceed = null;
		Object[] args = pjp.getArgs();//得到方法执行时所需要的参数
		try {
			 logStart();
			 proceed = pjp.proceed(args);
			 logEnd();
		} catch (Throwable e) {
			logException();
			throw new RuntimeException();
		}//明确调用业务层方法
		finally {
			logFinal();
		}
//		System.out.println("环绕通知：LogUtils类中logAround开始记录日志信息了");
		return proceed;
	}
}

```

## 		JDBCTemplate

Spring对数据库的操作在jdbc上面做了深层次的封装，使用spring的注入功能，可以把DataSource注册到JdbcTemplate之中。

　　JdbcTemplate位于![img](https://images2015.cnblogs.com/blog/659572/201606/659572-20160630165703077-1456883788.png)中。其全限定命名为org.springframework.jdbc.core.JdbcTemplate。要使用JdbcTemlate还需一个![img](https://images2015.cnblogs.com/blog/659572/201606/659572-20160630165907734-2109476245.png)这个包包含了一下事务和异常控制

　　

JdbcTemplate主要提供以下五类方法：

- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；

- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；

- query方法及queryForXXX方法：用于执行查询相关语句；

- call方法：用于执行存储过程、函数相关语句。

  实例

```java
package com.Alex.jdbcTemplate;

import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

import com.Alex.pojo.User;

public class jdbcTemplateDemo01 {
	public static void main(String[] args) {
//		//创建数据源
//		DriverManagerDataSource ds = new DriverManagerDataSource();
//		ds.setDriverClassName("com.mysql.jdbc.Driver");
//		ds.setUrl("jdbc:mysql://localhost:3306/test");
//		ds.setUsername("root");
//		ds.setPassword("Lwh123456");
//		//创建jdbcTemplate对象
//		JdbcTemplate jc = new JdbcTemplate();
//		String sql = "insert into user values(null,'雄',16,2000)";
//		jc.setDataSource(ds);
//		jc.execute(sql);
		ApplicationContext acc = new ClassPathXmlApplicationContext("bean2.xml");
		JdbcTemplate bean = acc.getBean("jdbcTemplate", JdbcTemplate.class);
		String sql = "select * from user where id=?";
		User user = bean.queryForObject(sql, new BeanPropertyRowMapper<User>(User.class), 3);
		System.out.println(user);
	}

}

	
```

​              

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
        <!-- 扫描jdbc.properties -->
        <context:property-placeholder location="classpath:jdbc.properties"/>
	<!-- 配置扫描包 -->
	<context:component-scan base-package="com.Alex"></context:component-scan>
	<!--配置注解开始 -->
	<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
	<!-- //创建数据源
		DriverManagerDataSource ds = new DriverManagerDataSource();
		ds.setDriverClassName("com.mysql.jdbc.Driver");
		ds.setUrl("jdbc:mysql://localhost:3306/test");
		ds.setUsername("root");
		ds.setPassword("Lwh123456");
		//创建jdbcTemplate对象
		JdbcTemplate jc = new JdbcTemplate(); 
		jc.setDataSource(ds);-->
		
		<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
			<property name="dataSource" ref="dataSource"></property>
		</bean>
		<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
			<property name="driverClassName" value="${jdbc.driver}"></property>
			<property name="url" value="${jdbc.url}"></property>
			<property name="username" value="${jdbc.user}"></property>
			<property name="password" value="${jdbc.password}"></property>
		</bean>
</beans>
```

## Spring声明式事务	

### 		xml配置法：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
         http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
    ">
        
    <!-- 配置JdbcTemplate --> 
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    	<property name="dataSource" ref="dataSource"></property>
    </bean>   
    
    <!-- 配置 dataSource-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    	<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    	<property name="url" value="jdbc:mysql:///test"></property>
    	<property name="username" value="root"></property>
    	<property name="password" value="123456"></property>
    </bean>
    
    <!-- 配置userDao -->
    <bean id="userDao" class="com.tanzhou.dao.impl.UserDaoImpl">
    	<!-- <property name="jdbcTemplate" ref="jdbcTemplate"></property> -->
    	<property name="dataSource" ref="dataSource"></property>
    </bean>
    
    <!-- 配置UserService -->
    <bean id="userService" class="com.tanzhou.service.impl.UserServiceImpl">
    	<property name="userDao" ref="userDao"></property>
    </bean>
    
    <!-- spring基于xml的声明式事务 -->
    <!--一.配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    	<property name="dataSource" ref="dataSource"></property>
    </bean>
    
    <!-- 二：配置事务通知
    	1.导入事务tx和aop的约束文件
    	2.使用tx:advice 标签配置事务通知，
    		id:给事务通知唯一的标识
    		transaction-manager：给事务通知提供一个事务管理引用
     -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
    	<!-- 五：配置事务的属性 -->
    	<tx:attributes>
    	<!-- 
    		isolation:用于指定事务的隔离级别，默认值为default，表示使用数据库的默认隔离级别 
    		no-rollback-for：用于指定一个异常，当产生该异常时，事务不回滚，产生其他异常时，则事务回滚，没有默认值
    		 	表示任何异常都回滚
    		rollback-for：用于指定一个异常，当产生该异常时，事务回滚，产生其他异常时，则事务不回滚，没有默认值。表示任何异常都回滚
    		propagation：用于指定传播行为，。默认值为REQUIRED,表示一定会有事务，一般是增删改的选择，如果是查询，
    		选择SUPPORTS
    		read-only：用于指定事务是否只读，只有查询才可以设置为true，默认为false
    		timeout：超时时间，默认-1代表永不超时
    	 -->
    		<tx:method name="*" propagation="REQUIRED" read-only="false"/>
    		<tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
    	</tx:attributes>
    </tx:advice>
    <!-- 三. 配置aop-->
    <aop:config>
    	<!-- 配置切入点表达式 -->
    	<aop:pointcut id="pt" expression="execution(* com.tanzhou.service.impl.*.*(..))" />
    	<!-- 四：建立切入点表达式和事务通知的对应关系 -->
    	<aop:advisor advice-ref="txAdvice" pointcut-ref="pt"/>
    </aop:config>
</beans>






```

### 		注解方式：

#### 		xml文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
         http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
    ">
        
    <!-- 配置JdbcTemplate --> 
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    	<property name="dataSource" ref="dataSource"></property>
    </bean>   
    
    <!-- 配置 dataSource-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    	<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    	<property name="url" value="jdbc:mysql:///test"></property>
    	<property name="username" value="root"></property>
    	<property name="password" value="123456"></property>
    </bean>
    
    <!-- 配置userDao -->
    <bean id="userDao" class="com.tanzhou.dao.impl.UserDaoImpl">
    	<!-- <property name="jdbcTemplate" ref="jdbcTemplate"></property> -->
    	<property name="dataSource" ref="dataSource"></property>
    </bean>
    
    <!-- 配置UserService -->
    <bean id="userService" class="com.tanzhou.service.impl.UserServiceImpl">
    	<property name="userDao" ref="userDao"></property>
    </bean>
    
    <!-- spring基于xml的声明式事务 -->
    <!--一.配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    	<property name="dataSource" ref="dataSource"></property>
    </bean>
    
    <!-- 二：配置事务通知
    	1.导入事务tx和aop的约束文件
    	2.使用tx:advice 标签配置事务通知，
    		id:给事务通知唯一的标识
    		transaction-manager：给事务通知提供一个事务管理引用
     -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
    	<!-- 五：配置事务的属性 -->
    	<tx:attributes>
    	<!-- 
    		isolation:用于指定事务的隔离级别，默认值为default，表示使用数据库的默认隔离级别 
    		no-rollback-for：用于指定一个异常，当产生该异常时，事务不回滚，产生其他异常时，则事务回滚，没有默认值
    		 	表示任何异常都回滚
    		rollback-for：用于指定一个异常，当产生该异常时，事务回滚，产生其他异常时，则事务不回滚，没有默认值。表示任何异常都回滚
    		propagation：用于指定传播行为，。默认值为REQUIRED,表示一定会有事务，一般是增删改的选择，如果是查询，
    		选择SUPPORTS
    		read-only：用于指定事务是否只读，只有查询才可以设置为true，默认为false
    		timeout：超时时间，默认-1代表永不超时
    	 -->
    		<tx:method name="*" propagation="REQUIRED" read-only="false"/>
    		<tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
    	</tx:attributes>
    </tx:advice>
    <!-- 三. 配置aop-->
    <aop:config>
    	<!-- 配置切入点表达式 -->
    	<aop:pointcut id="pt" expression="execution(* com.tanzhou.service.impl.*.*(..))" />
    	<!-- 四：建立切入点表达式和事务通知的对应关系 -->
    	<aop:advisor advice-ref="txAdvice" pointcut-ref="pt"/>
    </aop:config>
</beans>


```

#### java文件：

```java
@Trasactional()注解
```

