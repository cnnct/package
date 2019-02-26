## 1、在已[整合Activiti流程引擎](/gong-zuo-liu-yun-ping-tai/zhu-yao-pei-zhi-shuo-ming/activitizheng-he-spring.md)的基础上，引入如下jar包：

![](/assets/activiti_2.png)

## 2、引入流程编辑器到web目录

从从示例代码【activiti-explorer】中拷贝：diagram-viewer目录、editor-app目录、modeler.html文件，这三个对象到WebRoot目录中。

![](/assets/activiti_4.png)

## 3、个性化改造

* ## 汉化

5.22.0版本源代码中activiti-modeler.jar中的com.cnnct.module.activiti.modeler包下的三个类，activiti官方源代码目录中解压出来，进行修改，

实际上只改了一个类，StencilsetRestResource，指定汉化json的路径：/config/activiti/modeler/stencilset.json。

![](/assets/activiti_3.png)

* ## app-cfg.js

修改了contextRoot:window.opener.base，可能给编辑器自己调用请求路径用。

* ## 编辑器的关闭按钮改造

toolbar-default-actions.js，263行，改成点关闭按钮为关闭本窗口，原功能是跳转到另一个页面。

* ## 任务派遣小窗口界面修改

#### 原来是三个文本框，改成可搜索下拉列表，

##### ①页面modeler.html中，引入实现搜索下拉列表控件的css、js资源，使用【--lucg】注释的片段

##### ②页面assignment-popup.html中，13行左右，改成下拉列表控件

下拉列表从该模型对应平台的接口获取，相关表：

wf\_sys\_interf\_conf第三方平台接口配置表。

wf\_def\_model，定义和模型关联表

wf\_sys\_procdef，第三方平台和流程定义关联

1. ##### properties-assignment-controller.js中，顶部一段代码是控制调用用户查询，触发调用com.cnnct.module.process.model.ModelCtrl.getUserGroup方法，实际是查询第三方系统的提供的用户查询接口ModelServImpl.getUserGroup，第三方接口需要按我们的要求返回，接口api文档有说明。
2. ## 自定义用户、用户组，其中用户组暂未启用，代码中已注释

在activiti.cfg.xml中的customSessionFactories进行配置

CustomUserManager.findUserByQueryCriteria，实现UserEntityManager，

流程引擎中自动会调用这个实现类，查询用户信息，原父类是查询本数据库的用户表信息。

