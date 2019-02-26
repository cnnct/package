# 1、工作流平台对外接口文档

[http://soeasycn.com/api](http://soeasycn.com/api/)

注：若文档地址无法访问，请联系彭敏，13735493070。

# 2、资料说明

> ### 开发指南

工作流云平台基于：管理平台2.0新框架 + Activiti5.22.0

Activiti官网地址：[https://www.activiti.org/](https://www.activiti.org/)

> ### 开发指南

$svn$/1-文档/09-资料/Activiti5.22.0开发指南.pdf

> ### 官方示例项目，可直接部署

* #### $svn$/工作流云平台/2-源代码/activiti官方示例项目/activiti-explorer.war

是工作流管理平台的参考原型，有页面显示，自带在线编辑工具

登录 账号密码，可网上搜索，搜索结果如下：

账号    密码    角色

kermit    kermit    admin

gonzo    gonzo    manager

fozzie    fozzie    user

* #### $svn$/工作流云平台/2-源代码/activiti官方示例项目/activiti-rest.war

相当于提供一个可以提供调用的基础接口示例，但无页面展示

> ### 官方源代码

* $svn$//2-源代码/activiti官方源代码/Activiti-activiti-5.22.0.zip

我们管理平台中对在线编辑器的汉化有用到源码改造：activiti-modeler.jar中com.cnnct.module.activiti.modeler包下的三个类，但实际只用到一个类StencilsetRestResource。

> ## eclipse流程编辑器插件

使用Install new SoftWare，地址为：[http://activiti.org/designer/update/](qq://txfile/#)

# 3、基础规则说明

> ### 流程文件部署

* 流程文件以bpmn或bpmn20.xml结尾，可以用eclipse中整合的activiti流程图插件绘制保存。
* 首次部署必须先使用工具绘制完成。
* 部署方式一：可放到项目指定目录，通过系统启动自动加载部署，加载部署配置在activiti.cfg.xml中的deploymentResources配置荐中配置。启动部署完成后，源文件可移除，或者进行备份后删除。
* 部署方式二：可通管工作流管理平台上导入部署方式部署，流程文件另外备份留在即可。

> ### 基本概念

* ##### 流程定义

通过流程文件部署完成后产生流程定义，同一流程key，多次部署，流程定义版本会自动增1，若要新启动一个流程时，会自动使用最新版本流程定义启动。![](/assets/flowdef.png)

* ##### 流程模型（也叫流程模板）

从流程定义中新转换派生出模型，目的是在流程版本上进行少量调整后，再重新部署，用以生成新版本的流程定义，方便流程管理员维护操作，而不需要开发人员干预。

在流程定义菜单中，【查看详情】，

模态框显示该定义各版本信息列表，再用【流程定义模板】，弹出新tab页，显示该定义的模板列表，如下图所示：

![](D:/Program Files/Youdao/YoudaoNote/data/iceaugust@163.com/8e9d446e5a5f4fed8da39e666c60e700/clipboard.png)这里可以新增，也可以修改，会打开一个模型编辑器。

新增是沿用原流程定义的相关属性，新生成模型数据。

也可以修改，修改保存后，模型版本并无增加。

* ##### 流程实例

启动一个流程定义，会生成一个流程实例。

* ##### 任务项

流程实例启动后，会根据活动节点，生成节点的任务项，任务项是与具体操作人绑定。

