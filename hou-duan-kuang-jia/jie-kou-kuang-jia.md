# ！！！接口日志开关等，配置、表等

# 接口框架

\*涉及到的文件：  
![](/assets/frontDoc_interface1.png)  
![](/assets/frontDoc_interface2.png)

* spring配置文件涉及内容：
  1.src下spring-servlet文件涉及内容：
  ![](/assets/frontDoc_interface3.png)
  2.core下spring-servlet文件涉及内容：
  ![](/assets/frontDoc_interface5.png)
  ![](/assets/frontDoc_interface4.png)
* 主业务控制文件：
  
  1.文件：
  ![](/assets/frontDoc_interface6.png)
  ![](/assets/frontDoc_interface7.png)
  2.版本加载：
  ![](/assets/interface_version4.png)
* 接口版本控制部分：
  1.配置文件
  ![](/assets/interface_version1.png)
  
  2.版本业务主代码：
  ![](/assets/interface_version2.png)
  ![](/assets/interface_version3.png)
* 接口日志部分：
  1.接口日志记录过程：
  通过接口开关配置可以获取是否开启接口日志记录，如果要记录接口日志，
  将根据sys_para表中配置的参数决定记录日志的规则
  
  ![](/assets/interface_version6.png)
  ![](/assets/interface_version5.png)
  
* 示例：
  ![](/assets/frontDoc_interface8.png)



