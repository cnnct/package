注注：本章节重点说明整合在线编辑器，在原官方提供的在线编辑器上的个性化改造

任务派遣小窗口界面\(陆\)

原来是三个文本框，改成可的下拉列表，页面位于assignment-popup.html中，13行左右

下拉列表从该模型对应平台的接口获取，相关表：

wf\_sys\_interf\_conf第三方平台接口配置表。

wf\_def\_model，定义和模型关联表

wf\_sys\_procdef，第三方平台和流程定义关联

  


页面位于assignment-popup.html中

properties-assignment-controller.js中，顶部一段代码是控制调用用户查询，触发调用com.cnnct.module.process.model.ModelCtrl.getUserGroup方法，实际是查询第三方系统的提供的用户查询接口ModelServImpl.getUserGroup，第三方接口需要按我们的要求返回，接口api文档有说明。



  


  




  


  


  


CustomUserManager.findUserByQueryCriteria，

实现UserEntityManager，在activiti.cfg.xml中的customSessionFactories进行配置

流程引擎中自动会调用这个实现类，查询用户信息，原父类是查询本库的用户表信息。

  


模型名称置为禁用（施）

编辑完模型点save-model.html里，nameField文本框改为只读，为了实现定义的名称和模型名称保持一致。如果要修改，从管理界面流程定义首页，统一修改，该修改功能会一并把相关联的名称一并改掉。

  


  


  


汉化

com.cnnct.module.activiti.modeler包下的三个类，activiti官方源代码目录中解压出来，进行修改，

实际上只改了一个类，StencilsetRestResource汉化json的路径。

  


app-cfg.js

修改了contextRoot:window.opener.base，可能给自己调用请求路径用。

  


编辑器的关闭按钮改造

toolbar-default-actions.js，263行，改成点关闭按钮为关闭本窗口，原功能是跳转到另一个页面

  


