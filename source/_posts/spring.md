---
title: spring相关知识
date: 2018-11-19 14:55:34
tags: [java,spring]
categories: java
---

# web应用

这里先说一下web应用的一般加载顺序，先放出一张web.xml的样例



```
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
         version="2.5">
  <!-- spring的配置文件 -->

  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
  <!-- spring mvc核心：分发servlet -->
  <servlet>
    <servlet-name>mvc-dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- spring mvc的配置文件 -->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springMVC.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>


  <servlet-mapping>
    <servlet-name>mvc-dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>


</web-app>

```


**当一个web项目放到tomcat中的时候，tocat默认会去读取该项目webapp下的web.xml文件，先加载<context-param>节点信息，转换为键值对，并交给servletContext（servlet上下文，web项目所有部分都将共享这个上下文）,以便listen和filter等在初始化的时候用到这些上下文信息**


```
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
```

**然后创建listener中的类实例，创建监听器**

```
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```
**加载filter和servlet**

在加载servlet的时候可以指定通过load- on-startup属性来设置servlet的加载顺序，它的值必须是一个整数（如果被设置负数或者没有设置load- on-startup属性，则会在servlet被调用的时候才加载该servlet）

---

# spring加载（容器启动流程）

由web应用加载的过程我们可以得知，在web.xml加载的时候会去加载该listener

```
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```
查看该类可知ContextLoaderListener继承ContextLoader

**ContextLoaderListener.java**
![image](http://photo.xiaoyuwlr.com/images/ContextLoaderListener.jpg)

**ContextLoader.java**
![image](http://photo.xiaoyuwlr.com/images/ContextLoader.jpg)

==其中staic静态代码块会在类加载的时候就会执行，该类会去查找一个ContextLoader.properties的文件并加载，有兴趣的的可以debug进去看一下（注意类加载和实例化时代码块执行的顺序）==

再看回ContextLoaderListener，debug一下可知当java容器启动时会触发该类的contextInitialized方法（这里我就不截图了），该contextInitialized方法里会调用父类的initWebApplicationContext方法，具体执行步骤为：

1. java容器启动触发ContextLoaderListener的contextInitialized
2. contextInitialized 方法调用ContextLoader的initWebApplicationContext方法
3. initWebApplicationContext调用createWebApplicationContext方法
4. createWebApplicationContext 调用determineContextClass方法
5. determineContextClass有如下代码
```
contextClassName = defaultStrategies.getProperty(WebApplicationContext.class.getName());
```
可以得知是从defaultStrategies中加载的，而defaultStrategies在staic代码块执行的时候就已经初始化进去了，所以determineContextClass返回应该是ContextLoader.properties中的XmlWebApplicationContext。

6. 回到 initWebApplicationContext 方法，调用configureAndRefreshWebApplicationContext方法
7. configureAndRefreshWebApplicationContext 调用了AbstractApplicationContext的refresh方法
8. refresh 方法调用了obtainFreshBeanFactory
9. obtainFreshBeanFactory 调用了AbstractRefreshableApplicationContext类的refreshBeanFactory方法
10. refreshBeanFactory调用了XmlWebApplicationContext的loadBeanDefinitions
11. loadBeanDefinitions中加载了对应的applicationContext.xml

其中7-11不步我就没有一步一步debug进去看了，有兴趣的可以亲自debug进去看一看程序执行流程，其中initWebApplicationContext这个方法主要做三件事
1. 创建WebApplicationContext，通过createWebApplicationContext()方法
2. 加载spring配置文件，并创建beans。通过configureAndRefreshWebApplicationContext()方法
3. 将spring容器context挂载到ServletContext 这个web容器上下文中。通过servletContext.setAttribute()方法。

# 容器启动流程图
## spring容器初始化

![image](http://photo.xiaoyuwlr.com/images/spring.png)
## 创建WebApplicationContext对象流程
![image](http://photo.xiaoyuwlr.com/images/WebApplicationContext.png)
## 读取XML流程
![image](http://photo.xiaoyuwlr.com/images/readXml.png)
==看源码看的有些恶心了，剩下的留着下次继续看==
# spring XML配置文件解析
# spring bean创建和初始化

