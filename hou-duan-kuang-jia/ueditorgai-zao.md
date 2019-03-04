# ！！！！较18年3月后还做了哪些新的优化改造！！！动了哪些文件

ueditor源码版本，下载地址，以及改造点  
以及对135支持注意事项说明

## ueditor改造

* 新框架的ueditor改造基于ueditor-1.4.3.3的jsp版本
* 下载地址：[http://ueditor.baidu.com/website/download.html](http://ueditor.baidu.com/website/download.html)
* ueditor的改造主要有这几点：

  1.改造图片，附件，视屏等文件上传到ftp服务器上。  
    2.集成135微信图文编辑器插件\[[http://www.135plat.com/open\_editor.html\]。](http://www.135plat.com/open_editor.html]。)  
    3.将ueditor组件化，易于开发。

**一.前端部分改造：**

* 涉及的文件：

![](/assets/frontDoc_ueditor1.png)  
注意：这里要说明的是，由于前端集成了135微信编辑器，该插件对ueditor的前端代码进行了混淆，所以在后续改造和优化上会较麻烦，暂时需要参照ueditor源码进行改造

**二.后端部分改造：**

* 涉及的文件：

![](/assets/frontDoc_ueditor2.png)

改造主要的文件有：ActionEnter.java,ConfigManager.java,BinaryUploader.java,StorageManager.java等

**三.ueditor模板加载文件：**  
 ueditor.ftl和ueditorTag.java
 
**四.后续改造部分:**
1.修复上传图片乱序问题：
* 涉及文件：

![](/assets/ueditor_2.png)
2.修复ctrl+v+enter快捷方式带来的问题：
* 涉及文件：
![](/assets/ueditor_3.png)