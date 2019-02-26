# ！！！是否有更新！！！mybatis二级缓存是放这个章节下级，还是单独起章节？？怎么合适就怎么弄

此处理可写一下总体ehcache配置，特别是负载配置

# 一、ehcache

#### ehcache.xml

```
    <!-- ================负载配置开始=============== -->
    <!-- 
    除配置本段两个Factory节点外，还需要在各子缓存的结点中增加cacheEventListenerFactory，详见sysPara示例配置部分
    共分为①、②、③，共三处配置

    负载配置①：配置对方缓存服务信息
    peerDiscovery：atutomatic自动，manual手动，本例使用manual
    rmiUrls：对方缓存服务url，本例中只是以sysPara为例，若是多个url时，中间用|竖线分隔即可
    timeToLive 搜索某个网段上的缓存：0是限制在同一个服务器，1是限制在同一个子网，32是限制在同一个网站，
        64是限制在同一个region，128是同一块大陆，还有个255
    !!!!!特别注意，我在本机测试两个tomcat时，一直不生效，后来加上timeToLive=0，即生效
    -->       
<!--     <cacheManagerPeerProviderFactory  -->
<!--         class="net.sf.ehcache.distribution.RMICacheManagerPeerProviderFactory" -->
<!--         properties="peerDiscovery=manual,timeToLive=0,rmiUrls=//127.0.0.1:40001/sysPara"/> -->

    <!-- 负载配置②：配置本地ip，端口
        hostName：主机名或者ip
        socketTimeoutMillis:同步超时时间
     -->
<!--     <cacheManagerPeerListenerFactory  -->
<!--            class="net.sf.ehcache.distribution.RMICacheManagerPeerListenerFactory"  -->
<!--            properties="hostName=127.0.0.1,port=40002,socketTimeoutMillis=2000"/>  -->
    <!-- ================负载配置结束=============== -->

        <!-- syspara缓存  ,暂定永不失效  -->
       <cache name="sysPara"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           eternal="true"
           timeToIdleSeconds="0"
           timeToLiveSeconds="0"
           memoryStoreEvictionPolicy="LRU"
           transactionalMode="off"
    >
        <searchable keys="true"/> <!--可以根据Key进行查询，查询的Attribute就是keys-->
        <!-- 负载配置③：配置RMI，非负载配置可注释 -->
<!--         <cacheEventListenerFactory class="net.sf.ehcache.distribution.RMICacheReplicatorFactory"/> -->
      </cache>
```

# 二、redis

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

