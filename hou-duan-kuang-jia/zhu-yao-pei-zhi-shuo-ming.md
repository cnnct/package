从web.xml中的dispatcher等配置开始，依次按目录层次所包含的主要配置文件进行说明各配置文件的作用以及主要配置项、加载顺序，包括core包中，和src包中

#### web.xml

配置Spring

```
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:config/spring/context.xml</param-value>
</context-param>
```

配置SpringMVC

```
<servlet>
  <servlet-name>springmvc</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:config/spring/context-mvc.xml</param-value>
  </init-param>
  <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
  <servlet-name>springmvc</servlet-name>
  <url-pattern>/</url-pattern>
</servlet-mapping>
```

配置FreeMarker

```
<servlet>
  <servlet-name>freemarker</servlet-name>
  <servlet-class>freemarker.ext.servlet.FreemarkerServlet</servlet-class>
  <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
  <servlet-name>freemarker</servlet-name>
  <url-pattern>*.ftl</url-pattern>
</servlet-mapping>
```

#### context.xml

../../baseconfig/spring/spring-applicationCore.xml、../../baseconfig/spring/applicationContext-service.xml为Jar包内标签所用。

../../baseconfig/redis.xml为Redis缓存所用。

applicationContext-dao.xml、applicationContext-service.xml为数据源和事务所用。

freemarker.xml为FreeMarker框架所用。

```
<!-- 导入jar包中的配置文件,不可删除 -->
<import resource="../../baseconfig/spring/spring-applicationCore.xml"/>
<import resource="../../baseconfig/spring/applicationContext-service.xml"/>
<!--     <import resource="../../baseconfig/redis.xml"/> -->

<!-- 开发人员可见可修改配置文件，【1.3版本开始，将dao.xml和transation.xml合并到dao.xml中】 -->
<import resource="applicationContext-dao.xml"/>
<import resource="applicationContext-service.xml"/>
<import resource="freemarker.xml"/>
```

#### context-mvc.xml

../../baseconfig/spring/springmvc-servlet.xml为Jar包内标签所用。

springmvc-servlet.xml为开发人员配置controller等所用。

```
<!-- 导入jar包中的配置文件,不可删除 -->
<import resource="../../baseconfig/spring/springmvc-servlet.xml"/>

<!-- 开发人员可见可修改配置文件 -->
<import resource="springmvc-servlet.xml"/>
```

#### spring-applicationCore.xml

```
<!-- 配置Jar内标签所用的*Mapper*.java和*Mapper*.xml -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
  <!-- 配置扫描包的路径
    如果要扫描多个包，中间使用半角逗号分隔
    要求mapper.xml和mapper.java同名且在同一个目录 
  -->
  <property name="basePackage" value="increator.core.mapper"/>
  <!-- 使用sqlSessionFactoryBeanName -->
  <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
</bean>
    
<!--     扫描标签库的service和tag注解组件 -->
<context:component-scan base-package="increator.core.service,com.cnt,increator.core.tag"/>
   
<!-- 数据库工具类,用于获取数据库类型 -->
<bean id="databaseUtil" class= "increator.core.util.DatabaseUtil" init-method="init" lazy-init="false" />
    
<!-- 用于Mybatis热部署,开发环境时使用,生产环境通过修改para.properties中的mybatis_refresh为false禁用 -->
<bean id="mapperRefreshStart" class="increator.base.MapperRefreshStart" init-method="init" />
```

#### redis.xml

```
<!-- Redis连接池的配置 -->
<bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig" >
  <!-- 最大连接数, 默认8个 -->
  <property name="maxTotal" value="${maxTotal}"/>
  <!-- 最大空闲连接数, 默认8个-->
  <property name="maxIdle" value="${maxIdle}"/>
  <!-- 最大等待毫秒数(如果设置为阻塞时BlockWhenExhausted),如果超时就抛异常, 小于零:阻塞不确定的时间,  默认-1 -->
  <property name="maxWaitMillis" value="${maxWaitMillis}"/>
  <!-- 在获取连接的时候检查有效性, 默认false -->
  <property name="testOnBorrow" value="${testOnBorrow}"/>
  <!-- 在返回连接的时候检查有效性, 默认false -->
  <property name="testOnReturn" value="${testOnReturn}"/>
</bean>
<!-- redis连接配置，依次为主机ip，端口，是否使用池，(usePool=true时)redis的池配置 -->
<bean id="jedisConnFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
  <property name="hostName" value="${hostName}"/>
  <property name="port" value="${port}"/>
  <property name="usePool" value="${usePool}"/>
  <property name="poolConfig" ref="jedisPoolConfig"/>
</bean>
<!-- redis模板配置 -->
<bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
  <property name="connectionFactory" ref="jedisConnFactory"/>
  <property name="keySerializer">
    <bean class="org.springframework.data.redis.serializer.StringRedisSerializer"/>
  </property>
  <property name="valueSerializer">
    <bean class="org.springframework.data.redis.serializer.JdkSerializationRedisSerializer"/>
  </property>
</bean>
    
<!-- redis工厂 -->
<bean id="RedisFactory" class="com.cnnct.basic.redis.RedisFactory" init-method="init" lazy-init="false"/>
```



