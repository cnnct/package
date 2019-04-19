# java打包混淆说明

总共分两大步：

1.从eclipse等IDE中导出jar包，

> 2019-04-18，pengm,在打包2.8版本的jar包时，单独运行打包后的web项目时，加载出现奇怪问题，最后我直接把classes目录下的increator目录和baseconfig目录两个一起压成zip文件，后缀改成jar，又能正常使用。
>
> ![](/assets/jar_export.png)

2.使用proguard混淆jar包中代码

一、从eclipse等IDE中导出jar包

此处以eclipse为例，右击项目----&gt;Export----&gt;Export----&gt;选择Java下的JAR        file----&gt;Next----&gt;取消Select the resources to export:下面所有已选，然后选中需要导出jar包项目的所有java源代码，不含有java源代码的目录选不选随意----&gt;选中Export generated class files and resources----&gt;点击Browse选中导出目录----&gt;选中Compress the contents of the JAR files和Add directory entries----&gt;这里    没提到的选项最好不要选中----&gt;点击Finish----&gt;完成。

示例如下：

![](/assets/exportjar.png)

二、使用proguard混淆jar包中代码

1.proguard下载启动

去proguard官网

[https://www.guardsquare.com/en/proguard](https://www.guardsquare.com/en/proguard)

下载后解压，运行proguardgui.bat。

2.proguard配置（示例所用版本为5.3.3）

\(1\)Input/Output：点击Add input选择要混淆的jar包，点击Add output选择混淆后jar导出目录，点击Add添加jar包引用，即eclipse项目Build Path中Libraries下所有jar包，包括jre，示例如下：

![](/assets/input/output.png)

\(2\)Shrinking：所有选项都不要选中。

\(3\)Obfuscation：选项如下：

![](/assets/obfuscation.png)

\(4\)Optimization：所有选项都不要选中。

\(5\)Information：选项如下：

![](/assets/information.png)

\(6\)Process：如上所示几步配置全部完成后，在这里点击Save configuration保存当前配置到配置文件中，配置文件后缀名为.pro。

\(7\)在电脑中用文本编辑器打开刚刚保存的配置文件，修改配置文件，如下所示：

![](/assets/settingfile.png)

配置含义：

—keep：此软件保留不混淆关键字

public class：表示java类

increator.core.entity.\_\_：表示此包下所有java类（包含子目录中的类）

{\\*;}：表示java类中属性名、方法名

{&lt;fileds&gt;;}：表示java类中属性名

{&lt;methods&gt;;}：表示java类中方法名

所以下面这句话

—keep public class increator.base.\*\* {&lt;methods&gt;;}

意思就是保留increator.base包下所有类的类名及方法名。

再下面这句话

—keep public class increator.core.entity.\*\* {\*;}

意思就是保留increator.core.entity包下所有类的类名、属性名、方法名（也就是都不混淆）。

\(8\)ProGuard：重新回到proguard软件界面----&gt;点击ProGuard下面的Load configuration选择刚刚修改完的配置文件----&gt;点击Process下的Process!开始混淆----&gt;完成。

