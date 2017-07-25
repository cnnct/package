# java打包混淆说明

总共分两大步：

```
1.从eclipse等IDE中导出jar包

2.使用proguard混淆jar包中代码
```

一、从eclipse等IDE中导出jar包

此处以eclipse为例，右击项目----&gt;Export----&gt;Export----&gt;选择Java下的JAR file----&gt;Next----&gt;取消Select the resources to export:下面所有已选，然后选中需要导出jar包项目的所有java源代码，不含有java源代码的目录选不选随意----&gt;选中Export generated class files and resources----&gt;点击Browse选中导出目录----&gt;选中Compress the contents of the JAR files和Add directory entries----&gt;这里没提到的选项最好不要选中----&gt;点击Finish----&gt;完成。

示例图如下：

![](/assets/exportjar.png)

二、使用proguard混淆jar包中代码



