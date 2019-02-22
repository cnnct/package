# ！！！！！sys\_report表的说明

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

> 此类必须继承JasperReportsMultiFormatView，重写父类的fillReport方法。

* ### jar包处理

> json-lib-2.1-jdk15.jar升级为json-lib-2.4-jdk15.jar
>
> groovy-all-1.7.5.jar升级为groovy-all-2.2.2.jar
>
> jasperreports-5.0.1.jar升级为jasperreports-5.6.0.jar
>
> 删除jasperreports-char-themes-5.0.5.jar、jasperreports-extensions-3.5.3.jar、jasperreports-fonts-5.0.jar等三个jar包

### 改造BaseCtrl.java

> 重载方法getPageMap，增加一个入参key，原理如下
>
> > * 在查询分页表格时，多指定一个当前唯一标识的key，一般是当前请求对应的url即可，根据这个key值将查询条件放到缓存中（并作特殊处理，去掉分页查询信息，因为报表一般是要查询所有记录）。
> >
> > ```
> > // ①分页参数
> > // ②并将参数放入缓存中，供报表使用，
> > // 其中第三个入参是用于jasper打印，需要额外增加的入参，实际就是一个map的key
> > // 可以是任意不重复的值（需要pdf打印的功能不能重复），一般就用当前url比较好理解
> > resultData = getPageMap(request, resultData, "/sys/auth/brch/query");
> > ```
> >
> > 再在报表显示请求方法中，重新根据key获取到当前的查询条件，查询结果作为报表展示数据的数据源。
> >
> > ```
> > //③数据源，先查询条件，可从缓存中获取，也可重新赋值
> > //入参即为对应在的查询功能缓存的查询条件的map的key，其key值与queryBrchInfo方法中指定key为同一个
> > Map reportMap=super.getTableSearchData("/sys/auth/brch/query");
> > model.addAttribute("jrMainDataSource", brchServ.getBrchList(reportMap));
> > ```



