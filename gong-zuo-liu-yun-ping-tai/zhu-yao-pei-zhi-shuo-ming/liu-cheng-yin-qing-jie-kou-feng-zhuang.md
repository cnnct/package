## 接口入口：

使用基础框架的统一接口配置，

ctrl：com.cnnct.interf.controller.workflow.WorkFlowCommonCtrl

service：com.cnnct.interf.services.workflow.WorkFlowCommonServImpl

## 接口功能介绍：

详见[工作流平台接口文档](http://soeasycn.com/api)

## 流程图在第三方平台显示说明：

ctrl：com.cnnct.module.activiti.processimg.ProcessImgCtrl

提供固定的请求url：

/nofunc/getProcessImg/{processKey}：使用流程key作为入参，显示流程定义原图

/getProcessImgWithActivitis/{instanceId}：使用流程实例id作为入参，显示当前流转状态流程图

