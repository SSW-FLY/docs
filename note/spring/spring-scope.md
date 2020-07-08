# @Scope

该注解用来描述spring容器中如何新建Bean的实例  
|Singleton|Prototype|Request|Session|GlobalSession|
|:--------|:--------|:------|:------|:------------|
|一个spring容器中只有一个Bean的实例，spring的默认配置，全局共享一个实例|每次调用都会新建一个Bean的实例|Web项目中，给每个http request新建一个bean实例|Web项目中，给每一个http session新建一个bean实例|给每一个global http session新建一个bean实例
