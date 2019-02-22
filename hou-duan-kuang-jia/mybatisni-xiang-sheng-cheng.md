# ！！！！18年3月后在源代码基础上做了哪几个类的修改，需要重点说明！！！！！

包括源码的改造点说明，以及源码下载地址，以及我们自己另外实现了先删除再生成的相关改造，相关的类、jar包等

mybatis-generator-core下载：[http://mvnrepository.com/artifact/org.mybatis.generator/mybatis-generator-core](http://mvnrepository.com/artifact/org.mybatis.generator/mybatis-generator-core)

#### 特定jar包

mybatis-generator-core-1.3.2.jar

此jar包中的源码修改有：

org.mybatis.generator.api.dom.java.Interface

org.mybatis.generator.config.xml.MyBatisGeneratorConfigurationParser

#### GeneratorSqlmap.java

使用mybatis逆向生成时，是运行GeneratorSqlmap.java这个类的main方法实现的。这个类中包含：先删除普通pojo、Mapper.java、Mapper.xml文件，再调用mybatis-generator-core-1.3.2.jar中的方法生成普通pojo、Mapper.java、Mapper.xml文件，再调用此类中的方法custGenerator生成custom\(即扩展的\)pojo、Mapper.java、Mapper.xml文件。

#### 使用mybatis逆向生成

#### 1.generatorConfig.xml文件配置

#### 注：

> * #### 每次使用逆向工程为一个数据源生成文件后，就修改一下driverClass、connectionURL、userId、password、publicKey以及3个targetPackage的值，还有&lt;table&gt;的tableName，配置上需要逆向工程生成文件的表，然后再执行逆向工程GeneratorSqlmap.java。
> * #### 每次运行GeneratorSqlmap.java生成文件时，会自动删除已存在的基础实体类和基础mapper的同名文件，因此使用前一定注意是否这三个文件有做过个性化修改。
> * #### 因此，鉴于前一事项，建议基础实体类和基础的mapper尽量不要去修改，若要修改或增加功能，在custom中进行处理。
> * #### 以上提到的三个基础文件及custom为别为：
>
> /src/com/cnnct/po/表名.java，对应的custom文件：/src/com/cnnct/po/custom/表名Cust.java
>
> /src/com/cnnct/mapper/表名Mapper.java，对应的custom文件：/src/com/cnnct/mapper/custom/表名CustMapper.java
>
> /src/com/cnnct/mapper/表名Mapper.xml，对应的custom文件：/src/com/cnnct/mapper/custom/表名CustMapper.xml

```
<generatorConfiguration>
    <properties resource="config/parameter/db.properties"/>
    <context id="testTables" targetRuntime="MyBatis3" defaultModelType="flat">
        <!-- 生成PO类时序列化 -->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin" />
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="${jdbc.driver}"
            connectionURL="${jdbc.url}" userId="${jdbc.username}"
            password="${jdbc.password} publicKey="${publicKey}">
        </jdbcConnection>

        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和
            NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!-- targetProject:生成PO类的位置 -->
        <javaModelGenerator targetPackage="com.cnnct.po"
            targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="com.cnnct.mapper"
            targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>
        <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
            targetPackage="com.cnnct.mapper"
            targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>
         <!-- 
         <table tableName="bs_city" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>
         <table tableName="bs_pay_org" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>

         <table tableName="sys_action_log" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false">
             <columnOverride column="message" javaType="java.lang.String" jdbcType="VARCHAR"/>
             <columnOverride column="in_data" javaType="java.lang.String" jdbcType="VARCHAR"/>
             <columnOverride column="out_data" javaType="java.lang.String" jdbcType="VARCHAR"/>
         </table>
    </context>
</generatorConfiguration>
```

#### 2.关于generatorConfig.xml文件中的三个targetPackage目录配置说明

###### 初始单数据源或多数据源的第一个数据源时，三个targetPackage分别默认为：com.cnnct.po、com.cnnct.mapper、com.cnnct.mapper。

###### 如果配置多个数据源或不想用这3个默认目录，修改规则为：在此3个目录的基础上补上一个后缀，后两个目录必须相同。

###### 如：com.cnnct.po2、com.cnnct.mapper2、com.cnnct.mapper2

###### 或 com.cnnct.pooracle、com.cnnct.mapperoracle、com.cnnct.mapperoracle等。

###### 而这里的后两个目录\(相同，即同一个目录\)，对应applicationContext-dao.xml文件中basePackage，通过数据源一一对应。

#### 注：如果使用的不是3个默认目录，那么，注入基础Mapper\(非custom中的\)时，需要使用别名，别名在基础Mapper中有。

#### 示例：

基础Mapper

```
package com.cnnct.mapper;

import org.springframework.stereotype.Component;

import com.cnnct.po.SysRole;

@Component("sysRoleMapperOracle")
public interface SysRoleMapper {
    int deleteByPrimaryKey(Long roleId);

    int insert(SysRole record);

    int insertSelective(SysRole record);

    SysRole selectByPrimaryKey(Long roleId);

    int updateByPrimaryKeySelective(SysRole record);

    int updateByPrimaryKey(SysRole record);
}
```

注入代码，此处为sysRoleMapperOracle对应基础Mapper中的别名，而不能随意起名

```
@Autowired
private SysRoleMapper sysRoleMapperOracle;
```



