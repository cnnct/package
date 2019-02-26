# ！！！这个项目目录结构层次要说明下

# ！！！实现了哪些功能，以及功能描述（运维功能和接口都说明）

# ！！！数据库表注释，并导出表结构及基础数据，备份，上传到svn的数据库目录中

# ！！！再重点用图或流程说明，提供出来的流程接口，大致数据流向，就是操作哪些表哪些状态或者调用了哪些api，若有图，源图文件要上传到svn的设计目录中

### 工作流云平台

工作流云平台基于：管理平台2.0新框架 + Activiti5.22.0

Activiti官网地址：https://www.activiti.org/

#### 核心框架目录结构：

一、核心框架Java类目录如下：

![](/assets/activiti5.png)

1、modeler目录中的三个类用于在线流程编辑器，用于将英文翻译为中文，其实就是activiti源代码中同样的包同样的三个类，相当于覆盖掉原来的，然后修改的地方如下：

![](/assets/activiti6.png)也就是将这个文件的路径修改为我们项目中的实际路径。

此处四个类需要在activiti.cfg.xml中配置才能使用：

![](/assets/activiti10.png)

2、processimg目录中的两个类用于流程图，用于根据R012接口获取的url来获取流程图文件流。

3、usergroup目录中的四个类用于用户和用户组配置，其中用户组目前没用到。CustomUserManager类中配置重写了用户查询方法及不验证密码方法。此处四个类需要在activiti.cfg.xml中配置才能使用：

![](/assets/activiti11.png)二、核心框架配置文件目录如下：

![](/assets/activiti12.png)

1、modeler目录下的stencilset.json用于在线编辑器的翻译，上面说过了。

2、activiti.cfg.xml就是activiti工作流引擎的核心配置文件，具体配置及功能在配置文件中已经写得很清楚了。

三、核心框架Web目录如下：

![](/assets/activiti13.png)

1、diagram-viewer目录不用改什么，都是activiti原本的。

2、editor-app目录中的app-cfg.js修改地方如下：

![](/assets/activiti14.png)

就是配置下项目根路径url。

3、modeler.html文件就是在线编辑器的首页，默认只能放在项目根目录下。

#### 核心功能目录结构：

一、核心功能Java类目录如下：

![](/assets/activiti16.png)

1、workflow目录下的WorkFlowCommonCtrl就是对外提供所有流程接口的类。

#### 基础功能目录结构：

一、基础功能Java类目录如下：

![](/assets/activiti17.png)

1、busiplatform目录下的三个类用于对接平台管理。

2、process目录下的definitioin目录用于流程定义管理；instance目录用于流程实例管理；model目录用于流程图模型管理；reports目录用于报表管理；task目录用于流程任务管理。

二、基础功能Web目录如下：

![](/assets/activiti18.png)

1、busiplatform目录下的三个类用于对接平台管理。

2、process目录下的definitioin目录用于流程定义管理；instance目录用于流程实例管理；model目录用于流程图模型管理；reports目录用于报表管理；task目录用于流程任务管理。

#### 关于表结构相关信息具体可看svn上的开发指南：

![](/assets/activiti19.png)

