---
layout: post
date: 2018-03-20 23:22
title: "Spring+Mybatis+Mysql接口层读写分离(应用篇)"
categories: Java
tag: 
    - 读写分离
    - 主从分离
    - Spring
    - MySql
comment: true
---
## 背景介绍

在[上一篇文章](http://www.csuldw.com/2018/03/13/2018-03-13-spring-mybatis-read-write-seperation/)中已经介绍过读写分离，并且通过代码也已实现局部的读写分离。为什么说是局部的呢？首先，来分析下，针对上一篇文章中提到的方法，如果在service层没有配置事务，那么当程序走到Dao层时，就可以根据自己定义的规则进行读写分离；倘若在service层配置了事物，那么在Dao切换数据库key的时候，是无法正真的进行读写分离的。因此，通过进一步的研究和尝试，找到了一种新的方法来实现真正意义上的Dao层读写分离，该方法可以在事务内部直接切换数据库，达到读写分库的功能。

<!-- more -->

## 思路

上一篇文章中介绍的方法，对存在事务的方法使用分库功能，无法成功，主要是因为Service层进入事务之后，在内部切换数据库Key无法正确。因此，继续深入，发现另一种方法是切换SqlSessionFactory，可以达到分库的效果。Spring整合MyBatis切换SqlSessionFactory有两种方法，第一种是继承SqlSessionDaoSupport，然后重写获取SqlSessionFactory的方法；第二中是继承SqlSessionTemplate，然后重写getSqlSessionFactory、getConfiguration 和SqlSessionInterceptor 这个拦截器。其中最为关键还是继承SqlSessionTemplate 并重写里面的方法。整个读写分离可归纳为以下三个步骤：

- Step 1：实现动态切换数据源：配置两个DataSource，配置两个SqlSessionFactory指向两个不同的DataSource，两个SqlSessionFactory都用一个SqlSessionTemplate，同时重写Mybatis提供的SqlSessionTemplate类，最后配置Mybatis自动扫描。
- Step 2：利用aop切面，拦截dao层所有方法，因为dao层方法命名的特点，比如所有查询sql都是select开头，或者get开头等等，拦截这些方法，并把当前数据源切换至从库。
- Step 3：由于在事务内部进行了分库，所以需要引入atomikos进行多事务分布式管理。


## 实现

### 重写SqlSessionTemplate

DynamicSqlSessionTemplate类继承SqlSessionTemplate，并重写getSqlSessionFactory、getConfiguration和SqlSessionInterceptor这个拦截器。代码如下：

1. DynamicSqlSessionTemplate.java

```java
package com.test.dlab.aop.core;

import static java.lang.reflect.Proxy.newProxyInstance;
import static org.apache.ibatis.reflection.ExceptionUtil.unwrapThrowable;
import static org.mybatis.spring.SqlSessionUtils.closeSqlSession;
import static org.mybatis.spring.SqlSessionUtils.getSqlSession;
import static org.mybatis.spring.SqlSessionUtils.isSqlSessionTransactional;
 
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.sql.Connection;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.exceptions.PersistenceException;
import org.apache.ibatis.executor.BatchResult;
import org.apache.ibatis.session.Configuration;
import org.apache.ibatis.session.ExecutorType;
import org.apache.ibatis.session.ResultHandler;
import org.apache.ibatis.session.RowBounds;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.MyBatisExceptionTranslator;
import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.dao.support.PersistenceExceptionTranslator;
import org.springframework.util.Assert;

/**
 * 
 * @author liudiwei
 *
 */
public class DynamicSqlSessionTemplate extends SqlSessionTemplate {
 
    private final SqlSessionFactory sqlSessionFactory;
    private final ExecutorType executorType;
    private final SqlSession sqlSessionProxy;
    private final PersistenceExceptionTranslator exceptionTranslator;
 
    private Map<Object, SqlSessionFactory> targetSqlSessionFactorys;
    private SqlSessionFactory defaultTargetSqlSessionFactory;
 
    public void setTargetSqlSessionFactorys(Map<Object, SqlSessionFactory> targetSqlSessionFactorys) {
        this.targetSqlSessionFactorys = targetSqlSessionFactorys;
    }
    
    public Map<Object, SqlSessionFactory> getTargetSqlSessionFactorys(){
        return targetSqlSessionFactorys;
    }
 
    public void setDefaultTargetSqlSessionFactory(SqlSessionFactory defaultTargetSqlSessionFactory) {
        this.defaultTargetSqlSessionFactory = defaultTargetSqlSessionFactory;
    }
 
    public DynamicSqlSessionTemplate(SqlSessionFactory sqlSessionFactory) {
        this(sqlSessionFactory, sqlSessionFactory.getConfiguration().getDefaultExecutorType());
    }
 
    public DynamicSqlSessionTemplate(SqlSessionFactory sqlSessionFactory, ExecutorType executorType) {
        this(sqlSessionFactory, executorType, new MyBatisExceptionTranslator(sqlSessionFactory.getConfiguration()
                .getEnvironment().getDataSource(), true));
    }
 
    public DynamicSqlSessionTemplate(SqlSessionFactory sqlSessionFactory, ExecutorType executorType,
            PersistenceExceptionTranslator exceptionTranslator) {
 
        super(sqlSessionFactory, executorType, exceptionTranslator);
 
        this.sqlSessionFactory = sqlSessionFactory;
        this.executorType = executorType;
        this.exceptionTranslator = exceptionTranslator;
        
        this.sqlSessionProxy = (SqlSession) newProxyInstance(
                SqlSessionFactory.class.getClassLoader(),
                new Class[] { SqlSession.class }, 
                new SqlSessionInterceptor());
 
        this.defaultTargetSqlSessionFactory = sqlSessionFactory;
    }
 
    @Override
    public SqlSessionFactory getSqlSessionFactory() {
 
        SqlSessionFactory targetSqlSessionFactory = targetSqlSessionFactorys.get(SqlSessionContentHolder.getContextType());
        if (targetSqlSessionFactory != null) {
            return targetSqlSessionFactory;
        } else if (defaultTargetSqlSessionFactory != null) {
            return defaultTargetSqlSessionFactory;
        } else {
            Assert.notNull(targetSqlSessionFactorys, "Property 'targetSqlSessionFactorys' or 'defaultTargetSqlSessionFactory' are required");
            Assert.notNull(defaultTargetSqlSessionFactory, "Property 'defaultTargetSqlSessionFactory' or 'targetSqlSessionFactorys' are required");
        }
        return this.sqlSessionFactory;
    }
 
    @Override
    public Configuration getConfiguration() {
        return this.getSqlSessionFactory().getConfiguration();
    }
 
    public ExecutorType getExecutorType() {
        return this.executorType;
    }
 
    public PersistenceExceptionTranslator getPersistenceExceptionTranslator() {
        return this.exceptionTranslator;
    }
 
    /**
     * {@inheritDoc}
     */
    public <T> T selectOne(String statement) {
        return this.sqlSessionProxy.<T> selectOne(statement);
    }
 
    /**
     * {@inheritDoc}
     */
    public <T> T selectOne(String statement, Object parameter) {
        return this.sqlSessionProxy.<T> selectOne(statement, parameter);
    }
 
    /**
     * {@inheritDoc}
     */
    public <K, V> Map<K, V> selectMap(String statement, String mapKey) {
        return this.sqlSessionProxy.<K, V> selectMap(statement, mapKey);
    }
 
    /**
     * {@inheritDoc}
     */
    public <K, V> Map<K, V> selectMap(String statement, Object parameter, String mapKey) {
        return this.sqlSessionProxy.<K, V> selectMap(statement, parameter, mapKey);
    }
 
    /**
     * {@inheritDoc}
     */
    public <K, V> Map<K, V> selectMap(String statement, Object parameter, String mapKey, RowBounds rowBounds) {
        return this.sqlSessionProxy.<K, V> selectMap(statement, parameter, mapKey, rowBounds);
    }
 
    /**
     * {@inheritDoc}
     */
    public <E> List<E> selectList(String statement) {
        return this.sqlSessionProxy.<E> selectList(statement);
    }
 
    /**
     * {@inheritDoc}
     */
    public <E> List<E> selectList(String statement, Object parameter) {
        return this.sqlSessionProxy.<E> selectList(statement, parameter);
    }
 
    /**
     * {@inheritDoc}
     */
    public <E> List<E> selectList(String statement, Object parameter, RowBounds rowBounds) {
        return this.sqlSessionProxy.<E> selectList(statement, parameter, rowBounds);
    }
 
    /**
     * {@inheritDoc}
     */
    public void select(String statement, ResultHandler handler) {
        this.sqlSessionProxy.select(statement, handler);
    }
 
    /**
     * {@inheritDoc}
     */
    public void select(String statement, Object parameter, ResultHandler handler) {
        this.sqlSessionProxy.select(statement, parameter, handler);
    }
 
    /**
     * {@inheritDoc}
     */
    public void select(String statement, Object parameter, RowBounds rowBounds, ResultHandler handler) {
        this.sqlSessionProxy.select(statement, parameter, rowBounds, handler);
    }
 
    /**
     * {@inheritDoc}
     */
    public int insert(String statement) {
        return this.sqlSessionProxy.insert(statement);
    }
 
    /**
     * {@inheritDoc}
     */
    public int insert(String statement, Object parameter) {
        return this.sqlSessionProxy.insert(statement, parameter);
    }
 
    /**
     * {@inheritDoc}
     */
    public int update(String statement) {
        return this.sqlSessionProxy.update(statement);
    }
 
    /**
     * {@inheritDoc}
     */
    public int update(String statement, Object parameter) {
        return this.sqlSessionProxy.update(statement, parameter);
    }
 
    /**
     * {@inheritDoc}
     */
    public int delete(String statement) {
        return this.sqlSessionProxy.delete(statement);
    }
 
    /**
     * {@inheritDoc}
     */
    public int delete(String statement, Object parameter) {
        return this.sqlSessionProxy.delete(statement, parameter);
    }
 
    /**
     * {@inheritDoc}
     */
    public <T> T getMapper(Class<T> type) {
        return getConfiguration().getMapper(type, this);
    }
 
    /**
     * {@inheritDoc}
     */
    public void commit() {
        throw new UnsupportedOperationException("Manual commit is not allowed over a Spring managed SqlSession");
    }
 
    /**
     * {@inheritDoc}
     */
    public void commit(boolean force) {
        throw new UnsupportedOperationException("Manual commit is not allowed over a Spring managed SqlSession");
    }
 
    /**
     * {@inheritDoc}
     */
    public void rollback() {
        throw new UnsupportedOperationException("Manual rollback is not allowed over a Spring managed SqlSession");
    }
 
    /**
     * {@inheritDoc}
     */
    public void rollback(boolean force) {
        throw new UnsupportedOperationException("Manual rollback is not allowed over a Spring managed SqlSession");
    }
 
    /**
     * {@inheritDoc}
     */
    public void close() {
        throw new UnsupportedOperationException("Manual close is not allowed over a Spring managed SqlSession");
    }
 
    /**
     * {@inheritDoc}
     */
    public void clearCache() {
        this.sqlSessionProxy.clearCache();
    }
 
    /**
     * {@inheritDoc}
     */
    public Connection getConnection() {
        return this.sqlSessionProxy.getConnection();
    }
 
    /**
     * {@inheritDoc}
     * @since 1.0.2
     */
    public List<BatchResult> flushStatements() {
        return this.sqlSessionProxy.flushStatements();
    }
 
    /**
     * Proxy needed to route MyBatis method calls to the proper SqlSession got from Spring's Transaction Manager It also
     * unwraps exceptions thrown by {@code Method#invoke(Object, Object...)} to pass a {@code PersistenceException} to
     * the {@code PersistenceExceptionTranslator}.
     */
    private class SqlSessionInterceptor implements InvocationHandler {
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
            final SqlSession sqlSession = getSqlSession(
                    DynamicSqlSessionTemplate.this.getSqlSessionFactory(),
                    DynamicSqlSessionTemplate.this.executorType, 
                    DynamicSqlSessionTemplate.this.exceptionTranslator);
            try {
                Object result = method.invoke(sqlSession, args);
                if (!isSqlSessionTransactional(sqlSession, DynamicSqlSessionTemplate.this.getSqlSessionFactory())) {
                    // force commit even on non-dirty sessions because some databases require
                    // a commit/rollback before calling close()
                    sqlSession.commit(true);
                }
                return result;
            } catch (Throwable t) {
                Throwable unwrapped = unwrapThrowable(t);
                if (DynamicSqlSessionTemplate.this.exceptionTranslator != null && unwrapped instanceof PersistenceException) {
                    Throwable translated = DynamicSqlSessionTemplate.this.exceptionTranslator
                        .translateExceptionIfPossible((PersistenceException) unwrapped);
                    if (translated != null) {
                        unwrapped = translated;
                    }
                }
                throw unwrapped;
            } finally {
                closeSqlSession(sqlSession, DynamicSqlSessionTemplate.this.getSqlSessionFactory());
            }
        }
    }
}
```

下面定义SqlSessionContentHolder类，使用ThreadLocal技术来记录当前线程中的sessionFactory的key。


2. SqlSessionContentHolder.java


SqlSessionContentHolder主要用于设置SqlSessionFactory的类型.

```java
package com.test.dlab.aop.core;

public abstract class SqlSessionContentHolder {

    public final static String SESSION_FACTORY_MASTER = "master";
    public final static String SESSION_FACTORY_SLAVE = "slave";
    
    private static final ThreadLocal<String> contextHolder = new ThreadLocal<String>();  
    
    public static void setContextType(String contextType) {  
        contextHolder.set(contextType);  
    }  
      
    public static String getContextType() {  
        return contextHolder.get();  
    }  
      
    public static void clearContextType() {  
        contextHolder.remove();  
    } 
}

```



## 定义数据源的AOP切面

定义数据源的AOP切面，通过该Dao的方法名判断是应该走读库还是写库。

### DynamicDataSourceAspect.java

```java
package com.test.dlab.aop.core;

import org.apache.commons.lang3.StringUtils;
import org.apache.log4j.Logger;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;

@Aspect
public class DynamicDataSourceAspect
{

    Logger log = Logger.getLogger(DynamicDataSourceAspect.class);
    
    private static final String[] slaveMethodPrefixList = new String[]
            { "quer", "find", "get", "check", "sum", "list", "is", "count", "load" };
    
    public void pointCut()
    {

    }

    public void before(JoinPoint jp)
    {
        String methodName = jp.getSignature().getName();
        
        log.info("当前方法名为 | MethodName: " + methodName);
        
        // dao方法查询走从库
        if(StringUtils.startsWithAny(methodName, slaveMethodPrefixList))
        //if (methodName.startsWith("query") || methodName.startsWith("get") || methodName.startsWith("count") || methodName.startsWith("list"))
        {
            log.info("设置sessionFactory：" + SqlSessionContentHolder.SESSION_FACTORY_SLAVE);
            SqlSessionContentHolder.setContextType(SqlSessionContentHolder.SESSION_FACTORY_SLAVE);
        }
        else
        {
            log.info("设置sessionFactory：" + SqlSessionContentHolder.SESSION_FACTORY_MASTER);
            SqlSessionContentHolder.setContextType(SqlSessionContentHolder.SESSION_FACTORY_MASTER);
        }
    }
}
```


## 修改配置文件

### 配置db.properties

在db.properties配置文件中增加主库和从库的信息，这里master为主库，slave为从库，具体内容如下：

```xml
jdbc.master.driver=com.mysql.jdbc.Driver
jdbc.master.url=jdbc:mysql://10.88.184.156:3306/resource_manage?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true
jdbc.master.username=root
jdbc.master.password=test

jdbc.slave01.driver=com.mysql.jdbc.Driver
jdbc.slave01.url=jdbc:mysql://127.0.0.1:3306/resource_manage?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true
jdbc.slave01.username=root
jdbc.slave01.password=123456
```


### 添加主从数据库连接池

修改applicationContext.xml，masterDataSource和slaveDataSource。

```xml
<!-- 主从数据库配置  part1 start -->
<!-- 配置连接池 -->
<bean id="masterDataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource" destroy-method="close">
    <!-- 数据库驱动 -->
    <property name="driverClass" value="${jdbc.master.driver}" />
    <!-- 相应驱动的jdbcUrl -->
    <property name="jdbcUrl" value="${jdbc.master.url}" />
    <!-- 数据库的用户名 -->
    <property name="user" value="${jdbc.master.username}" />
    <!-- 数据库的密码 -->
    <property name="password" value="${jdbc.master.password}" />
    
    <!--初始化时获取三个连接，取值应在minPoolSize与maxPoolSize之间。Default: 3 -->
    <property name="initialPoolSize" value="${jdbc.initialPoolSize}"/>
    <!--连接池中保留的最大连接数。Default: 15 -->
    <property name="maxPoolSize" value="${jdbc.maxPoolSize}"/>
    <!--连接池中保留的最小连接数。 -->
    <property name="minPoolSize" value="${jdbc.minPoolSize}"/>
    <!--最大空闲时间,1800秒内未使用则连接被丢弃。若为0则永不丢弃。Default: 0 -->
    <property name="maxIdleTime" value="${jdbc.maxIdleTime}" />
    <!--当连接池中的连接耗尽的时候c3p0一次同时获取的连接数。Default: 3 -->
    <property name="acquireIncrement" value="${jdbc.acquireIncrement}" />
    <!--每0秒检查所有连接池中的空闲连接。Default: 0 -->
    <property name="idleConnectionTestPeriod" value="${jdbc.idleConnectionTestPeriod}" />
    <!--定义在从数据库获取新连接失败后重复尝试的次数。Default: 30 -->
    <property name="acquireRetryAttempts" value="${jdbc.acquireRetryAttempts}" />
    <!--如果设为true那么在取得连接的同时将校验连接的有效性。Default: false -->
    <property name="testConnectionOnCheckin" value="true"/>
    <property name="preferredTestQuery" value="SELECT CURRENT_DATE"/>
</bean>
<!-- 配置连接池 -->
<bean id="slaveDataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"  destroy-method="close">
    <!-- 数据库驱动 -->
    <property name="driverClass" value="${jdbc.slave01.driver}" />
    <!-- 相应驱动的jdbcUrl -->
    <property name="jdbcUrl" value="${jdbc.slave01.url}" />
    <!-- 数据库的用户名 -->
    <property name="user" value="${jdbc.slave01.username}" />
    <!-- 数据库的密码 -->
    <property name="password" value="${jdbc.slave01.password}" />
    
    <!--初始化时获取三个连接，取值应在minPoolSize与maxPoolSize之间。Default: 3 -->
    <property name="initialPoolSize" value="${jdbc.initialPoolSize}"/>
    <!--连接池中保留的最大连接数。Default: 15 -->
    <property name="maxPoolSize" value="${jdbc.maxPoolSize}"/>
    <!--连接池中保留的最小连接数。 -->
    <property name="minPoolSize" value="${jdbc.minPoolSize}"/>
    <!--最大空闲时间,1800秒内未使用则连接被丢弃。若为0则永不丢弃。Default: 0 -->
    <property name="maxIdleTime" value="${jdbc.maxIdleTime}" />
    <!--当连接池中的连接耗尽的时候c3p0一次同时获取的连接数。Default: 3 -->
    <property name="acquireIncrement" value="${jdbc.acquireIncrement}" />
    <!--每0秒检查所有连接池中的空闲连接。Default: 0 -->
    <property name="idleConnectionTestPeriod" value="${jdbc.idleConnectionTestPeriod}" />
    <!--定义在从数据库获取新连接失败后重复尝试的次数。Default: 30 -->
    <property name="acquireRetryAttempts" value="${jdbc.acquireRetryAttempts}" />
    <!--如果设为true那么在取得连接的同时将校验连接的有效性。Default: false -->
    <property name="testConnectionOnCheckin" value="true"/>
    <property name="preferredTestQuery" value="SELECT CURRENT_DATE"/>
</bean>
<!-- 主从数据库配置 part1 end -->
```

### 配置两个sessionFactory

两个sessionFactory内容大致一样，唯一不同的就是dataSource数据源。

```xml
<!-- 配置 mybatis 的 sqlsession 的工厂 ， 设置mapper location-->
<bean id="masterSqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">  
    <property name="configLocation" value="classpath:config/mybatis.xml" />  
    <property name="dataSource" ref="masterDataSource" />  
    
    <property name="mapperLocations">
        <list>
            <value>classpath:com/test/dlab/mappers/***/*Mapper.xml</value>
            <value>classpath:com/test/dlab/mappers/***/***/*Mapper.xml</value>
        </list>
    </property>
    <property name="typeAliasesPackage" value="com.test.dlab.entity" />
    <!-- PageHelper 分页插件 -->
    <property name="plugins">
        <array>
          <bean class="com.github.pagehelper.PageHelper">
            <property name="properties">
              <value>
                dialect=mysql
                pageSizeZero=true
                reasonable=false
                <!-- supportMethodsArguments=false -->
                returnPageInfo=always 
              </value>
            </property>
          </bean>
        </array>            
    </property>
</bean>  

<bean id="slaveSqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">  
    <property name="configLocation" value="classpath:config/mybatis.xml" />  
    <property name="dataSource" ref="slaveDataSource" />  
    
    <property name="mapperLocations">
        <list>
            <value>classpath:com/test/dlab/mappers/***/*Mapper.xml</value>
            <value>classpath:com/test/dlab/mappers/***/***/*Mapper.xml</value>
        </list>
    </property>
    <property name="typeAliasesPackage" value="com.test.dlab.entity" />
    <!-- PageHelper 分页插件 -->
    <property name="plugins">
        <array>
          <bean class="com.github.pagehelper.PageHelper">
            <property name="properties">
              <value>
                dialect=mysql
                pageSizeZero=true
                reasonable=false
                <!-- supportMethodsArguments=false -->
                returnPageInfo=always 
              </value>
            </property>
          </bean>
        </array>            
    </property>
</bean>  
    
<!-- 两个SqlSessionFactory使用同一个SqlSessionTemplate配置 -->
<bean id="MasterAndSlaveSqlSessionTemplate" class="com.test.dlab.aop.core.DynamicSqlSessionTemplate">
    <constructor-arg index="0" ref="masterSqlSessionFactory" />
    <property name="targetSqlSessionFactorys">
        <map>  
            <entry value-ref="masterSqlSessionFactory" key="master"/>  
            <entry value-ref="slaveSqlSessionFactory" key="slave"/>  
        </map>  
    </property>
</bean>
```


### 配置mapper

这里采用sqlSessionTemplateBeanName进行mapper的配置，与之前sqlSessionFactoryBeanName有点区别。

```xml
 <!-- 一次性配置所有Mapper,替代MapperScannerConfigurer,指定要扫描包： 多个包用逗号隔开   -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.test.dlab.dao" />
    <property name="sqlSessionTemplateBeanName" value="MasterAndSlaveSqlSessionTemplate" />
</bean>
```


### 引入jta/atomikos实现分布式事务管理

由于Service层的事务不能更改，所以当我们引入多个库的时候，需要引入atomikos来进行分布式事务管理。

首先引入以下六个jar包

- atomikos-util-4.0.4.jar
- jta-1.1.jar 
- transactions-4.0.4.jar
- transactions-api-4.0.4.jar
- transactions-jdbc-4.0.4.jar
- transactions-jta-4.0.4.jar

然后配置全局的事务管理，并声明注解事务即可。

```java
<!-- 配置事务管理器bean -->
<bean id="atomikosTransactionManager" class="com.atomikos.icatch.jta.UserTransactionManager" init-method="init" destroy-method="close">  
    <property name="forceShutdown">  
        <value>true</value>  
    </property>  
</bean>  

<bean id="atomikosUserTransaction" class="com.atomikos.icatch.jta.UserTransactionImp">  
    <property name="transactionTimeout" value="300" />  
</bean>  
<!-- spring 事务管理器 -->    
<bean id="springTransactionManager" class="org.springframework.transaction.jta.JtaTransactionManager">  
    <property name="transactionManager" ref="atomikosTransactionManager" />  
    <property name="userTransaction" ref="atomikosUserTransaction" />  
    <!-- 必须设置，否则程序出现异常 JtaTransactionManager does not support custom isolation levels by default -->  
    <property name="allowCustomIsolationLevels" value="true"/>   
</bean>  
<!-- 声明注解事务 -->
<tx:annotation-driven transaction-manager="springTransactionManager"/>
```
    

### 配置事务属性

```xml
<!-- 配置事务属性 -->
<tx:advice id="txAdvice" transaction-manager="springTransactionManager">
    <tx:attributes>
        <!-- 增删改查方法  配置事务 -->        
        <tx:method name="save*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="add*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="create*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="insert*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="update*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="merge*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="del*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="remove*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="put*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="use*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="batch*" propagation="REQUIRED" rollback-for="java.lang.Exception"/>
        <tx:method name="bind*" propagation="REQUIRED" rollback-for="java.lang.Exception"/> 

        <!-- 查询方法 配置只读属性 -->
        <tx:method name="get*" propagation="REQUIRED" read-only="true"/>
        <tx:method name="count*" propagation="REQUIRED" read-only="true" />
        <tx:method name="find*" propagation="REQUIRED" read-only="true" />
        <tx:method name="list*" propagation="REQUIRED" read-only="true" />    
        <tx:method name="*" propagation="REQUIRED" />    
    </tx:attributes>    
</tx:advice>   

```

### 配置事务切面

为了让之前配置的Aspect作用域Dao方法上，我们需要在Dao层面加入一些配置，使数据源分库功能作用于Dao方法上，实现动态的读写分离。

```xml
<!-- 配置数据库注解aop -->
<!--     <bean id="dynamicDataSourceAspect" class="com.test.dlab.aop.spring.DynamicDataSourceAspect" />
<bean class="com.test.dlab.aop.aspect.DataSourceAspect2" id="dynamicDataSourceAspect" /> -->

<bean class="com.test.dlab.aop.core.DynamicDataSourceAspect" id="dynamicDataSourceAspect" />
<aop:config >
    <aop:pointcut id="txPointcut" expression="execution(* com.test.dlab.service..*.*(..))" />
    <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
    
    <!-- 将切面应用到自定义的切面处理器上，-9999保证该切面优先级最高执行 -->
     <aop:aspect ref="dynamicDataSourceAspect" order="-9999">
        <aop:pointcut id="tx" expression="execution(* com.test.dlab.dao..*.*(..))"/>
        <aop:before method="before" pointcut-ref="tx" />
    </aop:aspect> 
</aop:config>   
```

OK，启动项目，访问一切ok！


## 参考文献

1. [关于Spring3 + Mybatis3整合时，多数据源动态切换的问题](http://blog.csdn.net/zl3450341/article/details/20150687)
2. [Spring + Mybatis 项目实现动态切换数据源](https://www.cnblogs.com/FlyHeLanMan/p/6744171.html)