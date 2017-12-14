从web.xml中的dispatcher等配置开始，依次按目录层次所包含的主要配置文件进行说明各配置文件的作用以及主要配置项、加载顺序，包括core包中，和src包中

#### web.xml

配置Spring配置文件路径

```
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:config/spring/context.xml</param-value>
</context-param>
```

配置SpringMVC配置文件路径

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
```



