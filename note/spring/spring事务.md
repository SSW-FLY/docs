# spring 事务机制详解
Spring事务机制包括声明式事务和编程式，现在都是声明式事务。  
spring声明式事务操作简单，使得我们不必手动去获得连接，关闭连接，事务提交和回滚等这些操作。事务有四种事务属性：事务的传播行为，事务的隔离级别，事务超时时间，事务是否只读。

- 事务隔离级别
1. ISOLATION_DEFAULT：使用数据库默认隔离级别。
2. ISOLATION_READ_UNCOMMITTED：这是事务最低的隔离级别，别的事务可以看到这个事务未提交的数据，这种级别可以会产生脏读，不可重复读和幻读。
3. ISOLATION_READ_COMMITTED：读已提交，别的事务不能读取该事务未提交的数据。这个级别可以避免脏读。
4. ISOLATION_REPEATABLE_READ：这种级别一个事务不能读取另一个事务已经提交的数据外，还保证避免不可重复的。
5. ISOLATION_SERIALIZABLE：事务被处理为顺序执行，保持其串行化，可以避免脏读，不可重复读，幻读。

- 事务传播属性
1. PROPAGATION_REQUIRED：如果存在一个事务，则支持当前的事务。如果当前没有事务则开启一个新的事务。
2. PROPAGATION_SUPPORTS：如果存在一个事务，支持当前事务。如果没有事务，则非事务执行。
3. MANDATORY：如果存在一个事务，支持当前事务，如果没有一个活动的事务，则抛出异常。
4. REQUIRES_NEW：总是开启一个新的事务。
5. NOT_SUPPORTED：总是非事务的执行，并挂起存在事务。
6. NEVER：总是非事务的执行，如果存在一个活动事务，抛出异常。
7. NESTED：如果存在一个事务，则运行在一个嵌套的事务中，如果没有活动的事务，则按照REQUIRED属性执行
NEW 和NESTED 的区别主要是NEW情况下，内层事务提交后，外层事务不能对其进行回滚，两个事务互不影响，两个事务不是一个正真的嵌套事务。NESTED外层事务的回滚可以引起内层事务的回滚。而内存事务的异常不会导致外层事务的回滚

- 声明式事务  
声明式事务是基于AOP面向切面的，是spring的代理类实现了事务。
@Transactional的几种失效情况。
1. 注解作用在非public修饰的方法上，事务会失效。  
spring在实现时，会检查方法是不是public，如果不是public就不会获取注解的事务配置信息。其他类型的方法虽然有直接，但是注解无效，且不会报错。
2. 事务的传播级别设置出错。  
SUPPORTS：事务不存在，以非事务的方式继续运行
NOT_SUPPORTED：以非事务的方式运行，并挂起存在的事务。
NEVER：以非事务的方式运行，存在事务就报错。
3. 注解属性rollbackFor设置错误  
rollbackFor可以指定能够触发事务回滚的异常类型，Spring默认抛出了运行时异常或者error才回滚事务，其他异常不会触发回滚事务。如果对异常有要求需要设置
4. 同一个类中方法调用，注解失效

5. 异常被捕获，不能触发事务回滚