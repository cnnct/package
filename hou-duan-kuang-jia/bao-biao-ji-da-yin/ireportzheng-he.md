在中增加解析器配置

```
<!-- 注册XmlViewResolver，用于iReport & JasperReports报表生成 -->
<bean id="jasperReportResolver" class="org.springframework.web.servlet.view.XmlViewResolver">
    <property name="order" value="0" />
    <property name="location" value="classpath:baseconfig/spring/jasper-views.xml"/>
</bean>
```



