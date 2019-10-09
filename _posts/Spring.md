IoC(Inverse of Control),DI(Dependence Injection)

AOP(Aspect Oriented Programing)   -->动态代理

面向接口的编程。

注入的方式通常有俩种：一种是属性的set注入；一种是构造方法注入（推荐使用属性的set注入）
对于Ｂｅａｎ的autowire属性来说，推荐使用default，手动写入

convertion plugin.jar 注解，自己一套的命名规则，优先级高于xml,可能影响运行

- SpringAOP的实现手段
```
动态代理（Proxy,InvocationHandler）
Bytecode Instrument(字节码插桩)，通过CGLib实现
```

#####　事务
1. 声明式的事务管理
2. 编程式的事务管理
3. 事务的传播
```
PROPAGATION_REQUIRED:若当前没有事务，则新建一个事务；若已存在于一个事务中，则加入到该事务中。这是最常见的一种选择情况。
PROPAGATION_SUPPORTS:支持当前的事务，若当前没有事务，就以非事务的方式执行。
PROPAGATION_MANDATORY:使用当前事务，若当前没有事务，则抛出异常。
PROPAGATION_REQUIRES_NEW:新建事务，若当前存在的事务，则挂起当前的事务。
PROPAGATION_NOT_SUPPORTED:以非事务的方式执行，若当前存在事务，则挂起当前的事务。
PROPAGATION_NEVER:以非事务的方式执行，若当前存在事务，则抛出异常
PROPAGATION_NESTED:若当前存在事务，则在嵌套的事务中执行；若当前没有事务，则类似PROGATION_REQUIED
```

##### Java邮件发送　
1. JavaMail
2. SMTP(Simple Mail Transfer Protocol),简单邮件发送
3. POP3,邮局协议
4. 
5. MimeMessage
6.关于Ｗｅｂ下的单元测试(容器内测试、Mock测试)
