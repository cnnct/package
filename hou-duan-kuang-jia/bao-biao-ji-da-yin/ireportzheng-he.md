* ### 在core\baseconfig\spring\springmvc-servlet.xml中增加解析器配置

```
<!-- 注册XmlViewResolver，用于iReport & JasperReports报表生成 -->
<bean id="jasperReportResolver" class="org.springframework.web.servlet.view.XmlViewResolver">
    <property name="order" value="0" />
    <property name="location" value="classpath:baseconfig/spring/jasper-views.xml"/>
</bean>
```

* ### 增加文件core\baseconfig\spring\jasper-views.xml

> 主要作用是配置jaserreport生成的相关参数，如自定义报表jsperreport视图扩展类increator.base.report.CustomReportView

* ### 增加类increator.base.report.CustomReportView

> 此类必须继承JasperReportsMultiFormatView

* ### jar包处理

> json-lib-2.1-jdk15.jar升级为json-lib-2.4-jdk15.jar
>
> groovy-all-1.7.5.jar升级为groovy-all-2.2.2.jar
>
> jasperreports-5.0.1.jar升级为jasperreports-5.6.0.jar
>
> 删除jasperreports-xxxxxx等三个jar包



