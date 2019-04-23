涉及到的配置、注解等

以及定时器并发控制等

# Spring定时任务的使用

#### springmvc-servlet.xml文件配置示例：

```
<!-- 加载spring定时任务扫描注解驱动,并设置线程池数量,这两个线程池数量(pool-size)最好>=定时任务数量 -->

<task:annotation-driven executor="myExecutor" scheduler="myScheduler"/>

<task:executor id="myExecutor" pool-size="2"/>

<task:scheduler id="myScheduler" pool-size="2"/>

<!-- 指定spring定时任务类所在包 -->

<context:component-scan base-package="com.cnnct.task"/>
```

#### 定时任务类示例：

```
@Component

public class TestTask {

/**
*  定时任务demo,每隔5秒执行一次
*/

    @Scheduled(cron = "0/5 * * * * ?")

    public void execute() {

        // do something

    }    
}
```

![](/assets/task2.png)

