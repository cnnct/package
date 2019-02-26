# ！！！！需要更新！！！

# ！！！重点说明优化：二级缓存、事务过滤查询方法等，张涛提出的优化！！！

从web.xml中的dispatcher等配置开始，依次按目录层次所包含的主要配置文件进行说明各配置文件的作用以及主要配置项、加载顺序，包括core包中，和src包中

#### 加载过程：

tomcat启动后，加载web.xml文件，由于web.xml中先配置了spring配置文件，所以先加载context.xml，由此加载： ../../baseconfig/spring/spring-applicationCore.xml ../../baseconfig/spring/applicationContext-service.xml ../../baseconfig/redis.xml applicationContext-dao.xml applicationContext-service.xml freemarker.xml

Spring初始化数据源和事务以及所有bean,初始化FreeMarker全局配置。 然后加载context-mvc.xml，由此加载： ../../baseconfig/spring/springmvc-servlet.xml springmvc-servlet.xml

Spring初始化controller、拦截器等。

最后项目启动。

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

#### Jar包内spring-applicationCore.xml

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

#### Jar包内redis.xml

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

#### applicationContext-dao.xml

```
<!-- spring容器只有第一个context:property-placeholder会生效,后面的都会被忽略,所以加载properties配置文件都写在这里,以","号分隔 -->
<context:property-placeholder location="classpath:config/parameter/db.properties,classpath:config/parameter/redis.properties"/>


<!--============= datasourece配置开始================= -->
<!-- 数据源1 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <property name="driverClassName" value="${jdbc.driver}" />
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
        <!-- 配置初始化大小、最小、最大 -->
        <property name="initialSize" value="5" />
        <property name="minIdle" value="1" />
        <property name="maxActive" value="20" />
        <!-- 配置获取连接等待超时的时间 -->
        <property name="maxWait" value="60000" />
        <!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->
        <property name="timeBetweenEvictionRunsMillis" value="60000" />
        <!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->
        <property name="minEvictableIdleTimeMillis" value="300000" />
        <property name="validationQuery" value="#{'#{jdbc.driver}'=='com.mysql.jdbc.Driver'?'SELECT 1':'SELECT 1 FROM DUAL'}" />
        <property name="testWhileIdle" value="true" />
        <property name="testOnBorrow" value="false" />
        <property name="testOnReturn" value="false" />
        <!-- 打开PSCache，并且指定每个连接上PSCache的大小 -->
        <property name="poolPreparedStatements" value="true" />
        <property name="maxPoolPreparedStatementPerConnectionSize" value="20" />
    </bean>
    <!-- 数据源2 -->
<!--     <bean id="dataSource2" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close"> -->
<!--         <property name="driverClassName" value="${jdbc.driver2}" /> -->
<!--         <property name="url" value="${jdbc.url2}" /> -->
<!--         <property name="username" value="${jdbc.username2}" /> -->
<!--         <property name="password" value="${jdbc.password2}" /> -->
<!--         <property name="initialSize" value="5" /> -->
<!--         <property name="minIdle" value="1" /> -->
<!--         <property name="maxActive" value="20" /> -->
<!--         <property name="maxWait" value="60000" /> -->
<!--         <property name="timeBetweenEvictionRunsMillis" value="60000" /> -->
<!--         <property name="minEvictableIdleTimeMillis" value="300000" /> -->
<!--         <property name="validationQuery" value="#{'#{jdbc.driver}'=='com.mysql.jdbc.Driver'?'SELECT 1':'SELECT 1 FROM DUAL'}" /> -->
<!--         <property name="testWhileIdle" value="true" /> -->
<!--         <property name="testOnBorrow" value="false" /> -->
<!--         <property name="testOnReturn" value="false" /> -->
<!--         <property name="poolPreparedStatements" value="true" /> -->
<!--         <property name="maxPoolPreparedStatementPerConnectionSize" value="20" /> -->
<!--     </bean> -->

    <!-- 设置数据库供应商参数,在*mapper.xml中使用 -->
    <bean id="vendorProperties" class="org.springframework.beans.factory.config.PropertiesFactoryBean">
      <property name="properties">
        <props>
            <prop key="MySQL">mysql</prop>
            <prop key="Oracle">oracle</prop>
            <prop key="DB2">db2</prop>
            <prop key="SQL Server">sqlserver</prop>
        </props>
      </property>
    </bean>

    <!-- 引用数据库供应商参数,在*mapper.xml中使用 -->
    <bean id="databaseIdProvider"  class="org.apache.ibatis.mapping.VendorDatabaseIdProvider">
      <property name="properties" ref="vendorProperties"/>
    </bean>

    <!-- 数据源1的SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="configLocation" value="classpath:config/mybatis/SqlMapConfig.xml" />
        <property name="databaseIdProvider" ref="databaseIdProvider"/>
    </bean>
    <!-- 数据源2的SqlSessionFactory -->
<!--     <bean id="sqlSessionFactory2" class="org.mybatis.spring.SqlSessionFactoryBean"> -->
<!--         <property name="dataSource" ref="dataSource2"></property> -->
<!--         <property name="configLocation" value="classpath:config/mybatis/SqlMapConfig.xml" /> -->
<!--         <property name="databaseIdProvider" ref="databaseIdProvider"/> -->
<!--     </bean> -->

    <!-- 数据源1的MapperScannerConfigurer -->
    <!-- MapperScannerConfigurer:mapper的扫描器，将包下边的mapper接口自动创建代理对象，
        自动创建到spring容器中，bean的id是mapper的类名（首字母小写）
     -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 配置扫描包的路径
        如果要扫描多个包，中间使用半角逗号分隔
        要求mapper.xml和mapper.java同名且在同一个目录
         -->
        <property name="basePackage" value="com.cnnct.mapper"/>
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    </bean>
    <!-- 数据源2的MapperScannerConfigurer -->
<!--     <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"> -->
<!--         <property name="basePackage" value="com.cnnct.mapperoracle"/> -->
<!--         <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory2"/> -->
<!--     </bean> -->
    <!--============= datasourece配置结束================= -->

    <!--============= 事务配置开始================= -->
    <!-- 事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 数据源2的事务管理器 -->
<!--     <bean id="transactionManager2" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"> -->
<!--         <property name="dataSource" ref="dataSource2"></property> -->
<!--     </bean> -->

    <!-- 通知 -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="*" propagation="REQUIRED"/>
            <tx:method name="notran*" propagation="NOT_SUPPORTED" />
        </tx:attributes>
    </tx:advice>
    <!-- 数据源2的通知 -->
<!--     <tx:advice id="txAdvice2" transaction-manager="transactionManager2"> -->
<!--         <tx:attributes> -->
<!--             <tx:method name="*" propagation="REQUIRED"/> -->
<!--             <tx:method name="notran*" propagation="NOT_SUPPORTED" /> -->
<!--         </tx:attributes> -->
<!--     </tx:advice> -->

    <!-- aop -->
    <!-- expose-proxy属性用于通过ThreadLocal暴露Aop代理对象,使同一service类中事务和非事务混用时生效，用法示例：((BrchServ) AopContext.currentProxy()).test() -->
    <aop:config expose-proxy="true">
        <!-- 切入包下面的所有类的所有方法 不管返回值是什么，不管输入参数是什么 -->
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.cnnct..*ServImpl.*(..))"/>
    </aop:config>
    <!-- 数据源2的aop -->
<!--     <aop:config expose-proxy="true"> -->
<!--         <aop:advisor advice-ref="txAdvice2" pointcut="execution(* com.cnnct..*ServImpl.*(..))"/> -->
<!--     </aop:config> -->

    <!--============= 事务配置结束================= -->
```

#### applicationContext-service.xml

```
    <!-- 扫描注解,module为业务所在包 -->
    <context:component-scan base-package="com.cnnct.module" />

    <!-- ==================== ehcache 开始 ==================== --> 
    <!-- 错误码缓存 -->
    <bean id="errCodeProduct" class="com.cnnct.basic.cache.ErrCodeProduct">
        <property name="errCodeServ" ref="errCodeServ"/>
    </bean>
    <bean id="errCodeCacheFactory" class="com.cnnct.basic.cache.factory.ErrCodeCacheFactory" init-method="getCache" lazy-init="false">
        <property name="errCodeProduct" ref="errCodeProduct"/>
    </bean>
    <!-- syscode缓存 -->
    <bean id="sysCodeProduct" class="com.cnnct.basic.cache.SysCodeProduct">
        <property name="sysCodeServ" ref="sysCodeServ"/>
    </bean>
    <bean id="sysCodeCacheFactory" class="com.cnnct.basic.cache.factory.SysCodeCacheFactory" init-method="getCache" lazy-init="false">
        <property name="sysCodeProduct" ref="sysCodeProduct"/>
    </bean>
    <!-- syspara缓存 -->
    <bean id="sysParaProduct" class="com.cnnct.basic.cache.SysParaProduct">
        <property name="sysParaServ" ref="sysParaServ"/>
    </bean>
    <bean id="sysParaCacheFactory" class="com.cnnct.basic.cache.factory.SysParaCacheFactory" init-method="getCache" lazy-init="false">
        <property name="sysParaProduct" ref="sysParaProduct"/>
    </bean>
    <!-- trcode缓存 -->
    <bean id="trCodeProduct" class="com.cnnct.basic.cache.TrCodeProduct">
        <property name="trCodeServ" ref="trCodeServ"/>
    </bean>
    <bean id="trCodeCacheFactory" class="com.cnnct.basic.cache.factory.TrCodeCacheFactory" init-method="getCache" lazy-init="false">
        <property name="trCodeProduct" ref="trCodeProduct"/>
    </bean>
    <!-- reqcode缓存 -->
    <bean id="reqCodeProduct" class="com.cnnct.basic.cache.ReqCodeProduct">
        <property name="sysReqcodeServ" ref="sysReqcodeServ"/>
    </bean>
    <bean id="reqCodeCacheFactory" class="com.cnnct.basic.cache.factory.ReqCodeCacheFactory" init-method="getCache" lazy-init="false">
        <property name="reqCodeProduct" ref="reqCodeProduct"/>
    </bean>
    <!-- trOrder缓存 -->
    <bean id="trOrderProduct" class="com.cnnct.basic.cache.TrOrderProduct">
        <property name="trOrderServ" ref="trOrderServ"/>
    </bean>
    <bean id="trOrderCacheFactory" class="com.cnnct.basic.cache.factory.TrOrderCacheFactory" init-method="getCache" lazy-init="false">
        <property name="trOrderProduct" ref="trOrderProduct"/>
    </bean>
    <!-- ==================== ehcache 结束 ==================== -->

    <!-- ==================== 功能接口-start ==================== -->
    <!-- ===== 系统管理-start ===== -->
    <!-- base -->
    <bean id="baseServ" class="com.cnnct.basic.BaseServImpl"/>
    <!-- 定时任务加锁 -->
    <bean id="sysTaskLockServ" class="com.cnnct.module.sysmanager.systasklock.SysTaskLockServImpl"/>
    <!-- 错误码 -->
    <bean id="errCodeServ" class="com.cnnct.module.sysmanager.errcode.ErrCodeServImpl"/>
    <!-- syscode -->
    <bean id="sysCodeServ" class="com.cnnct.module.sysmanager.syscode.SysCodeServImpl"/>
    <!-- syspara -->
    <bean id="sysParaServ" class="com.cnnct.module.sysmanager.syspara.SysParaServImpl"/>
    <!-- 登录接口 -->
    <bean id="loginServ" class="com.cnnct.module.sysmanager.login.LoginServImpl"/>
    <!-- 日志接口 -->
    <bean id="actionLogServ" class="com.cnnct.module.sysmanager.actionlog.ActionLogServImpl"/>
    <!-- 系统错误日志接口 -->
    <bean id="sysErrLogServ" class="com.cnnct.module.sysmanager.syserrlog.SysErrLogServImpl"/>
    <!-- 首页 -->
    <bean id="indexServ" class="com.cnnct.module.sysmanager.index.IndexServImpl"/>
    <!-- 操作员管理 -->
    <bean id="operServ" class="com.cnnct.module.sysmanager.oper.OperServImpl"/>
    <!-- 部门管理 -->
    <bean id="brchServ" class="com.cnnct.module.sysmanager.brch.BrchServImpl"/>
    <!-- 角色管理 -->
    <bean id="roleServ" class="com.cnnct.module.sysmanager.role.RoleServImpl"/>
    <!-- 交易代码 -->
    <bean id="trCodeServ" class="com.cnnct.module.sysmanager.trcode.TrCodeServImpl"/>
     <!-- 附件表 -->
    <bean id="attachServ" class="com.cnnct.module.sysmanager.attachment.AttachServImple"/>
     <!-- 订单状态对应中文码 -->
    <bean id="trOrderServ" class="com.cnnct.module.sysmanager.trorder.TrOrderServImpl"/>
     <!-- 接口请求码管理 -->
    <bean id="sysReqcodeServ" class="com.cnnct.module.sysmanager.reqcode.SysReqcodeServImpl"/>
     <!-- 页面风格管理 -->
    <bean id="sysStyleServ" class="com.cnnct.module.sysmanager.style.SysStyleServImpl"/>

    <!-- file -->
    <bean id="fileServ" class="com.cnnct.module.sysmanager.file.FileServImpl"/>
    <!-- ===== 系统管理-end ===== -->
    <!-- ==================== 功能接口-end ==================== -->

    <!-- 接口类service -->
    <import resource="applicationContext-interfservice.xml"/>
```

#### freemarker.xml

```
    <!-- freemarker配置 -->
    <bean id="freemarkerConfig" class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
        <property name="templateLoaderPaths">
            <list>
                <value>/module/</value>
                <value>/main/</value>
                <value>/error/</value>
                <value>/WEB-INF/demo/</value>
                <value>/WEB-INF/template/</value>
            </list>
        </property>
        <property name="freemarkerSettings">
            <props>
                <prop key="template_update_delay">0</prop>
                <prop key="default_encoding">UTF-8</prop>
                <prop key="number_format">0.##########</prop>
                <prop key="datetime_format">yyyy-MM-dd HH:mm:ss</prop>
                <prop key="date_format">yyyy-MM-dd</prop>
                <prop key="time_format">HH:mm:ss</prop>
                <prop key="classic_compatible">true</prop>
                <prop key="template_exception_handler">ignore</prop>
            </props>
        </property>
    </bean>
```

#### Jar包内springmvc-servlet.xml

```
    <!-- 扫描标签库的Controller组件 -->
    <context:component-scan base-package="increator.core.controller"/>
    <!-- 扫描version接口版本控制组件 -->
     <context:component-scan base-package="increator.core.util.version"/>
     <!-- 扫描BaseC的Controller组件 -->
    <bean  class="increator.base.BaseC"/>
    <!-- 扫描aspect切面组件 -->
     <context:component-scan base-package="increator.base.aspect"/>
    <mvc:interceptors>
        <!--登录拦截器-->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <mvc:exclude-mapping path="/interf/**"/>
            <bean class="increator.base.interceptor.LoginInterceptor"></bean>
        </mvc:interceptor>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <mvc:exclude-mapping path="/interf/**"/>
            <bean class="increator.base.interceptor.ServInterceptor"></bean>
        </mvc:interceptor>
        <!-- 防重复提交拦截器 -->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <mvc:exclude-mapping path="/interf/**"/>
            <bean class="increator.base.interceptor.TokenInterceptor">
                <property name="interceptMethodName" value="del,save,add,update"/><!-- 拦截以这些开头的方法名 -->
            </bean>
        </mvc:interceptor>
    </mvc:interceptors>

    <!-- don't handle the static resource -->
    <mvc:default-servlet-handler />

    <!-- 由于接口版本控制，使用自定义的handleMaping标注，所以需要注释原本的类型转换驱动 -->
<!--     <mvc:annotation-driven conversion-service="conversionService"/> -->

    <!--统一异常处理器-->
    <bean class="increator.base.exception.CustomExceptionResolver"></bean>

    <!-- 由于接口版本控制，使用自定义的handleMaping标注，所以需要注释原本的类型转换驱动 -->
<!--     <bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean"> -->
        <!--转换器-->
<!--         <property name="converters"> -->
<!--             <list> -->
<!--                 <bean class="increator.base.converter.StringTrimConverter"/> -->
<!--                 <bean class="increator.base.converter.CustomDateConverter"/> -->
<!--             </list> -->
<!--         </property> -->
<!--     </bean> -->

    <bean id="multipartResolver"  class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="defaultEncoding" value="UTF-8"></property>
<!--        <property name="maxUploadSize" value="102400000"></property> -->
        <property name="maxUploadSize" value="1073741824"></property><!-- 1G -->
    </bean>

    <mvc:resources mapping="/images/**" location="/images/"/>
    <mvc:resources mapping="/style/**" location="/style/"/>
    <mvc:resources mapping="/script/**" location="/script/"/>
    <mvc:resources mapping="/main/**" location="/main/"/>

    <!-- html解析器 -->
    <bean id="htmlviewResolver"
        class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="increator.base.view.HtmlResourceView" />
        <property name="order" value="0" />
        <property name="prefix" value="/" />
        <property name="suffix" value=".html" />
        <property name="contentType" value="text/html;charset=UTF-8"></property>
    </bean>

    <!-- freemarker解析器 -->
    <bean id="freemarkerViewResolver" class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">
        <property name="order" value="1" />
        <property name="contentType" value="text/html; charset=utf-8" />
        <property name="cache" value="true" />
        <property name="suffix" value=".ftl" />
        <property name="requestContextAttribute" value="rc" />
        <!-- 自定义freemarkerview -->
        <property name="viewClass" value="increator.base.CnnctFreeMarkerView" />
    </bean>

    <!-- 注册XmlViewResolver，用于iReport & JasperReports报表生成 -->
    <bean id="jasperReportResolver" class="org.springframework.web.servlet.view.XmlViewResolver">
        <property name="order" value="0" />
        <property name="location" value="classpath:baseconfig/spring/jasper-views.xml"/>
    </bean>

    <!-- 系统日志切面类 -->
    <bean id="logAspect" class="increator.base.aspect.SysActionLogAspect" />
    <!-- 开启AOP监听,只对当前配置文件有效 -->
    <aop:aspectj-autoproxy expose-proxy="true" />
    <!-- AOP配置 -->
    <aop:config>
        <aop:aspect id="aspect" ref="logAspect">
             <aop:pointcut id="target" expression="execution(* com.cnnct..*Ctrl.*(..))"/>
<!--               <aop:before pointcut-ref="target" method="doBefore"/>   -->
<!--               <aop:after pointcut-ref="target" method="doAfter"/> -->
             <aop:around pointcut-ref="target" method="doAround" />
             <aop:after-throwing pointcut-ref="target" method="doThrowing" throwing="ex"/>
        </aop:aspect>
    </aop:config>

    <!-- 接口切面类 -->
    <bean id="serverAspect" class="com.cnnct.interf.aspect.ServerAspect" />
    <!-- 开启AOP监听,只对当前配置文件有效 -->
    <aop:aspectj-autoproxy expose-proxy="true" />
    <!--  接口切面类配置 -->
    <aop:config>
        <aop:aspect id="interfServerAspect" ref="serverAspect">
            <aop:pointcut id="interfServer" expression="execution(* com.cnnct.interf..*Ctrl.*(..)) || execution(* com.cnnct.interf..*Interceptor.*(..))"/>
            <aop:after-throwing pointcut-ref="interfServer" method="doThrowing" throwing="ex"/>  
        </aop:aspect>
    </aop:config>

    <!-- 用于ueditor注入附件service -->
    <bean class="com.baidu.ueditor.upload.StorageManager" init-method="getAttachService"/>
```

#### springmvc-servlet.xml

```
    <!-- 扫描注解,module为业务所在包 -->
    <context:component-scan base-package="com.cnnct.module" />
    <context:component-scan base-package="com.cnnct.interf.controller" />

     <!-- 示例接口拦截器frontEnd  -->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/interf/frontEnd/**"/>
            <bean class="com.cnnct.interf.interceptor.FrontEndInterceptor">
            </bean>
        </mvc:interceptor>
    </mvc:interceptors>
    <!-- 拦截器示例,如下所示,目前已注释,只是示例,无用配置  -->
<!--    <mvc:interceptors> -->
<!--        <mvc:interceptor> -->
<!--            <mvc:mapping path="/aabb"/> -->
<!--            <bean class="com.cnnct.interceptor.CommonInterceptor"></bean> -->
<!--        </mvc:interceptor> -->
<!--    </mvc:interceptors> -->

    <!-- 加载spring定时任务扫描注解驱动,并设置线程池数量,这两个线程池数量(pool-size)最好>=定时任务数量 -->
    <task:annotation-driven executor="myExecutor" scheduler="myScheduler"/>
    <task:executor id="myExecutor" pool-size="2"/>
    <task:scheduler id="myScheduler" pool-size="2"/>
    <!-- 指定spring定时任务类所在包 -->
<!--     <context:component-scan base-package="com.cnnct.task"/> -->
```

##### 事务过滤查询方法配置

![](/assets/Main1.png)



