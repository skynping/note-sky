
[TOC]

## 1.基于AspectJ的AOP注解开发

#### 1.1引入约束和相关包

```xml
	<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx.xsd">
    </beans>
```

![aop注解开发必需包.bmp](.\img\aop注解开发必需包.bmp)


#### 1.2 开启注解开发
```xml
	<!-- 在配置文件中开启注解的AOP的开发 -->
	<aop:aspectj-autoproxy/>
```
#### 1.3 配置目标类和切面类
```xml
	<!-- 配置目标类 -->
    <bean id="userDao" class="com.xx.www.demo1.UserDao"></bean>
    <!-- 配置切面类 -->
    <bean id="myAspectJ" class="com.xx.www.demo1.MyAspectJ"></bean>
```
#### 1.4 编写切面类
```java
package com.xx.www.demo1;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;

@Aspect
public class MyAspectJ {
	
	// 配置前置通知
	@Before(value="execution(* com.xx.www.demo1.UserDao.save(..))")
	public void toBefore() {
		System.out.println("前置增强。。。");
	}
	
	// 后置通知
	@AfterReturning(value="execution(* com.xx.www.demo1.UserDao.delete(..))",returning="result")
	public void back(Object result) {
		System.out.println("后置增强。。。" + result);
	}
	
	// 环绕通知
	@Around(value="execution(* com.xx.www.demo1.UserDao.update(..))")
	public Object round(ProceedingJoinPoint joinPoint) throws Throwable {
		System.out.println("环绕增强前。。。");
		Object obj = joinPoint.proceed(); // 运行被增强的方法
		System.out.println("环绕增强后。。。");
		return obj;
	}
	
	// 异常抛出通知
	@AfterThrowing(value="execution(* com.xx.www.demo1.UserDao.find(..))",throwing="e")
	public void throwException(Throwable e) {
		System.out.println("异常增强后。。。" + e.getMessage());
	}
	
	// 最终通知
	@After(value="MyAspectJ.pointcut1()")
	public void fin() {
		System.out.println("最终通知");
	}
	
	// 切入点注解
	@Pointcut(value="execution(* com.xx.www.demo1.UserDao.find(..))")
	private void pointcut1() {}
	
}
```

## 2. 连接数据库的几种方式
#### 2.1 JDBC原生方式
```xml
<!--  配置spring内置连接池 -->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <!--配置数据库连接相关属性 -->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3307/spring"/>
    <property name="username" value="root"/>
    <property name="password" value="ubuntu"/>
</bean>

<!--配置spring的JDBC的模板  -->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"></property>
</bean>
```
```java
package com.xx.www.demo2;

import org.junit.Test;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

public class jdbcdemo {
	@Test
	public void demo() {
		// 创建连接池
		DriverManagerDataSource datasource = new DriverManagerDataSource();
		datasource.setDriverClassName("com.mysql.jdbc.Driver");
		datasource.setUrl("jdbc:mysql://localhost:3307/spring");
		datasource.setUsername("root");
		datasource.setPassword("ubuntu");
		// 创建jdbc模板
		JdbcTemplate jdbctempalte = new JdbcTemplate(datasource);
		jdbctempalte.update("insert into account values(null,?,?)","小张",10000d);
	}
}
```
**引入相关包**
![JDBC](.\img\JDBC.png)

#### 2.2 DBCP方式
```xml
<!-- DBCP连接 -->
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3307/spring"/>
    <property name="username" value="root"/>
    <property name="password" value="ubuntu"/>
</bean>

<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"></property>
</bean>
```

```java
package com.xx.www.demo2;

import javax.annotation.Resource;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:ApplicationContext.xml")
public class jdbcSppringdemo {
	
	@Resource(name="jdbcTemplate")
	private JdbcTemplate jdbcTemplate;
	
	@Test
	public void demo() {
		jdbcTemplate.update("insert into account values(null,?,?)","小李",10000d);
	}
}
```
**引入相关包**
![DBCP](.\img\dbcp.png)

#### 2.3 C3P0方式
```xml
<!-- c3p0 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver"/>
    <property name="jdbcUrl" value="jdbc:mysql://localhost:3307/spring"/>
    <property name="user" value="root"/>
    <property name="password" value="ubuntu"/>
</bean>

<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"></property>
</bean>
```
```java
/*java代码同DBCP*/
```
**引入相关包**
![c3o0](.\img\c3p0.png)

#### 2.4 引入属性文件
在类路径下建立文件jdbc.properties
```xml
<!-- 引入属性文件 -->
<context:property-placeholder location="classpath:jdbc.properties"/>

<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="${jdbc.driverClass}"/>
    <property name="jdbcUrl" value="${jdbc.url}"/>
    <property name="user" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>

<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"></property>
</bean>
```

## 3. 事务
#### 3.1 什么是事务
- 事务：逻辑上的一组操作，组成这组操作的各个单元，要么全部成功，要么全部失败

#### 3.2 事务的特性
- **原子性**：事务不可分割
- **隔离性**：一个事务的执行不应该受到其他事务的干扰
- **一致性**：事务执行前后数据整体保持一致
- **持久性**; 一旦事务结束，数据就持久化到数据库

#### 3.3 不考虑事务的隔离性引发的问题
1. 读问题
	- ==**脏读**==：一个事务读到一个事务未提交的数据
	- ==**不可重复读**==：一个事务读取到另一个事务update的数据，导致一个事务多次查询结果不一致
	- ==**虚读**==：一个事务读到另一个事务已经insert的数据，导致一个事务多次查询结果不一致
2. 写问题
	- 丢失更新

#### 3.4 解决读问题
- 设置事务隔离级别
	* Read uncommitted: 未提交读，解决不了任何问题
	* **Read committed**： 已提交读，解决脏读
	* **Repeatable read**： 重复读，解决脏读和重复读
	* Serializable：解决所有问题

