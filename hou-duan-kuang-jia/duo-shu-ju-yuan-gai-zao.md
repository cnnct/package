# ！！！！涉及到的源代码改造，下面写得现在太简单了，框架如何改造后，才能支持多数据源？？

#### 多数据源逆向工程jar包改造

修改了mybatis-generator-1.3.2.jar中的：

org.mybatis.generator.api.dom.java.Interface

org.mybatis.generator.config.xml.MyBatisGeneratorConfigurationParser

多数据源的实现只要在spring的xml文件中配置即可，详情参照开发人员手册中的多数据源配置。

