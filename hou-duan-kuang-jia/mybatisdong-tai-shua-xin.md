实现相关的源代码、配置、jar说明，若涉及到改造了网上下载的源代码，贴上源代码下载路径，以及改造点

#### 特定jar包

无。

#### 启动mybatis动态刷新

首先，在para.properties中配置"mybatis\_refresh=true"，以及"mybatis\_refresh\_info={"sqlSessionFactory":"com/cnnct/mapper"}"。

mybatis\_refresh为true时启用，false时禁用。开发时可设置为true，生产环境时务必改为false。

mybatis\_refresh\_info使用Json格式，key为SqlSessionFactoryBean的beanName，value为\*Mapper\*.xml文件所在的目录。多数据源时，按照Json格式接着写在后面就行了。

#### MapperRefreshStart.java

此类为mybatis动态刷新的启动类，默认已经添加到Sping配置文件中，随项目启动而初始化。

#### MapperRefresh.java

此类为mybatis动态刷新的逻辑实现类，根据para.properties中配置"mybatis\_refresh"的值来判断是否启动动态刷新。

#### 流程原理

1、在spring-applicationCore.xml中配置MapperRefreshStart的bean，并设置初始化方法init。

2、MapperRefreshStart中的init方法根据para.properties配置文件中的数据源bean名称获取SqlSessionFactory配置，并在启动动态刷新类MapperRefresh时传入。

3、动态刷新类MapperRefresh和获取SqlSessionFactory中所有的Mybatis类型的xml文件后，启动线程无限循环遍历所有文件，将最后修改时间与第一次MapperRefresh类时记录的时间beforeTime进行对比，如果最后修改时间大于beforeTime，那么在重新编译加载这些文件后，将此时的时间赋给beforeTime，并进行下一次循环对比。

