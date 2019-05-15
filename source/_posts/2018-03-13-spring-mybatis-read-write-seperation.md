---
layout: post
date: 2018-03-13 23:22
title: "Spring+Mybatis+Mysql数据库读写分离"
categories: Java
tag: 
    - 读写分离
    - 主从分离
    - Spring
    - MySql
comment: true
---

## 背景介绍

通常，在与数据库进行交互时，对数据库的操作都是“读多写少”，一方面，对数据库读取数据的压力比较大；另一方面，如果数据库分布在国内，那么在国外访问项目的时候，如果查询的接口较多，那么直接访问国内的数据库会大大的降低访问性能。因此，为了提升数据访问速度，缓解数据库的压力，我们可以在国外的服务器也安装一个mysql，部署一个项目，两个mysql进行主从配置，那么对于接口就需要采用读写分离策略，其基本思想是：将数据库分为主库和从库，主库只有一个，从库可有多个，主库主要负责写入数据，而从库则负责读取数据。

<!-- more -->

要求：

1. 主从一致：读库和写库的数据一致；
2. 写走主库：写数据必须写到写库；
3. 读走从库：读数据必须到读库；

本文针对读写分离使用的方法是基于应用层实现，对原有代码的改动量较小，只是对配置文件进行了修改，下面来看具体实现。


## 原理

SSM框架将后台划分成了Dao、Service、Mapper层，读写分离的原理是在进入Service/Dao（具体哪一层，看配置项）之前，使用AOP来判断请求是前往写库还是读库，判断依据可以根据方法名判断，比如说以query、find、get等开头的就走读库，其他的走写库。

## 实现

### 定义动态数据源

实现通过集成Spring提供的AbstractRoutingDataSource，只需要实现determineCurrentLookupKey方法即可，由于DynamicDataSource是单例的，线程不安全的，所以采用ThreadLocal保证线程安全，由DynamicDataSourceHolder完成。

1.DynamicDataSource.java

```java
package com.test.dlab.aop.aspect;

import org.springframework.jdbc.datasource.lookup.AbstractRoutingDataSource;

/**
 *
 * 定义动态数据源，实现通过集成Spring提供的AbstractRoutingDataSource，
 * 只需要实现determineCurrentLookupKey方法即可
 * 由于DynamicDataSource是单例的，线程不安全的，所以采用ThreadLocal保证线程安全，
 * 由DynamicDataSourceHolder完成。
 * 
 * @author liudiwei 2018年3月12日
 *
 */
public class DynamicDataSource extends AbstractRoutingDataSource
{
    @Override
    protected Object determineCurrentLookupKey()
    {
        // 使用DynamicDataSourceHolder保证线程安全，并且得到当前线程中的数据源key
        return DynamicDataSourceHolder.getDataSourceKey();
    }
}
```

下面定义DynamicDataSourceHolder类，使用ThreadLocal技术来记录当前线程中的数据源的key。

2.DynamicDataSourceHolder.java


```java
package com.test.dlab.aop.aspect;

import org.apache.log4j.Logger;

/**
 * 
 * @author liudiwei 2018年3月12日 使用ThreadLocal技术来记录当前线程中的数据源的key
 *
 */
public class DynamicDataSourceHolder
{
    private static Logger log = Logger.getLogger(DynamicDataSource.class);

    // 写库对应的数据源key
    private static final String MASTER = "master";
    // 读库对应的数据源key
    private static final String SLAVE = "slave";

    // 使用ThreadLocal记录当前线程的数据源key
    private static final ThreadLocal<String> holder = new ThreadLocal<String>();

    /**
     * 设置数据源key
     * 
     * @param key
     */
    public static void setDataSourceKey(String key)
    {
        holder.set(key);
    }

    /**
     * 获取数据源key
     * 
     * @return
     */
    public static String getDataSourceKey()
    {
        return holder.get();
    }

    /**
     * 标记写库
     */
    public static void markAsMaster()
    {
        setDataSourceKey(MASTER);
    }

    /**
     * 标记读库
     */
    public static void markAsSlave()
    {
        setDataSourceKey(SLAVE);
    }

    public static void clearDataSource()
    {
        log.info("移除clearDataSource");
        holder.remove();
    }
}

```

## 定义数据源的AOP切面

定义数据源的AOP切面，通过该Service的方法名判断是应该走读库还是写库。

### DataSourceAspect.java

```java
package com.test.dlab.aop.aspect;

import org.apache.log4j.Logger;
import org.aspectj.lang.JoinPoint;

/**
 * 定义数据源的AOP切面，通过该Service的方法名判断是应该走读库还是写库
 * 
 * @author liudiwei 2018年3月12日
 *
 */
public class DataSourceAspect
{

    private static final Logger log = Logger.getLogger(DataSourceAspect.class);

    /**
     * 在进入Service方法之前执行
     * 
     * @param point
     *            切面对象
     */
    public void before(JoinPoint point)
    {
        // 获取到当前执行的方法名
        String methodName = point.getSignature().getName();
        if (isSlave(methodName))
        {
            // 标记为读库
            DynamicDataSourceHolder.markAsSlave();
        }
        else
        {
            // 标记为写库
            DynamicDataSourceHolder.markAsMaster();
        }
    }

    /**
     * 判断是否为读库
     * 
     * @param methodName
     * @return
     * 
     */
    private Boolean isSlave(String methodName)
    {
        log.info("根据Service方法名前缀判断是否走从库.");
        return org.apache.commons.lang3.StringUtils.startsWithAny(methodName, "query", "quer", "find", "get");
    }
}

```


### 优化后的Aspect 

DataSourceAspect2.java

```java
package com.test.dlab.aop.aspect;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

import org.apache.commons.lang3.StringUtils;
import org.apache.log4j.Logger;
import org.aspectj.lang.JoinPoint;
import org.springframework.transaction.interceptor.NameMatchTransactionAttributeSource;
import org.springframework.transaction.interceptor.TransactionAttribute;
import org.springframework.transaction.interceptor.TransactionAttributeSource;
import org.springframework.transaction.interceptor.TransactionInterceptor;
import org.springframework.util.PatternMatchUtils;
import org.springframework.util.ReflectionUtils;

import java.lang.reflect.Field;

/**
 * 定义数据源的AOP切面，通过该Dao的方法名判断是应该走读库还是写库
 * 
 * @author liudiwei 2018年3月12日
 *
 */
public class DataSourceAspect2
{
    private static final Logger log = Logger.getLogger(DataSourceAspect.class);

    private List<String> slaveMethodPattern = new ArrayList<String>();

    private static final String[] defaultSlaveMethodStart = new String[] { "quer", "find", "get" };

    private String[] slaveMethodStart;

    /**
     * 读取事务管理中的策略
     * 
     * @param txAdvice
     * @throws Exception
     */
    @SuppressWarnings("unchecked")
    public void setTxAdvice(TransactionInterceptor txAdvice) throws Exception
    {
        if (txAdvice == null)
        {
            log.info("没有配置事务管理策略");
            return;
        }
        // 从txAdvice获取到策略配置信息
        TransactionAttributeSource transactionAttributeSource = txAdvice.getTransactionAttributeSource();
        if (!(transactionAttributeSource instanceof NameMatchTransactionAttributeSource))
        {
            return;
        }
        // 使用反射技术获取到NameMatchTransactionAttributeSource对象中的nameMap属性值
        NameMatchTransactionAttributeSource matchTransactionAttributeSource = (NameMatchTransactionAttributeSource) transactionAttributeSource;
        Field nameMapField = ReflectionUtils.findField(NameMatchTransactionAttributeSource.class, "nameMap");

        log.info("nameMapField AAAAA:" + nameMapField);

        nameMapField.setAccessible(true); // 设置该字段可访问
        // 获取nameMap的值
        Map<String, TransactionAttribute> map = (Map<String, TransactionAttribute>) nameMapField
                .get(matchTransactionAttributeSource);
        // 遍历nameMap
        for (Map.Entry<String, TransactionAttribute> entry : map.entrySet())
        {
            log.info("entity结果:" + entry.toString());
            // 判断之后定义了ReadOnly的策略才加入到slaveMethodPattern
            if (!entry.getValue().isReadOnly())
            {
                continue;
            }
            slaveMethodPattern.add(entry.getKey());
        }
    }

    /**
     * 在进入Service方法之前执行
     * 
     * @param point
     *            切面对象
     */
    public void before(JoinPoint point)
    {
        // 获取到当前执行的方法名
        String methodName = point.getSignature().getName();
        log.info("方法名称：" + methodName);
        boolean isSlave = false;
        if (slaveMethodPattern.isEmpty())
        {
            log.info("当前Spring容器中没有配置事务策略，采用方法名匹配方式");
            isSlave = isSlave(methodName);
        }
        else
        {
            log.info("使用策略规则匹配");
            for (String mappedName : slaveMethodPattern)
            {
                if (isMatch(methodName, mappedName))
                {
                    isSlave = true;
                    break;
                }
            }
        }
        if (isSlave)
        {
            log.info("标记为读库");
            DynamicDataSourceHolder.markAsSlave();
        }
        else
        {
            log.info("标记为写库");
            DynamicDataSourceHolder.markAsMaster();
        }
    }

    public void after(JoinPoint point)
    {
        DynamicDataSourceHolder.clearDataSource();
    }

    /**
     * 判断是否为读库
     * 
     * @param methodName
     * @return
     */
    private Boolean isSlave(String methodName)
    {
        log.info("根据Dao方法名前缀判断是否走从库.");
        return StringUtils.startsWithAny(methodName, getSlaveMethodStart());
    }

    /**
     * 通配符匹配
     * 
     * Return if the given method name matches the mapped name.
     * <p>
     * The default implementation checks for "xxx*", "*xxx" and "*xxx*" matches,
     * as well as direct equality. Can be overridden in subclasses.
     * 
     * @param methodName
     *            the method name of the class
     * @param mappedName
     *            the name in the descriptor
     * @return if the names match
     * @see org.springframework.util.PatternMatchUtils#simpleMatch(String,
     *      String)
     */
    protected boolean isMatch(String methodName, String mappedName)
    {
        return PatternMatchUtils.simpleMatch(mappedName, methodName);
    }

    /**
     * 用户指定slave的方法名前缀
     * 
     * @param slaveMethodStart
     */
    public void setSlaveMethodStart(String[] slaveMethodStart)
    {
        this.slaveMethodStart = slaveMethodStart;
    }

    public String[] getSlaveMethodStart()
    {
        if (this.slaveMethodStart == null)
        {
            // 没有指定，使用默认
            return defaultSlaveMethodStart;
        }
        return slaveMethodStart;
    }

}
```



## 修改配置文件

### 配置db.properties

在db.properties配置文件中增加主库和从库的信息，这里master为主库，slave为从库，具体内容如下：

```xml
jdbc.master.driver=com.mysql.jdbc.Driver
jdbc.master.url=jdbc:mysql://127.0.0.1:3307/resource_test?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true
jdbc.master.username=root
jdbc.master.password=123456

jdbc.slave01.driver=com.mysql.jdbc.Driver
jdbc.slave01.url=jdbc:mysql://127.0.0.1:3306/resource_test?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true
jdbc.slave01.username=root
jdbc.slave01.password=123456
```
### 修改applicationContext.xml

1)添加主从数据库连接池masterDataSource和slave01DataSource。

```xml
<!-- 配置连接池 -->
<bean id="masterDataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"    destroy-method="close">
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
<bean id="slave01DataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"    destroy-method="close">
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
```

2)定义DataSource

```xml
<!-- 定义数据源，使用自己实现的数据源 -->
<bean id="dataSource" class="com.test.dlab.aop.aspect.DynamicDataSource">
    <!-- 设置多个数据源 -->
    <property name="targetDataSources">
        <map key-type="java.lang.String">
            <!-- 这个key需要和程序中的key一致 -->
            <entry key="master" value-ref="masterDataSource" />
            <entry key="slave" value-ref="slave01DataSource" />
        </map>
    </property>
    <!-- 设置默认的数据源，这里默认走写库 -->
    <property name="defaultTargetDataSource" ref="masterDataSource" />
</bean>
```

3)配置事务管理器

```xml
<!-- 配置事务管理器bean -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

4)配置事务属性

```xml
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!--定义查询方法都是只读的 -->
        <tx:method name="query*" read-only="true" />
        <tx:method name="find*" read-only="true" />
        <tx:method name="get*" read-only="true" />
        <!-- 主库执行操作，事务传播行为定义为默认行为 -->
        <tx:method name="save*" propagation="REQUIRED" />
        <tx:method name="update*" propagation="REQUIRED" />
        <tx:method name="delete*" propagation="REQUIRED" />
        <!-- 主库执行操作，事务传播行为定义为默认行为 -->
        <!--其他方法使用默认事务策略 -->
        <tx:method name="*" />     
    </tx:attributes>    
</tx:advice>
```

5)配置事务切面

```xml
<!-- 主从数据库配置  part2 start -->
<!-- 配置数据库注解aop -->
<bean class="com.test.dlab.aop.aspect.DataSourceAspect2" id="dataSourceAspect" />
<!-- 配置xml事务 切面 -->
<aop:config expose-proxy="true">
    <aop:pointcut id="txPointcut" expression="execution(* com.test.dlab.service..*.*(..))" />
    <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>

    <!-- 将切面应用到自定义的切面处理器上，-9999保证该切面优先级最高执行 -->
    <aop:aspect ref="dataSourceAspect" order="-9999">
        <aop:pointcut id="tx" expression="execution(* com.test.dlab.dao..*.*(..))"/>
        <aop:before method="before" pointcut-ref="tx" />
    </aop:aspect>
</aop:config>
<!-- 主从数据库配置  part2 end -->
```

OK，启动项目，访问一切ok！

## 注意事项

1.由于实现的是dao层的读写分离，因此在配置aop的时候，应该去掉 `proxy-target-class="true"`:

```xml
<aop:aspectj-autoproxy />  
```

2.由于service层配置了事务，所以为了不影响dao层的主从分离，在配置service事务属性的时候，不能添加下列语句，否则数据库在service层进入事务之后，无法实现dao层的主从分离。

```xml
<tx:method name="*" propagation="REQUIRED" /> 
```



