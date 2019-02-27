# 主要整合：

* Spring整合Activiti流程引擎
* 在线流程编辑器整合
* 流程引擎接口封装
* 流程管理维护平台

# 项目目录说明

* ### 1、基础核心改造相关文件目录

### ![](/assets/activiti_dir_07.png)

modeler：汉化在线流程编辑器相关类；

processimg：第三方系统显示流程图展示接口调用相关类；

usergroup：自定义用户、用户组，暂时用到用户相关，用户组暂未使用到；

stencilset.json：线流程编辑器汉化转换配置；

* ### 2、接口平台封装相关文件目录

![](/assets/activiti_dir_01.png)WorkFlowComonCtrl.java：接口服务的Ctrl；

WorkFlowInterceptor.java：接口服务拦截器；

WorkFlowCommonServImpl.java：接口服务service；

### 3、管理维护平台相关文件目录

![](/assets/activiti_dir_02.png)

busi

![](/assets/activiti_dir_03.png)

busiplatform：对接平台管理；

process/definitioin：流程定义管理；

process/instance：流程实例管理；

process/model：流程图模型管理；

process/reports：报表管理；

process/task：流程任务管理。

### 3、在线流程编辑器相关文件目录

![](/assets/activiti_dir_08.png)

