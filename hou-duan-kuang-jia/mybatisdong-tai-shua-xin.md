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

