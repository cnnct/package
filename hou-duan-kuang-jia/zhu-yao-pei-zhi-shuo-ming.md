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



