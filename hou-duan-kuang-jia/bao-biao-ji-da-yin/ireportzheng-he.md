* ### 在core\baseconfig\spring\springmvc-servlet.xml中增加解析器配置

```
<!-- 注册XmlViewResolver，用于iReport & JasperReports报表生成 -->
<bean id="jasperReportResolver" class="org.springframework.web.servlet.view.XmlViewResolver">
    <property name="order" value="0" />
    <property name="location" value="classpath:baseconfig/spring/jasper-views.xml"/>
</bean>
```

* ### 增加文件core\baseconfig\spring\jasper-views.xml

> 主要作用是配置jaserreport生成的相关参数，如自定义报表扩展类increator.base.report.CustomReportView



