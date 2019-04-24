
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

#### 3.5 Spring平台事务管理API
- 平台事务管理器：接口，是Spring用于管理事务的真正对象
	- **DataSourceTranctionManager** : 底层使用JDBC管理事务
	- **HibernateTranctionManager**：底层使用Hibernate管理事务

#### 3.6 TranscationDefinition：事务定义信息
- 事务定义：用于定义事务的相关的信息，**隔离级别**、超时信息、**传播行为**、是否只读

#### 3.7 TranscationStatus：事务的状态
- 事务状态：用于记录在事务管理过程中，事务的状态的对象

#### 3.8 事务管理的API的关系
- Spring进行事务管理的时候，首先**平台事务管理器**根据**事务定义**信息进行事务的管理，在事务管理的过程中，产生各种状态，将这些状态的信息记录到**事务状态**的对象中。

#### 3.9 传播行为
- 传播行为：业务层之间的方法相互调用，事务的传播行为主要业务层相互调用的问题
- Spring中提供了其中事务的传播行为
	- 保证多个操作在同一个事务中
		- **PROPAGATION_REQUIRED**: 默认值，使用A中的事务，使用A中的事务，如果A没有，创建一个新的事务，将操作包含起来
		- PROPAGTION_SUPPORTS
		- PROPAGTION_MANDATORY

	- 保证多个事务不再同一个事务中
		- **PROPAGATION_REQUIRES_NEW**:如果A中有事务，将A的事务挂起（暂停），创建新事务，只包含自身的操作。如果A中没有事务，创建一个新事务
		- PROPAGTION_NOT_SUPPORTS
		- PROPAGTION_NEVER

	- 嵌套事务
		- **PROPAGATION_NESTED**: 嵌套事务，如果A中有事务，按照A的事务执行，执行完成后，这只一个保存点，执行B中的操作，如果没有异常，执行通过，如果异常，可以选择回滚到保存点或初始状态

#### 3.10 声明式事务管理
**测试类关系**
![类关系](.\img\类关系.png)

##### 3.10.1 xml方式
```xml
<!-- 配置事务管理器 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!-- 配置事务的增强 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <!-- 事务管理的规则 -->
    <tx:attributes>
        <tx:method name="save*" propagation="REQUIRED"/>
        <tx:method name="update*" propagation="REQUIRED"/>
        <tx:method name="delete*" propagation="REQUIRED"/>
        <tx:method name="find*" read-only="true"/>
        <tx:method name="*" propagation="REQUIRED"/>
    </tx:attributes>
</tx:advice>

<!-- AOP配置 -->
<aop:config>
    <aop:pointcut expression="execution(* com.xx.www.demo3.AccountServiceImp.*(..))" id="pointcut1"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut1"/>
</aop:config>
```

##### 3.10.2 注解方式
```xml
<!-- 注解方式配置事务 -->
<!-- 配置事务管理器 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!-- 开启注解事务 -->
<tx:annotation-driven transaction-manager="transactionManager"/>
```
```java
package com.xx.www.demo3;

import org.springframework.transaction.annotation.Isolation;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;


/*	@Transactional：配置事务管理注解
 * isolation: 隔离级别
 * propagation： 传播行为
 * */
@Transactional(isolation=Isolation.DEFAULT,propagation=Propagation.REQUIRED)
public class AccountServiceImp implements AccountService {
	
	private AccountDao accountDao;
	
	public void setAccountDao(AccountDao accountDao) {
		this.accountDao = accountDao;
	}

	@Override
	public void tranferMoney(String from, String to, Double money) {
		// TODO Auto-generated method stub
		accountDao.outMoney(from, money);
//		int i = 1/0;
		accountDao.inMoney(to, money);
	}

}

```