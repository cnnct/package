### 整合步骤：

1、引入如下jar包：

![](/assets/activiti_1.png)

2、配置Activiti工作流引擎——activiti.cfg.xml，并在Spring配置文件context.xml中引入，详细配置信息：

```
    <!-- ======个性化配置===== -->
    <!-- 扫描Activiti在线编辑器的跳转@RestController,编辑器汉化 -->
    <context:component-scan base-package="com.cnnct.module.activiti.modeler" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    </context:component-scan>

    <!-- ======官方固定配置===== -->
    <!-- 单例json对象,用于Activiti在线编辑器 -->
    <bean id="objectMapper" class="com.fasterxml.jackson.databind.ObjectMapper"/>

    <!-- ======官方固定配置===== -->
    <!-- 流程引擎配置 -->
    <bean id="processEngineConfiguration" class="org.activiti.spring.SpringProcessEngineConfiguration">
        <!-- 数据源 -->
        <property name="dataSource" ref="dataSource" />
        <!-- 事务 -->
        <property name="transactionManager" ref="transactionManager" />
        <!-- 是否检查并补全Activiti引擎必需的表,一般用于第一次启动引擎 -->
<!--         <property name="databaseSchemaUpdate" value="true" /> -->
        <!-- 流程图等所用的字体 -->
        <property name="activityFontName" value="宋体" />
        <property name="labelFontName" value="宋体" />
        <!-- ======个性化配置===== -->
        <!-- 项目启动部署流程定义，首次以文件部署定方式时开启，相当于批量导入部署，首次启动完成后，就可以注释掉，也可以直接在管理平台上做批量导入部署 -->
<!--         <property name="deploymentResources" value="classpath*:config/activiti/**/*.bpmn" /> -->
        <!-- 个性化配置：自定义用户和组管理 -->
        <property name="customSessionFactories">
            <list>
                <bean class="com.cnnct.module.activiti.usergroup.CustomUserFactory" />
                <bean class="com.cnnct.module.activiti.usergroup.CustomGroupFactory" />
            </list>
        </property>
    </bean>

    <!-- ======以下均为官方固定配置===== -->
    <!-- 官方固定配置：流程引擎的抽象，通过它我们可以获得我们需要的一切服务，可视为下方所有bean的总合集-->
    <bean id="processEngine" class="org.activiti.spring.ProcessEngineFactoryBean">
        <property name="processEngineConfiguration" ref="processEngineConfiguration" />
    </bean>

    <!-- Activiti中每一个不同版本的业务流程的定义都需要使用一些定义文件，
            部署文件和支持数据(例如BPMN2.0 XML文件，表单定义文件，流程定义图像文件等)，
            这些文件都存储在Activiti内建的Repository中。RepositoryService提供了对 repository的存取服务 -->
    <bean id="repositoryService" factory-bean="processEngine"
        factory-method="getRepositoryService" />

    <!-- 在Activiti中，每当一个流程定义被启动一次之后，都会生成一个相应的流程对象实例。
     RuntimeService提供了启动流程、查询流程实例、设置获取流程实例变量等功能。
            此外它还提供了对流程部署，流程定义和流程实例的存取服务 -->
    <bean id="runtimeService" factory-bean="processEngine"
        factory-method="getRuntimeService" />

    <!-- 在Activiti中业务流程定义中的每一个执行节点被称为一个Task，对流程中的数据存取，
            状态变更等操作均需要在Task中完成。TaskService提供了对用户Task 和Form相关的操作。
            它提供了运行时任务查询、领取、完成、删除以及变量设置等功能 -->
    <bean id="taskService" factory-bean="processEngine"
        factory-method="getTaskService" />

    <!-- Activiti中内置了用户以及组管理的功能，必须使用这些用户和组的信息才能获取到相应的Task。
     IdentityService提供了对Activiti 系统中的用户和组的管理功能 ，暂未用-->
    <bean id="identityService" factory-bean="processEngine"
        factory-method="getIdentityService" />

    <!-- ManagementService提供了对Activiti流程引擎的管理和维护功能，
            这些功能不在工作流驱动的应用程序中使用，主要用于Activiti系统的日常维护 -->
    <bean id="managementService" factory-bean="processEngine"
        factory-method="getManagementService" />

    <!-- HistoryService用于获取正在运行或已经完成的流程实例的信息，与RuntimeService中获取的流程信息不同，
            历史信息包含已经持久化存储的永久信息，并已经被针对查询优化 -->
    <bean id="historyService" factory-bean="processEngine"
        factory-method="getHistoryService" />

    <!-- 对表单的管理，暂未用 -->
    <bean id="formService" factory-bean="processEngine"
        factory-method="getFormService" />
```



