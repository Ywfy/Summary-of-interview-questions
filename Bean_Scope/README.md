# Spring Bean的作用域之间有什么区别

在Spring中，可以通过<bean>标签的scope属性设置bean的作用域<br>

scope可以有五种选择<br>

|类别|说明|
|:---|:---|
|Singleton|在Spring IOC容器中仅存在一个实例，Bean以单实例的方式存在|
|prototype|每次getBean()都获取一个新的实例|
|request|每次HTTP请求都会创建一个新的Bean,仅适用于WebApplicationContext环境|
|session|同一个Session共享一个Bean实例,仅适用于WebApplicationContext环境|
|global-session|所有的Session共享一个Bean实例,仅适用于WebApplicationContext环境|

Spring默认是采用Singleton的配置，且在创建IOC容器时就创建bean的对象实例。<br>

