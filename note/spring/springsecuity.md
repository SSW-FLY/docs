# springsecurity

## 1.核心组件
- SecurityContextHolder

SecurityContextHolder是用来存储安全上下文信息，当前用户是谁，是否被认证，具有哪些权限都被存放在SecurityContextHolder中。SecurityContextHolder默认使用ThreadLocal策略存储认证信息。这是一种线程绑定策略。Spring Security在用户登录时自动绑定认证信息到当前线程，在用户退出时，自动清理当前线程的认证信息。

```java
Object principal = SecurityContextHolder.getContext().getAuthentication().getPrincipal();
```
getAuthentication()返回了认证信息，再次getPrincipal()返回了身份信息，UserDetails便是Spring对身份信息封装的一个接口。

## 2.spring security 是如何完成身份认证
1. 用户名和密码被过滤器获取到，封装成Authentication，通常情况下是UsernamePasswordAuthenticationToken这个实现类。

2. AuthenticationManager身份管理器负责验证这个Authentication

3. 认证成功后，AuthenticationManager身份管理器返回一个被填满了信息的Authentication实例。

4. SecurityContextHolder 安全上下文容器将之前返回的Authentication保存到上下文环境中。

在第三步中出现的AuthenticationManager(interface)是认证相关的核心接口，也是发起认证的出发点，在现实中，我们登录除了账号 + 密码，还有手机号 + 密码，邮箱 + 密码等。AuthenticationManager不直接进行认证，但是他的一个实现类ProviderManager内部会维护一个List<AuthenticationProvider>列表，存放多种认证方式，这是一种委托者模式。核心认证入口只有一个：AuthenticationManager，不同的认证方式对应不用的AuthenticationProvider。
ProviderManager中的List，会依照次序去认证，认证成功则立即返回，若认证失败返回null，下一个AuthenticationProvider会继续尝试认证，所有认证器都无法认证成功，ProviderManager会抛出一个异常。

