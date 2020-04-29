# spring-boot 多线程
## 使用配置文件的方式
> 参考文章
> `https://mp.weixin.qq.com/s?__biz=MzI4Njc5NjM1NQ==&mid=2247491492&idx=1&sn=1a12af326243f195a1b688a7f5e0fae3&chksm=ebd62088dca1a99eab0252520f0154b7bdc09b544e9dfae448573d2e891ff9752b7904cfb5ac&scene=21#wechat_redirect`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:task="http://www.springframework.org/schema/task"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                        http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task.xsd">

    <!-- 开启异步，并引入线程池 -->
    <task:annotation-driven executor="threadPool" />

    <!-- 定义线程池 -->
    <bean id="threadPool"
        class="org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor">
        <!-- 核心线程数，默认为1 -->
        <property name="corePoolSize" value="10" />

        <!-- 最大线程数，默认为Integer.MAX_VALUE -->
        <property name="maxPoolSize" value="50" />

        <!-- 队列最大长度，一般需要设置值>=notifyScheduledMainExecutor.maxNum；默认为Integer.MAX_VALUE -->
        <property name="queueCapacity" value="100" />

        <!-- 线程池维护线程所允许的空闲时间，默认为60s -->
        <property name="keepAliveSeconds" value="30" />

        <!-- 完成任务自动关闭 , 默认为false-->
        <property name="waitForTasksToCompleteOnShutdown" value="true" />

        <!-- 核心线程超时退出，默认为false -->
        <property name="allowCoreThreadTimeOut" value="true" />

        <!-- 线程池对拒绝任务（无线程可用）的处理策略，目前只支持AbortPolicy、CallerRunsPolicy；默认为后者 -->
        <property name="rejectedExecutionHandler">
            <!-- AbortPolicy:直接抛出java.util.concurrent.RejectedExecutionException异常 -->
            <!-- CallerRunsPolicy:主线程直接执行该任务，执行完之后尝试添加下一个任务到线程池中，可以有效降低向线程池内添加任务的速度 -->
            <!-- DiscardOldestPolicy:抛弃旧的任务、暂不支持；会导致被丢弃的任务无法再次被执行 -->
            <!-- DiscardPolicy:抛弃当前任务、暂不支持；会导致被丢弃的任务无法再次被执行 -->
            <bean class="java.util.concurrent.ThreadPoolExecutor$CallerRunsPolicy" />
        </property>
    </bean>
</beans>
```

## 使用注解的方式(推荐)
`