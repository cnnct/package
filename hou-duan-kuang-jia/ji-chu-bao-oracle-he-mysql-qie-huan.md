要实现切换主要是哪些jar或配置在起作用，databaseid工作原理大致说明一下，比如默认是找匹配的，再次是找无databaseid的等

#### 多数据源

多数据源是同时存在的：不管这几个数据源是不同类型的数据库还是同一类型的数据库，运行时都是同时存在的。

多数据源是互不干扰的：各个数据源之间是互不干扰的，各自对应不同的Mapper接口和xml文件。

#### databaseId

目前配置的有：

```
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
```

这四个数据库类型，databaseId即是四个prop的值。

在\*Mapper\*.xml文件中：

```
<select id="getRoleList" parameterType="java.util.HashMap" resultType="java.util.HashMap" databaseId="mysql">
```

如上所示，在每个select/insert/update/delete等标签写上databaseId="mysql"，表示只有当前数据源是MySQL才会执行这里面的内容。

#### databaseId匹配原理

我画了一个匹配过程，如下：

![](/assets/databaseId.png)

