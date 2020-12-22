# SpringMVC

## 传统MVC模式介绍

![image-20200227142806221](C:\Users\mac\AppData\Roaming\Typora\typora-user-images\image-20200227142806221.png)

\1.   M：Model模型：处理封装映射数据（pojo）

\2.   V：View视图：数据页面显示（jsp）

\3.   C：Controller控制器：接收请求并决定调用哪个模型去处理请求，然后再确定用哪个视图来显示返回的数据、

\4.   优点：

a)  低耦合

b)  重用性高

## SpringMVC模式介绍

![image-20200227142829138](C:\Users\mac\AppData\Roaming\Typora\typora-user-images\image-20200227142829138.png)

![image-20200926222228909](imgs/image-20200926222228909.png)

\1.   传统的mvc项目，是一个请求对应一个Controller类（Controller一般为类），会造成存在大量的Controller类的存在，难以管理；SpringMVC使用dispatcher实现一个请求对应一个Controller方法，可以使用类名将请求分类，便于管理。

## SpringDemo

\1.   导包

 

```xml
 <dependencies>

​    <!-- SpringMVC需要依赖的jar包 -->

​    <dependency>

​      <groupId>**org.springframework**</groupId>

​      <artifactId>**spring-webmvc**</artifactId>

​      <version>**4.3.13.RELEASE**</version>

​    </dependency>

  </dependencies>
```

\2.   配置

a)  Web.xml

```xml
<!-- SpringMVC有一个前端控制器可以拦截所有请求，并且根据url派发-->

 <servlet>

 <!-- 配置DispatcherServlet前端控制器 -->

  <servlet-name>**spirngMVC**</servlet-name>

  <servlet-class>**org.springframework.web.servlet.DispatcherServlet**</servlet-class>

  <!-- 配置初始化参数，供DispatcherServlet初始化时使用 -->

  <init-param>

   <param-name>**contextConfigLocation**</param-name>

   <param-value>**classpath:SpringConfig.xml**</param-value>

  </init-param>

  <!-- 配置servlet启动加载，servlet默认是访问创建对象；

​    值越小优先级越高，越先创建对象

   -->

  <load-on-startup>**1**</load-on-startup>

 </servlet>

 <!-- 

​    /：拦截所有请求；优先级最低，低于*.jsp这些请求

​    /*：拦截所有请求；优先级高于*.jsp这些请求

  -->

 <servlet-mapping>

  <servlet-name>**spirngMVC**</servlet-name>

  <url-pattern>**/**</url-pattern>

 </servlet-mapping>
```

b)  SpringMVC.xml

```xml
<?xml version=**"1.0"** encoding=**"UTF-8"**?>

<beans xmlns=**"http://www.springframework.org/schema/beans"**

  xmlns:xsi=**"http://www.w3.org/2001/XMLSchema-instance"**

  xmlns:context=**"http://www.springframework.org/schema/context"**

  xsi:schemaLocation=**"http://www.springframework.org/schema/beans**

​    **https://www.springframework.org/schema/beans/spring-beans.xsd**

​    **http://www.springframework.org/schema/context**

​    **https://www.springframework.org/schema/context/spring-context.xsd"**>

 

  <!-- 配置包扫描，扫描类中的注解 -->

 <context:component-scan base-package=**"com.tanzhou"**></context:component-scan>

  <!-- 配置一个视图解析器，帮我们拼接页面地址 -->

  <bean class=**"org.springframework.web.servlet.view.InternalResourceViewResolver"**>

​    <property name=**"prefix"** value=**"/WEB-INF/page"**></property>

​    <property name=**"suffix"** value=**".jsp"**></property>

  </bean>

 

</beans>
```

\3.   测试

```java
/**

 \* Controller注解：标识Controller层，告诉SpringMVC这是一个处理器

 *

 */

@Controller

public class HelloSpringMVC **{**

 

  // 配置与这个方法映射的url，url缺省服务器地址和项目名称

  @RequestMapping**(**"/success"**)**

  public String success**()** **{**

​    System**.**out**.**println**(**"正在处理中"**);**

​    

​    //跳转到对应的jsp界面：jsp的准确地址

​    //视图解析器，自动拼接前缀后缀

​    **return** "/success"**;**

  **}**

**}** 
```

\4.   SpringMVC执行流程

![image-20200227163253671](C:\Users\mac\AppData\Roaming\Typora\typora-user-images\image-20200227163253671.png)

## Servlet精确匹配与模糊匹配

\1.   精确匹配

a)  填写完整的url路径，无“*”

b)  /first 与 http://localhost:8080/myshop/first

\2.   模糊匹配

a)  路径匹配

​        i.     /kata/*

​        ii.     http://localhost:8080/myshop/kata/demo.html

b)  扩展名匹配

​        i.     *.jsp

​        ii.     http://localhost:8080/myshop/kata/demo.jsp

c)   /*与/

​        i.     都是任意匹配url

​        ii.     /的优先级最低，/*为路径匹配，优先级低于精准匹配，高于扩展名匹配

\3.   使用规范

a)  必须以“/”或者“*”开头

b)  模糊匹配中，扩展名与路径无法同时设置；比如：he*.jsp

\4.   优先级

a)  作用：解决url匹配，冲突的问题。

b)  规则：精准匹配--->模糊匹配（路径匹配--->/*--->扩展名匹配--->/）

## Tomact父类web.xml

\1.   父类中默认存在的Servlet，与路径映射

a)  DefaultServlet：静态资源的处理与响应，比如浏览器访问html，图片等,除了jsp与servlet

```xml
<servlet>

​    <servlet-name>**default**</servlet-name>

​    <servlet-class>**org.apache.catalina.servlets.DefaultServlet**</servlet-class>

​    <init-param>

​      <param-name>**debug**</param-name>

​      <param-value>**0**</param-value>

​    </init-param>

​    <init-param>

​      <param-name>**listings**</param-name>

​      <param-value>**false**</param-value>

​    </init-param>

​    <load-on-startup>**1**</load-on-startup>

</servlet>

 

<servlet-mapping>

   <servlet-name>**default**</servlet-name>

   <url-pattern>**/**</url-pattern>

</servlet-mapping>
```

b)  JspServlet：jsp文件的处理与响应

 

```xml
 <servlet>

​    <servlet-name>**jsp**</servlet-name>

​    <servlet-class>**org.apache.jasper.servlet.JspServlet**</servlet-class>

​    <init-param>

​      <param-name>**fork**</param-name>

​      <param-value>**false**</param-value>

​    </init-param>

​    <init-param>

​      <param-name>**xpoweredBy**</param-name>

​      <param-value>**false**</param-value>

​    </init-param>

​    <load-on-startup>**3**</load-on-startup>

</servlet>

 

  <servlet-mapping>

​    <servlet-name>**jsp**</servlet-name>

​    <url-pattern>***.jsp**</url-pattern>

​    <url-pattern>***.jspx**</url-pattern>

  </servlet-mapping>
```

## @RequestMapping

\1.   修饰类和修饰方法

a)  修饰方法：为方法映射一个请求路径，第一个“/”可写可不写，没有的情况，SpringMVC也会默认添加

b)  修饰类：为当前类中所有方法的请求地址加上一个根目录

\2.   属性

a)  value：传入的值，就是映射的URL路径

b)  method

​        i.     规定请求方式，规定后想要请求此方法，要满足url匹配和请求方式相同

​        ii.     传入值为标识请求方式的枚举类，比如method = RequestMethod.POST；RequestMethod.GET；

c)   params

![image-20200227163715563](C:\Users\mac\AppData\Roaming\Typora\typora-user-images\image-20200227163715563.png)

​        i.     规定请求参数，对应对应servlet中的getParamete(String name)

​        ii.     params = {"username"}：必须携带key为username的参数，不管是为null还是为空

​       iii.     params = {"!username"}：必须不能携带key为username的参数

​       iv.     params = {"username=123"}：必须携带key为username的参数，并且对应的value值为123

​        v.     params = {"username!=123"}：没有key为username，有的话key不能等于123

​       vi.     params = {"username!=123","key"}：可以传入String数组，规定多个参数，满足第一个条件，还需要满足第二个条件

d)  headers 

​        i.     规定请求头参数，对应servlet中的getHeader(String name);

​        ii.     格则与params

\3.   精确匹配：一个请求对应一个方法

\4.   Ant风格（模糊匹配）

a)  通配符

​        i.     ？：一层路径中的任意一个字符（有数量固定）

​        ii.     *：一层路径中的任意多个字符（无数量规定，0也行）

​       iii.     **：多层路径，多个字符（注意：与字符连用时为一层任意多个字符，不能为多层。只有单独使用时才为多层路径，多个字符）

b)  优先级：精确--->模糊（？--->*--->**）

## Dispatcher默认配置文件地址

\1.   在web.xml文件配置文件地址

![image-20200227163743641](C:\Users\mac\AppData\Roaming\Typora\typora-user-images\image-20200227163743641.png)

\2.   默认读取地址：

a)  读取地址为：WEB-INF目录，web.xml所在的目录

b)  文件名为：servletName拼接上“-servlet.xml”

## 请求类型

\1.   OPTIONS, HEAD, GET, POST, PUT, DELETE, TRACE, CONNECT八中请求类型（get和post常用，其他因为安全问题被禁用）

\2.   get和post的区别

a)  GET是向服务器发索取数据的一种请求；而POST是向服务器提交数据的一种请求，要提交的数据位于信息头后面的实体中

b)  GET方式的数据最多只能有1024字节，而POST则没有此限制。

c)   GET方式参数会显示在地址栏上，POST放在请求实体中

d)  GET幂等，POST 不幂等

## 幂等性

\1.   概述

a)  数学中：幂等元素是指被自己重复运算(或对于函数是为复合)的结果等于它自己的元素。例如，乘法下仅有两个幂等实数，为0和1

b)  http中：HTTP中方法的幂等性是指一次和多次请求某一个资源应该具有同样的副作用

\2.   GET方法

a)  只是查询数据，不会产生副作用，所以GET方法是幂等的；

b)  例子：通过URL查询id为1员工的数据，获取当前服务器时间

\3.   POST请求

a)  POST 方法是一个非幂等方法，因为调用多次，会产生不同的副作用。

b)  例子：通过URL减少指定的用户的账户余额

### 支付软件的幂等性测试（防止表单重复提交）

\1.   场景：我们发起一笔付款请求，应该只扣用户账户一次钱，当遇到网络重发或系统bug重发，用户可能拿不响应的数据，数据在传输的时候丢包了，用户再次对该订单发起一次付款请求，后台也应该只扣一次钱

\2.   解决方法

a)  单线程：每笔订单生成唯一的订单号，在付款操作时服务器会将订单号存入容器中，并且查看容器中有无次订单号；有此订单号，说明此请求是重复提交的，重复提交的直接返回浏览器，不给于处理；无此订单号，说明此请求是第一次提交，进行处理

b)  多线程：加锁保证，程序的执行顺序



级联封装：

​	

## 文件处理

### 		文件下载

```java
	/*
	 下载：
	 	就是将服务器端的文件以流的形式写到客户端
	 	ResponseEntity:泛型代表的是放到响应体中的数据类型
	 		将要下载的文件数据，以及相应信息封装到这个对象中，
	 		浏览器通过解析发送回去的响应数据，就可以进行下载的操作
	 */
	@RequestMapping("/download")
	public ResponseEntity<byte[]> download(HttpServletRequest request) throws IOException{
		//将要下载文件读取成一个字节数组
		byte[] imgs ;
		//F:\ssm\springmvcday05\src\main\webapp\img\coco.gif
		ServletContext context = request.getServletContext();
		InputStream in = context.getResourceAsStream("img/coco.gif");
		//流中数据有多少，那么方法获取到的就是有多少，创建的字节数组长度就是为多大
		imgs = new byte[in.available()];
		in.read(imgs);//数组保存的是图片的字节数组，放到响应体中
		
		MultiValueMap<String , String>  headers = new HttpHeaders();
		//通过设置响应头的方式告诉浏览器，响应体中放的是字节数组，实际就是一张图片，需要进行下载
		headers.add("Content-Disposition", "attachment;filename="+System.currentTimeMillis()+".gif");
		ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(imgs,headers,HttpStatus.OK);
		return responseEntity;
	}
```

### 文件上传

​	导入依赖

```xml
 <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.4</version>
    </dependency>
    <dependency>
```

​	配置bean

```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
            <property name="maxUploadSize" value="#{1024*1024*30}"></property>
            <property name="defaultEncoding" value="utf-8"></property>
        </bean>
```

​	jsp文件

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<!-- enctype:编码类型 multipart/form-data：表单数据由多部分组成，既有文本数据，也有文件的二进制数据，可以
实现多种类型文件上传

application/x-www-form-urlencoded:只能上传文本格式的文件


文件上传：
	1.设置表单
	2.导入依赖：commons-fileupload
	3.在springmvc配置文件中，配置文件上传解析器
	4.文件上传请求处理
 -->
<form action="${pageContext.request.contextPath}/upload"  enctype="multipart/form-data" method="post">
用户名：<input type="text" name="userName"><br/>
用户头像1：
	<input type="file" name="headerimg"><br/>
用户头像2：	<input type="file" name="headerimg"><br/>
用户头像3：	<input type="file" name="headerimg"><br/>
用户头像4：	<input type="file" name="headerimg"><br/>
用户头像5：	<input type="file" name="headerimg"><br/>
	<input type="submit" value="提交"> 
</form>

<a href="${pageContext.request.contextPath}/test01">test拦截器</a>
</body>
</html>
```

java代码

```java
/**
	 * 单个文件上传
	 * @param userName
	 * @param file
	 * @param model
	 * @return
	 */
	/*@RequestMapping("/upload")
	public String upload(
			@RequestParam(value="userName",required=false) String userName,
			@RequestParam(value="headerimg",required=false)MultipartFile file,
			Model model) {
		System.out.println(userName);
		System.out.println(file.getName());
		System.out.println(file.getOriginalFilename());
		//上传文件
		File dest = new File("E:"+File.separator+"upload"+File.separator+file.getOriginalFilename());
		try {
			file.transferTo(dest);
			model.addAttribute("msg", "上传文件成功");
		} catch (IOException e) {
			e.printStackTrace();
			model.addAttribute("msg", "上传文件失败");
		}
		return "success";
	}*/
	/**
	 * 多个文件上传
	 * @param userName
	 * @param file
	 * @param model
	 * @return
	 */
	@RequestMapping("/upload")
	public String upload(
			@RequestParam(value="userName",required=false) String userName,
			@RequestParam(value="headerimg",required=false)MultipartFile[] files,
			Model model) {
		//遍历数组
		for(MultipartFile file : files){
			if(!file.isEmpty()){//不为空
				//上传
				//e:/upload/coco.gif
				//e:/upload/coco毫秒值.gif
				String fileName = file.getOriginalFilename();//coco.gif
				//前缀
				String prefix = fileName.substring(0,fileName.lastIndexOf("."));//coco
				System.out.println("前缀"+prefix);
				//后缀
				String suffix = fileName.substring(fileName.lastIndexOf("."));
				System.out.println("后缀"+suffix);
				//最终确定的文件名
				String name = prefix+System.currentTimeMillis()+suffix;
				File dest = new File("E:"+File.separator+"upload"+File.separator+name);
				try {
					file.transferTo(dest);
					model.addAttribute("msg", "上传文件成功");
				} catch (IOException e) {
					e.printStackTrace();
					model.addAttribute("msg", "上传文件失败");
				}
			}
		}
		return "success";
	}
```

## 拦截器

 	springmvc提供了拦截器机制，允许在运行目标方法之前，进行拦截工作，
 	或者目标方法运行之后，进行一些其他的处理
 	
 	过滤器：javaweb制定
 	拦截器：springmvc制定，接口 HandlerInterceptor
 	preHandle:在执行目标方法之前调用，返回布尔类型，如果方法返回true，
 	那就是目标方法放行，如果方法返回false，则目标方法不放行
 	postHandle：在目标方法运行之后调用
 	afterCompletion：请求全部完成之后执行，资源响应之后执行
 	

 实现HandlerInterceptor接口，重写方法

正常执行过程：preHandle方法返回值为true
preHandle-->目标方法--->postHandle--->目标页面-->afterCompletion

preHandle方法返回值为false:
preHandle方法执行了

目标方法出现异常：preHandle方法返回值为true:
preHandle-->目标方法--->afterCompletion



MyFirstInterceptor。。。。preHandle
MyTwoInterceptor...preHandle
test01方法执行了。。。
MyTwoInterceptor...postHandle
MyFirstInterceptor。。。。postHandle
success jsp运行了。。
MyTwoInterceptor...afterCompletion
MyFirstInterceptor。。。。afterCompletion

1--2--目标--2--1

1.拦截器不放行的情况下，则哪一块不放行从此以后都不会执行
如果第二个拦截器不放行，但是前面已经放行了的拦截器的afterCompletion还是会执行
：已经放行的拦截器，afterCompletion总会执行

2.
拦截器的preHandle：按照顺序执行
拦截器的postHandle:按照逆序执行
拦截器的afterCompletion：按照逆序执行





## shiro：

​	安全框架
​	作用：认证检测，授权，
​	

	springSecurity:和spring关系太紧密，使用起来比较复杂，


​	
​	Security manage:安全管理器：对主体进行认证和授权
​	Authenticator:认证器
​	Authorizer：授权器
​	realm:数据源，通过数据源存取认证和授权的相关数据
​	
​	在realm，存在授权和认证的逻辑


​	
​	UnknownAccountException：不存在用户异常
​	IncorrectCredentialsException：错误凭证异常
​	
​	用户认证
​	1.通过ini配置文件创建Security manager
​	2.调用subject的login方法提交认证
​	内部是通过ModularRealmAuthenticator拿着token和ini文件中所配置的用户名密码进行对比
​	iniRealm负责从shiro.ini文件中查询用户信息（用户名以及密码），如果用户名找不到，
​	那么ModularRealmAuthenticator返回null，说明用户不存在，就抛出异常
​	如果用户名找到，那么则比较密码，如果密码不对，ModularRealmAuthenticator抛出异常


​	
​	
​	
​	
​	shiro授权方式：
​	编程式
​	注解式
​	标签式
​	
​	user:create:权限标识符
​	资源：操作
​	
	资源:操作：实例
	user:update  表示对用户资源进行update操作
	user:update：1 表示对用户资源的1号实例进行update操作
	
	user:update：*:表示对所有用户的资源实例进行update操作
	
	user:*:1 ：表示用户资源实例1进行所有的操作
	
	用户授权：
		1.调用subject进行授权，调用的方法isPermitted，判断这个方法提供的参数是否在用户的权限范围内
		2.由安全管理进行授权，通过ModularRealmAuthorizer来进行授权
		3.由ModularRealmAuthorizer执行realm（自定义）从数据库中查询权限数据
		执行授权方法doGetAuthorizationInfo
		4.realm从数据库开始查询权限数据，返回给ModularRealmAuthorizer
		5.要进行权限数据的匹配，但是通过权限解析器进行权限字符串对比
		6.isPermitted方法提供的参数如果在realm查询的权限数据中，就说明该用户有权限进行操作，则授权成功，
	如果没有，则代表当前用户没有操作这个资源的权利。没有权限就会抛出异常


​	
​	
​	



