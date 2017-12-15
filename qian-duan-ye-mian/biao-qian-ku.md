标签库相关配置总体说明，若新开发一个标签要做哪些步骤等

其它个大的标签要重点说明如何实现，以及若动了源代码，则要列表哪些地方作了调整改造

我这里只列了表格，其它的复杂一点交互的都需要再重点说明一下，至少要说明到用到哪些关键的配置，类、css,js等


##一.新标签的开发说明：

* 1.以button标签为例，新增加标签时首先需要创建标签的java文件和ftl模板文件，文件位置如下：![](/assets/frontDoc_tag1.png)
![](/assets/frontDoc_tag2.png)

标签java类命名规则，配合ftl模板名，按驼峰命名规则，以Tag结尾，如cas_select_child.ftl对应的java类为casSelectChildTag.java,
以ButtonTag.java和button.ftl文件为例介绍标签编写规范：
![](/assets/frontDoc_tag3.png)