---
title: spring
date: 2023-05-30 18:46:54
permalink: /pages/3lhl98/
categories:
  - spring篇
tags:
  - spring篇
---
# spring篇

[[toc]]

## 1.什么是spring?

Spring 是个java企业级应用的开源开发框架。Spring主要用来开发Java应用，但是有些扩展是针对

构建J2EE平台的web应用。Spring 框架目标是简化Java企业级应用开发，并通过POJO为基础的编程

模型促进良好的编程习惯。

## 2.你们项目中为什么使用Spring框架？

这么问的话，就直接说Spring框架的好处就可以了。比如说Spring有以下特点：

**轻量：**Spring 是轻量的，基本的版本大约2MB。

**控制反转：**Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找

依赖的对象们。

**面向切面的编程****(AOP)****：**Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。

**容器：**Spring 包含并管理应用中对象的生命周期和配置。

**MVC****框架**：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。

**事务管理：**Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务

（JTA）。**异常处理：**Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛

出的）转化为一致的unchecked 异常。

## 3.Autowired和Resource关键字的区别？

@Resource和@Autowired都是做bean的注入时使用，其实@Resource并不是Spring的注解，它的

包是javax.annotation.Resource，需要导入，但是Spring支持该注解的注入。

1、共同点

两者都可以写在字段和setter方法上。两者如果都写在字段上，那么就不需要再写setter方法。

2、不同点

（1）@Autowired

@Autowired为Spring提供的注解，需要导入包

org.springframework.beans.factory.annotation.Autowired;只按照byType注入。

````java
public class TestServiceImpl {
 // 下面两种@Autowired只要使用一种即可
 @Autowired
 private UserDao userDao; // 用于字段上
 
 @Autowired
 public void setUserDao(UserDao userDao) { // 用于属性的方法上
 this.userDao = userDao;
 }
}
````

果允许null值，可以设置它的required属性为false。如果我们想使用按照名称（byName）来装

配，可以结合@Qualififier注解一起使用。如下：

````java
public class TestServiceImpl {
 @Autowired
 @Qualifier("userDao")
 private UserDao userDao;
}
````

（2）@Resource

public class TestServiceImpl {

 // 下面两种@Autowired只要使用一种即可

 @Autowired

 private UserDao userDao; // 用于字段上

 

 @Autowired

 public void setUserDao(UserDao userDao) { // 用于属性的方法上

 this.userDao = userDao;

 }

}

public class TestServiceImpl {

 @Autowired

 @Qualifier("userDao")

 private UserDao userDao;

}@Resource默认按照ByName自动注入，由J2EE提供，需要导入包javax.annotation.Resource。

@Resource有两个重要的属性：name和type，而Spring将@Resource注解的name属性解析为

bean的名字，而type属性则解析为bean的类型。所以，如果使用name属性，则使用byName的自

动注入策略，而使用type属性时则使用byType自动注入策略。如果既不制定name也不制定type属

性，这时将通过反射机制使用byName自动注入策略。

````java
public class TestServiceImpl {
 // 下面两种@Resource只要使用一种即可
 @Resource(name="userDao")
 private UserDao userDao; // 用于字段上
 
 @Resource(name="userDao")
 public void setUserDao(UserDao userDao) { // 用于属性的setter方法上
 this.userDao = userDao;
 }
}
````

注：最好是将@Resource放在setter方法上，因为这样更符合面向对象的思想，通过set、get去操

作属性，而不是直接去操作属性。

@Resource装配顺序：

①如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛

出异常。

②如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常。

③如果指定了type，则从上下文中找到类似匹配的唯一bean进行装配，找不到或是找到多个，都会

抛出异常。

④如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，

则回退为一个原始类型进行匹配，如果匹配则自动装配。

@Resource的作用相当于@Autowired，只不过@Autowired按照byType自动注入。

## 4.依赖注入的方式有几种，各是什么?

**一、构造器注入** 将被依赖对象通过构造函数的参数注入给依赖对象，并且在初始化对象的时候注

入。

优点： 

对象初始化完成后便可获得可使用的对象。

缺点： 

当需要注入的对象很多时，构造器参数列表将会很长； 

不够灵活。若有多种注入方式，每种

方式只需注入指定几个依赖，那么就需要提供多个重载的构造函数，麻烦。

public class TestServiceImpl {

 // 下面两种@Resource只要使用一种即可

 @Resource(name="userDao")

 private UserDao userDao; // 用于字段上

 

 @Resource(name="userDao")

 public void setUserDao(UserDao userDao) { // 用于属性的setter方法上

 this.userDao = userDao;

 }

}

**二、****setter方法注入** IoC Service Provider通过调用成员变量提供的setter函数将被依赖对象注入给

依赖类。

优点： 

灵活。可以选择性地注入需要的对象。

缺点： 

依赖对象初始化完成后由于尚未注入被依赖对象，因此还不能使用。

**三、接口注入** 依赖类必须要实现指定的接口，然后实现该接口中的一个函数，该函数就是用于依赖

注入。该函数的参数就是要注入的对象。

优点 接口注入中，接口的名字、函数的名字都不重要，只要保证函数的参数是要注入的对象类型即

可。

缺点： 

侵入行太强，不建议使用。

PS：什么是侵入行？ 

如果类A要使用别人提供的一个功能，若为了使用这功能，需要在自己的类中

增加额外的代码，这就是侵入性。

## 5.讲一下什么是Spring?

Spring是一个轻量级的IoC和AOP容器框架。是为Java应用程序提供基础性服务的一套框架，目的是

用于简化企业应用程序的开发，它使得开发者只需要关心业务需求。常见的配置方式有三种：基于

XML的配置、基于注解的配置、基于Java的配置。

主要由以下几个模块组成：

Spring Core：核心类库，提供IOC服务；

Spring Context：提供框架式的Bean访问方式，以及企业级功能（JNDI、定时任务等）；

Spring AOP：AOP服务；

Spring DAO：对JDBC的抽象，简化了数据访问异常的处理；

Spring ORM：对现有的ORM框架的支持；

Spring Web：提供了基本的面向Web的综合特性，例如多方文件上传；

Spring MVC：提供面向Web应用的Model-View-Controller实现。

## 6.说说你对Spring MVC的理解?

**什么是****MVC****模式**

MVC：MVC是一种设计模式

MVC的原理图：

![image-20230530143503126](./8_mybatis.assets/image-20230530143503126.png)

**分析：**

M-Model 模型（完成业务逻辑：有javaBean构成，service+dao+entity）

V-View 视图（做界面的展示 jsp，html……）

C-Controller 控制器（接收请求—>调用模型—>根据结果派发页面）

springMVC是一个MVC的开源框架，springMVC=struts2+spring，springMVC就相当于是Struts2

加上sring的整合，但是这里有一个疑惑就是，springMVC和spring是什么样的关系呢？这个在百度

百科上有一个很好的解释：意思是说，springMVC是spring的一个后续产品，其实就是spring在原

有基础上，又提供了web应用的MVC模块，可以简单的把springMVC理解为是spring的一个模块

（类似AOP，IOC这样的模块），网络上经常会说springMVC和spring无缝集成，其实springMVC

就是spring的一个子模块，所以根本不需要同spring进行整合。

工作原理：

![image-20230530143739545](./8_mybatis.assets/image-20230530143739545.png)

1、 用户发送请求至前端控制器DispatcherServlet。

2、 DispatcherServlet收到请求调用HandlerMapping处理器映射器。

3、 处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器

拦截器(如果有则生成)一并返回给DispatcherServlet。

4、 DispatcherServlet调用HandlerAdapter处理器适配器。

5、 HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。

6、 Controller执行完成返回ModelAndView。

7、 HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。

8、 DispatcherServlet将ModelAndView传给ViewReslover视图解析器。

9、 ViewReslover解析后返回具体View。

10、DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。

11、 DispatcherServlet响应用户。

**组件说明：**

以下组件通常使用框架提供实现：

DispatcherServlet：作为前端控制器，整个流程控制的中心，控制其它组件执行，统一调度，降低

组件之间的耦合性，提高每个组件的扩展性。HandlerMapping：通过扩展处理器映射器实现不同的映射方式，例如：配置文件方式，实现接口

方式，注解方式等。

HandlAdapter：通过扩展处理器适配器，支持更多类型的处理器。

ViewResolver：通过扩展视图解析器，支持更多类型的视图解析，例如：jsp、freemarker、pdf、

excel等。

**组件：** 

**1****、前端控制器****DispatcherServlet****（不需要工程师开发）**

**,****由框架提供** 作用：接收请求，响

应结果，相当于转发器，中央处理器。有了dispatcherServlet减少了其它组件之间的耦合度。 用户

请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由它

调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。

**2****、处理器映射器****HandlerMapping(****不需要工程师开发****),****由框架提供** 作用：根据请求的url查找

Handler HandlerMapping负责根据用户请求找到Handler即处理器，springmvc提供了不同的映射

器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

**3****、处理器适配器****HandlerAdapter** 作用：按照特定规则（HandlerAdapter要求的规则）去执行

Handler 通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对

更多类型的处理器进行执行。

**4****、处理器****Handler(****需要工程师开发****)** **注意：编写****Handler****时按照****HandlerAdapter****的要求去做，**

**这样适配器才可以去正确执行****Handler** Handler 是继DispatcherServlet前端控制器的后端控制器，

在DispatcherServlet的控制下Handler对具体的用户请求进行处理。 由于Handler涉及到具体的用

户业务请求，所以一般情况需要工程师根据业务需求开发Handler。

**5****、视图解析器****View resolver(****不需要工程师开发****),****由框架提供** 作用：进行视图解析，根据逻辑视图

名解析成真正的视图（view） View Resolver负责将处理结果生成View视图，View Resolver首先根

据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将

处理结果通过页面展示给用户。 springmvc框架提供了很多的View视图类型，包括：jstlView、

freemarkerView、pdfView等。 一般情况下需要通过页面标签或页面模版技术将模型数据通过页面

展示给用户，需要由工程师根据业务需求开发具体的页面。

**6****、视图****View(****需要工程师开发****jsp...)** View是一个接口，实现类支持不同的View类型（jsp、

freemarker、pdf...）

**核心架构的具体流程步骤如下：** 1、首先用户发送请求——>DispatcherServlet，前端控制器收到

请求后自己不进行处理，而是委托给其他的解析器进行处理，作为统一访问点，进行全局的流程控

制； 2、DispatcherServlet——>HandlerMapping， HandlerMapping 将会把请求映射为

HandlerExecutionChain 对象（包含一个Handler 处理器（页面控制器）对象、多个

HandlerInterceptor 拦截器）对象，通过这种策略模式，很容易添加新的映射策略； 3、

DispatcherServlet——>HandlerAdapter，HandlerAdapter 将会把处理器包装为适配器，从而支

持多种类型的处理器，即适配器设计模式的应用，从而很容易支持很多类型的处理器； 4、

HandlerAdapter——>处理器功能处理方法的调用，HandlerAdapter 将会根据适配的结果调用真正的处理器的功能处理方法，完成功能处理；并返回一个ModelAndView 对象（包含模型数据、逻

辑视图名）； 5、ModelAndView的逻辑视图名——> ViewResolver， ViewResolver 将把逻辑视图

名解析为具体的View，通过这种策略模式，很容易更换其他视图技术； 6、View——>渲染，View

会根据传进来的Model模型数据进行渲染，此处的Model实际是一个Map数据结构，因此很容易支

持其他视图技术； 7、返回控制权给DispatcherServlet，由DispatcherServlet返回响应给用户，到

此一个流程结束。

看到这些步骤我相信大家很感觉非常的乱，这是正常的，但是这里主要是要大家理解springMVC中

的几个组件：

前端控制器（DispatcherServlet）：接收请求，响应结果，相当于电脑的CPU。

处理器映射器（HandlerMapping）：根据URL去查找处理器。

处理器（Handler）：需要程序员去写代码处理逻辑的。

处理器适配器（HandlerAdapter）：会把处理器包装成适配器，这样就可以支持多种类型的处理

器，类比笔记本的适配器（适配器模式的应用）。

视图解析器（ViewResovler）：进行视图解析，多返回的字符串，进行处理，可以解析成对应的页

面。

## 7. SpringMVC常用的注解有哪些?

@RequestMapping：用于处理请求 url 映射的注解，可用于类或方法上。用于类上，则表示类中

的所有响应请求的方法都是以该地址作为父路径。

@RequestBody：注解实现接收http请求的json数据，将json转换为java对象。

@ResponseBody：注解实现将conreoller方法返回对象转化为json对象响应给客户。

## 8.**谈谈你对Spring的AOP理解**?

AOP（Aspect-Oriented Programming，面向切面编程）能够将那些与业务无关，却为业务模块所

共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复

代码，降低模块间的耦合度，并有利于未来的可扩展性和可维护性。

Spring AOP是基于动态代理的，如果要代理的对象实现了某个接口，那么Spring AOP就会使用JDK

动态代理去创建代理对象；而对于没有实现接口的对象，就无法使用JDK动态代理，转而使用CGlib

动态代理生成一个被代理对象的子类来作为代理。

当然也可以使用AspectJ，Spring AOP中已经集成了AspectJ，AspectJ应该算得上是Java生态系统中

最完整的AOP框架了。使用AOP之后我们可以把一些通用功能抽象出来，在需要用到的地方直接使

用即可，这样可以大大简化代码量。我们需要增加新功能也方便，提高了系统的扩展性。日志功

能、事务管理和权限管理等场景都用到了AOP。

这里只要你提到了AspectJ，那么面试官很有可能会继续问：

## 9.Spring AOP和AspectJ AOP有什么区别？

Spring AOP是属于运行时增强，而AspectJ是编译时增强。Spring AOP基于代理（Proxying），而

AspectJ基于字节码操作（Bytecode Manipulation）。

Spring AOP已经集成了AspectJ，AspectJ应该算得上是Java生态系统中最完整的AOP框架了。

AspectJ相比于Spring AOP功能更加强大，但是Spring AOP相对来说更简单。

如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择AspectJ，它比

SpringAOP快很多。

可能还会继续问：

**在****Spring AOP** **中，关注点和横切关注的区别是什么？**

关注点是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。 横切关

注点是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数

据传输，几乎应用的每个模块都需要的功能。因此这些都属于横切关注点。

那什么是连接点呢？连接点代表一个应用程序的某个位置，在这个位置我们可以插入一个AOP切

面，它实际上是个应用程序执行Spring AOP的位置。切入点是什么？切入点是一个或一组连接点，通知将在这些位置执行。可以通过表达式或匹配的方

式指明切入点。

**什么是通知呢？有哪些类型呢？**

通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过SpringAOP框架触发的代

码段。

Spring切面可以应用五种类型的通知：

**before**：前置通知，在一个方法执行前被调用。

**after:** 在方法执行之后调用的通知，无论方法执行是否成功。

**after-returning:** 仅当方法成功完成后执行的通知。

**after-throwing:** 在方法抛出异常退出时执行的通知。

**around:** 在方法执行之前和之后调用的通知。

## 10.说说你对Spring的IOC是怎么理解的？

（1）IOC就是控制反转，是指创建对象的控制权的转移。以前创建对象的主动权和时机是由自己把

控的，而现在这种权力转移到Spring容器中，并由容器根据配置文件去创建实例和管理各个实例之

间的依赖关系。对象与对象之间松散耦合，也利于功能的复用。DI依赖注入，和控制反转是同一个

概念的不同角度的描述，即 应用程序在运行时依赖IoC容器来动态注入对象需要的外部资源。

（2）最直观的表达就是，IOC让对象的创建不用去new了，可以由spring自动生产，使用java的反

射机制，根据配置文件在运行时动态的去创建对象以及管理对象，并调用对象的方法的。

（3）Spring的IOC有三种注入方式 ：构造器注入、setter方法注入、根据注解注入。

IoC让相互协作的组件保持松散的耦合，而AOP编程允许你把遍布于应用各层的功能分离出来

形成可重用的功能组件。

## 11.解释一下spring bean的生命周期？

首先说一下Servlet的生命周期：实例化，初始init，接收请求service，销毁destroy；

Spring上下文中的Bean生命周期也类似，如下：

（1）实例化Bean：

对于BeanFactory容器，当客户向容器请求一个尚未初始化的bean时，或初始化bean的时候需要注

入另一个尚未初始化的依赖时，容器就会调用createBean进行实例化。对于ApplicationContext容

器，当容器启动结束后，通过获取BeanDefifinition对象中的信息，实例化所有的bean。（2）设置对象属性（依赖注入）：

实例化后的对象被封装在BeanWrapper对象中，紧接着，Spring根据BeanDefifinition中的信息 以

及 通过BeanWrapper提供的设置属性的接口完成依赖注入。

（3）处理Aware接口：

接着，Spring会检测该对象是否实现了xxxAware接口，并将相关的xxxAware实例注入给Bean：

①如果这个Bean已经实现了BeanNameAware接口，会调用它实现的setBeanName(String

beanId)方法，此处传递的就是Spring配置文件中Bean的id值；

②如果这个Bean已经实现了BeanFactoryAware接口，会调用它实现的setBeanFactory()方法，传

递的是Spring工厂自身。

③如果这个Bean已经实现了ApplicationContextAware接口，会调用

setApplicationContext(ApplicationContext)方法，传入Spring上下文；

（4）BeanPostProcessor：

如果想对Bean进行一些自定义的处理，那么可以让Bean实现了BeanPostProcessor接口，那将会

调用postProcessBeforeInitialization(Object obj, String s)方法。

（5）InitializingBean 与 init-method：

如果Bean在Spring配置文件中配置了 init-method 属性，则会自动调用其配置的初始化方法。

（6）如果这个Bean实现了BeanPostProcessor接口，将会调用

postProcessAfterInitialization(Object obj, String s)方法；由于这个方法是在Bean初始化结束时调

用的，所以可以被应用于内存或缓存技术；

以上几个步骤完成后，Bean就已经被正确创建了，之后就可以使用这个Bean了。

（7）DisposableBean：

当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，会调用其实现

的destroy()方法；

（8）destroy-method：

最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方

法。

![image-20230530144518322](./9_spring.assets/image-20230530144518322.png)

## **12.** 解释Spring支持的几种bean的作用域？

Spring容器中的bean可以分为5个范围：

（1）singleton：默认，每个容器中只有一个bean的实例，单例的模式由BeanFactory自身来维

护。

（2）prototype：为每一个bean请求提供一个实例。

（3）request：为每一个网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回

收。

（4）session：与request范围类似，确保每个session中有一个bean的实例，在session过期后，

bean会随之失效。

（5）global-session：全局作用域，global-session和Portlet应用相关。当你的应用部署在Portlet

容器中工作时，它包含很多portlet。如果你想要声明让所有的portlet共用全局的存储变量的话，那

么这全局变量需要存储在global-session中。全局作用域与Servlet中的session作用域效果相同。

## **13.** Spring基于xml注入bean的几种方式？

（1）Set方法注入；

（2）构造器注入：①通过index设置参数的位置；②通过type设置参数类型；（3）静态工厂注入；

（4）实例工厂；

通常回答前面两种即可，因为后面两种很多人都不太会，不会的就不要说出来，不然问到你不会就

尴尬了。

## **14. **Spring框架中都用到了哪些设计模式**？**

这是一道相对有难度的题目，你不仅要回设计模式，还要知道每个设计模式在Spring中是如何使用

的。

**简单工厂模式**：Spring 中的 BeanFactory 就是简单工厂模式的体现。根据传入一个唯一的标识来获

得 Bean 对象，但是在传入参数后创建还是传入参数前创建，要根据具体情况来定。

**工厂模式**：Spring 中的 FactoryBean 就是典型的工厂方法模式，实现了 FactoryBean 接口的 bean

是一类叫做 factory 的 bean。其特点是，spring 在使用 getBean() 调用获得该 bean 时，会自动调

用该 bean 的 getObject() 方法，所以返回的不是 factory 这个 bean，而是这个 bean.getOjbect()

方法的返回值。

**单例模式**：在 spring 中用到的单例模式有： scope="singleton" ，注册式单例模式，bean 存放于

Map 中。bean name 当做 key，bean 当做 value。

**原型模式**：在 spring 中用到的原型模式有： scope="prototype" ，每次获取的是通过克隆生成的新

实例，对其进行修改时对原有实例对象不造成任何影响。

**迭代器模式**：在 Spring 中有个 CompositeIterator 实现了 Iterator，Iterable 接口和 Iterator 接

口，这两个都是迭代相关的接口。可以这么认为，实现了 Iterable 接口，则表示某个对象是可被迭

代的。Iterator 接口相当于是一个迭代器，实现了 Iterator 接口，等于具体定义了这个可被迭代的

对象时如何进行迭代的。

**代理模式**：Spring 中经典的 AOP，就是使用动态代理实现的，分 JDK 和 CGlib 动态代理。

**适配器模式**：Spring 中的 AOP 中 AdvisorAdapter 类，它有三个实现：

MethodBeforAdviceAdapter、AfterReturnningAdviceAdapter、ThrowsAdviceAdapter。Spring

会根据不同的 AOP 配置来使用对应的 Advice，与策略模式不同的是，一个方法可以同时拥有多个

Advice。Spring 存在很多以 Adapter 结尾的，大多数都是适配器模式。

**观察者模式**：Spring 中的 Event 和 Listener。spring 事件：ApplicationEvent，该抽象类继承了

EventObject 类，JDK 建议所有的事件都应该继承自 EventObject。spring 事件监听器：

ApplicationListener，该接口继承了 EventListener 接口，JDK 建议所有的事件监听器都应该继承

EventListener。

**模板模式**：Spring 中的 org.springframework.jdbc.core.JdbcTemplate 就是非常经典的模板模式

的应用，里面的 execute 方法，把整个算法步骤都定义好了。**责任链模式**：DispatcherServlet 中的 doDispatch() 方法中获取与请求匹配的处理器

HandlerExecutionChain，this.getHandler() 方法的处理使用到了责任链模式。

**注意**：这里只是列举了部分设计模式，其实里面用到了还有享元模式、建造者模式等。可选择性的

回答，主要是怕你回答了迭代器模式，然后继续问你，结果你一问三不知，那就尴了尬了。

## **15.说说Spring** **中** **ApplicationContext** **和** **BeanFactory** **的区别**?

**包目录不同**

spring-beans.jar 中 org.springframework.beans.factory.BeanFactory

spring-context.jar 中 org.springframework.context.ApplicationContext

**国际化**

BeanFactory 是不支持国际化功能的，因为 BeanFactory 没有扩展 Spring 中 MessageResource

接口。相反，由于 ApplicationContext 扩展了 MessageResource 接口，因而具有消息处理的能力

（i18N）。

**强大的事件机制（****Event****）**

基本上牵涉到事件（Event）方面的设计，就离不开观察者模式，ApplicationContext 的事件机制

主要通过 ApplicationEvent 和 ApplicationListener 这两个接口来提供的，和 Java swing 中的事件

机制一样。即当 ApplicationContext 中发布一个事件时，所有扩展了 ApplicationListener 的 Bean

都将接受到这个事件，并进行相应的处理。

**底层资源的访问**

ApplicationContext 扩展了 ResourceLoader（资源加载器）接口，从而可以用来加载多个

Resource，而 BeanFactory 是没有扩展 ResourceLoader。

**对** **Web** **应用的支持**

与 BeanFactory 通常以编程的方式被创建，ApplicationContext 能以声明的方式创建，如使用

ContextLoader。当然你也可以使用 ApplicationContext 的实现方式之一，以编程的方式创建 ApplicationContext

实例。

**延迟加载**

\1. BeanFactroy 采用的是延迟加载形式来注入 Bean 的，即只有在使用到某个 Bean 时(调用

getBean())，才对该 Bean 进行加载实例化。这样，我们就不能发现一些存在的 spring 的配置

问题。而 ApplicationContext 则相反，它是在容器启动时，一次性创建了所有的 Bean。这

样，在容器启动时，我们就可以发现 Spring 中存在的配置错误。

\2. BeanFactory 和 ApplicationContext 都支持 BeanPostProcessor、

BeanFactoryPostProcessor 的使用。两者之间的区别是：BeanFactory 需要手动注册，而

ApplicationContext 则是自动注册。

可以看到，ApplicationContext 继承了 BeanFactory，BeanFactory 是 Spring 中比较原始的

Factory，它不支持 AOP、Web 等 Spring 插件。而 ApplicationContext 不仅包含了 BeanFactory

的所有功能，还支持 Spring 的各种插件，还以一种面向框架的方式工作以及对上下文进行分层和实

现继承。

BeanFactory 是 Spring 框架的基础设施，面向 Spring 本身；而 ApplicationContext 面向使用

Spring 的开发者，相比 BeanFactory 提供了更多面向实际应用的功能，几乎所有场合都可以直接使

用 ApplicationContext，而不是底层的 BeanFactory。

**常用容器**

BeanFactory 类型的有 XmlBeanFactory，它可以根据 XML 文件中定义的内容，创建相应的

Bean。

ApplicationContext 类型的常用容器有：

\1. ClassPathXmlApplicationContext：从 ClassPath 的 XML 配置文件中读取上下文，并生成上

下文定义。应用程序上下文从程序环境变量中取得。

\2. FileSystemXmlApplicationContext：由文件系统中的 XML 配置文件读取上下文。

\3. XmlWebApplicationContext：由 Web 应用的 XML 文件读取上下文。例如我们在 Spring MVC

使用的情况。

## **16.Spring** **框架中的单例** **Bean** **是线程安全的么？**

Spring 框架并没有对单例 Bean 进行任何多线程的封装处理。

关于单例 Bean 的线程安全和并发问题，需要开发者自行去搞定。

单例的线程安全问题，并不是 Spring 应该去关心的。Spring 应该做的是，提供根据配置，创

建单例 Bean 或多例 Bean 的功能。当然，但实际上，大部分的 Spring Bean 并没有可变的状态，所以在某种程度上说 Spring 的单例

Bean 是线程安全的。如果你的 Bean 有多种状态的话，就需要自行保证线程安全。最浅显的解决办

法，就是将多态 Bean 的作用域（Scope）由 Singleton 变更为 Prototype。

## **17.Spring** **是怎么解决循环依赖的？**

整个流程大致如下：

\1. 首先 A 完成初始化第一步并将自己**提前曝光**出来（通过 ObjectFactory 将自己提前曝光），在

初始化的时候，发现自己依赖对象 B，此时就会去尝试 get(B)，这个时候发现 B 还没有被创建

出来；

\2. 然后 B 就走创建流程，在 B 初始化的时候，同样发现自己依赖 C，C 也没有被创建出来；

\3. 这个时候 C 又开始初始化进程，但是在初始化的过程中发现自己依赖 A，于是尝试 get(A)。这

个时候由于 A 已经添加至缓存中（一般都是添加至三级缓存 singletonFactories），通过

ObjectFactory 提前曝光，所以可以通过 ObjectFactory#getObject() 方法来拿到 A 对象。C 拿

到 A 对象后顺利完成初始化，然后将自己添加到一级缓存中；

\4. 回到 B，B 也可以拿到 C 对象，完成初始化，A 可以顺利拿到 B 完成初始化。到这里整个链路

就已经完成了初始化过程了。

关键字：三级缓存，提前曝光。

## **18.说说事务的隔离级别**?

未提交读(Read Uncommitted)：允许脏读，也就是可能读取到其他会话中未提交事务修改的数据

提交读(Read Committed)：只能读取到已经提交的数据。Oracle等多数数据库默认都是该级别 (不

重复读)可重复读(Repeated Read)：在同一个事务内的查询都是事务开始时刻一致的，Mysql的InnoDB默

认级别。在SQL标准中，该隔离级别消除了不可重复读，但是还存在幻读（多个事务同时修改同一

条记录，事务之间不知道彼此存在，当事务提交之后，后面的事务修改的数据将会覆盖前事务，前

一个事务就像发生幻觉一样）

可串行化(Serializable)：完全串行化的读，每次读都需要获得表级共享锁，读写相互都会阻塞。

![image-20230530145127003](./9_spring.assets/image-20230530145127003.png)

不可重复读和幻读的区别主要是：解决不可重复读需要锁定了当前满足条件的记录，而解决幻读需

要锁定当前满足条件的记录及相近的记录。比如查询某个商品的信息，可重复读事务隔离级别可以

保证当前商品信息被锁定，解决不可重复读；但是如果统计商品个数，中途有记录插入，可重复读

事务隔离级别就不能保证两个事务统计的个数相同。

## **19.说说事务的传播级别**?

Spring事务定义了7种传播机制：

\1. PROPAGATION_REQUIRED:默认的Spring事物传播级别，若当前存在事务，则加入该事务，若

不存在事务，则新建一个事务。

\2. PAOPAGATION_REQUIRE_NEW:若当前没有事务，则新建一个事务。若当前存在事务，则新建

一个事务，新老事务相互独立。外部事务抛出异常回滚不会影响内部事务的正常提交。

\3. PROPAGATION_NESTED:如果当前存在事务，则嵌套在当前事务中执行。如果当前没有事务，

则新建一个事务，类似于REQUIRE_NEW。

\4. PROPAGATION_SUPPORTS:支持当前事务，若当前不存在事务，以非事务的方式执行。

\5. PROPAGATION_NOT_SUPPORTED:以非事务的方式执行，若当前存在事务，则把当前事务挂

起。

\6. PROPAGATION_MANDATORY:强制事务执行，若当前不存在事务，则抛出异常.

\7. PROPAGATION_NEVER:以非事务的方式执行，如果当前存在事务，则抛出异常。

Spring事务传播级别一般不需要定义，默认就是PROPAGATION_REQUIRED，除非在嵌套事务的情

况下需要重点了解。

## **20.Spring** **事务实现方式**?

编程式事务管理：这意味着你可以通过编程的方式管理事务，这种方式带来了很大的灵活性，但很

难维护。

声明式事务管理：这种方式意味着你可以将事务管理和业务代码分离。你只需要通过注解或者XML

配置管理事务。

## **21. **Spring框架的事务管理有哪些优点?

它为不同的事务API(如JTA, JDBC, Hibernate, JPA, 和JDO)提供了统一的编程模型。它为编程式事务

管理提供了一个简单的API而非一系列复杂的事务API(如JTA).它支持声明式事务管理。它可以和

Spring 的多种数据访问技术很好的融合。

它为不同的事务API(如JTA, JDBC, Hibernate, JPA, 和JDO)提供了统一的编程模型。它为编程式事务

管理提供了一个简单的API而非一系列复杂的事务API(如JTA).它支持声明式事务管理。它可以和

Spring 的多种数据访问技术很好的融合。

它为不同的事务API(如JTA, JDBC, Hibernate, JPA, 和JDO)提供了统一的编程模型。它为编程式事务

管理提供了一个简单的API而非一系列复杂的事务API(如JTA).它支持声明式事务管理。它可以和

Spring 的多种数据访问技术很好的融合。

## **22.事务三要素是什么？**

**数据源**：表示具体的事务性资源，是事务的真正处理者，如MySQL等。

**事务管理器**：像一个大管家，从整体上管理事务的处理过程，如打开、提交、回滚等。

**事务应用和属性配置**：像一个标识符，表明哪些方法要参与事务，如何参与事务，以及一些相关属

性如隔离级别、超时时间等。

## **23. 事务注解的本质是什么？**

@Transactional 这个注解仅仅是一些（和事务相关的）元数据，在运行时被事务基础设施读取消

费，并**使用这些元数据来配置****bean****的事务行为**。 大致来说具有两方面功能，**一是表明该方法要参**

**与事务，二是配置相关属性来定制事务的参与方式和运行行为**

声明式事务主要是得益于Spring AOP。使用一个事务拦截器，在方法调用的前后/周围进行事务性

增强（advice），来驱动事务完成。

@Transactional注解既可以标注在类上，也可以标注在方法上。当在类上时，默认应用到类里的所

有方法。如果此时方法上也标注了，则方法上的优先级高。 另外注意方法一定要是public的。