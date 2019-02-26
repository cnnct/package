> ### 整合步骤：

## 1、在已[整合Activiti流程引擎](/gong-zuo-liu-yun-ping-tai/zhu-yao-pei-zhi-shuo-ming/activitizheng-he-spring.md)的基础上，引入如下jar包：

![](/assets/activiti_2.png)

## 2、引入流程编辑器到web目录

从从示例代码【activiti-explorer】中拷贝：diagram-viewer目录、editor-app目录、modeler.html文件，这三个对象到WebRoot目录中。

## 3、个性化改造

汉化

com.cnnct.module.activiti.modeler包下的三个类，activiti官方源代码目录中解压出来，进行修改，

实际上只改了一个类，StencilsetRestResource汉化json的路径。

app-cfg.js

修改了contextRoot:window.opener.base，可能给自己调用请求路径用。

编辑器的关闭按钮改造

toolbar-default-actions.js，263行，改成点关闭按钮为关闭本窗口，原功能是跳转到另一个页面

将5.22.0版本源代码中activiti-modeler.jar的源代码中的三个java文件复制到项目中，看情况修改。

![](/assets/activiti_3.png)

3、复制前端文件

将5.22.0版本源代码中activiti-webapp-explorer2中的如下目录复制到项目中，看情况修改。

![](/assets/activiti_4.png)

