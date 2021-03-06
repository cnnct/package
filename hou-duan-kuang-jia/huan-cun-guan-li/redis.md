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

    <bean id="RedisFactory" class="com.cnnct.basic.redis.RedisFactory" init-method="init" lazy-init="false"/>
```

#### 特定jar包

jedis-2.4.2.jar

spring-data-redis-1.3.0.RELEASE.jar

#### 启动redis

首先，搭建好redis服务器，然后将context.xml中的"&lt;import resource="../../baseconfig/redis.xml"/&gt;"解除注释就可以使用redis了，通过RedisUtil工具类对redis进行操作。session默认是缓存在ehcache中的，如果想切换到redis中，将para.properties中的"sessionRedis=false"改为"sessionRedis=true"即可。

注：redis中存放的对象必须序列化。

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

    <bean id="RedisFactory" class="com.cnnct.basic.redis.RedisFactory" init-method="init" lazy-init="false"/>
```

#### redis.properties

下面属性一一对应redis.xml中的属性。

```
maxTotal=300
maxIdle=250
maxWaitMillis=3000
testOnBorrow=true
testOnReturn=true

hostName=172.16.200.20
port=6379
usePool=true
```

#### RedisFactory.java

这是Redis工厂类，所有redis在这里配置。

#### RedisUtil.java

这是Redis工具类，调用Redis，做读取、修改、删除等操作。

#### Redis启用禁用：

在para.properties文件中配置，true启用，false禁用：

```
sessionRedis=false
```



