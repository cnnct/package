特别注意写明用到的百度控件的版本，及下载地址，若改过源码，要重点说明  
另外需要额外说明文件缓存表的使用原理

以及上传文件命名规则等，以及与ftp.properties文件的关系

##### 1、控件下载地址

[http://fex.baidu.com/webuploader/](http://fex.baidu.com/webuploader/)

##### 2、修改部分

基本上就改了upload.js文件，此文件中代码注释齐全：![](/assets/WebUploader1.png)

##### 3、文件回显实现原理

\(1\)标签添加属性，给开发人员传附件表sys\_attachment的upload\_batch字段，即文件批次号标识。

\(2\)标签封装类中根据此值查出附件表中的文件列表。

\(3\)渲染模板时，将文件列表传过去，js接收到文件列表后，调用百度控件API，将文件列表添加到百度控件文件序列。

![](/assets/WebUploader2.png)

##### 4、组件目录

![](/assets/webuploader.png)

