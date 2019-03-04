##登录验证码：
1.涉及的文件：
 login.ftl
 view_para.properties
 com.cnnct.module.sysmanager.login.LoginCtrl
 com.cnnct.module.sysmanager.login.LoginServImpl
 kaptcha-2.3.2.jar
 applicationContext-bean.xml

2.加载过程：
* 当请求登录页的时候会检查配置文件中验证码的开关，如果验证码开关开启就会指示页面加载开放验证码，需要验证验证码。
![](/assets/vercode_1.png)
* 页面加载验证码：
![](/assets/vercode2.png)
* 后台加载绘制验证码：
