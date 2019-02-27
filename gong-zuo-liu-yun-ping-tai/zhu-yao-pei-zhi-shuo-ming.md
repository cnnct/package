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

### 注

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

注注：管理维平台详细说明，详见章节：[工作流管理维护平台](/gong-zuo-liu-yun-ping-tai/zhu-yao-pei-zhi-shuo-ming/liu-cheng-guan-li-wei-hu-ping-tai-3010-shi-3011.md)

### 3、在线流程编辑器相关文件目录

![](/assets/activiti_dir_08.png)

modeler.html：在线流程编辑器首页

diagram-viewer、editor-app：在线编辑器自带，拷贝过来

#### 注：在线流程编辑器个性化改造，详见章节：[在线编辑编辑器整合](/gong-zuo-liu-yun-ping-tai/zhu-yao-pei-zhi-shuo-ming/activitizai-xian-bian-ji-qi.md)



