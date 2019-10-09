MVC框架

存在大量用户界面、业务逻辑复杂

ognl xwork

用户需求，产品提炼需求，开放实现,运营？测试？

StringTokenizer解析字符串分隔符
******
1. 对应***自定义的转换器***来说，需要提供三个信息 Action的名字、Action中待转行的属性名以及该属性对应的类型转换器
struts2自带的转换器StrutsTypeConverter  **全局类型转换**，在src目录下新建xwork-conversion.properties,该文件的内容是待转换的类=转换器的名字
**ActionError级别错误消息  FieldError级别错误消息**
```执行流程：
首先进行类型转换
然后进行输入校验
如果在上述方法中出现任何错误，都不会再去执行execute方法。页面也会专项struts.xml中该aciton的input对应的页面
```

2. ActionSupport类的addActionError()方法的实现：首先创建ArrayList对象，然后将错误消息添加到ArrayList对象中
当调用getActionErrors()方法返回Action级别的错误消息列表是，实际是集合的一个副本


9. Action中自定义方法的输入校验.对于通过action的methon自定方法，其自定义的输入校验方法名为validateMyExecute(假设自定义方法名为myExecute).底层通过反射调用

10.  validateMyExecute validate

11. 自定义Field级别的错误提示消息
1)新建Action命名的properties文件，e.g. RegisterAction.properties
2)在属性文件中指定每一个出错字段的错误消息  e.g.  invalid.fieldvalue.age=age invalid!!!

native2ascii   语言转为ascii
 对于国际化的资源文件，命名规则  package_语言名_国家名

 MessageFormat


 invalid birthday   浏览器，语言设置  中英文格式不一样


### Strutsk框架校验先后顺序
1. 校验框架xml文件
2. 执行自定义的校验方法  validateMyExecute
3. 执行validate


可以在Action中定义异常和结果，全局以及局部。  局部优先级高于全局。

struts2应用分层体系
   action  <------   service <--------- DAO

struts2  **模型驱动 Model Driven** ,以前使用的是属性驱动
属性驱动与模型驱动的比较：
1. 属性驱动灵活，准确；模型驱动不灵活，
因为许多时候，页面所提交过来的参数并不属于模型中的属性，也就是说页面所提交过来的参数与模型中的属性并不一致，这是很常见的情况
2. 模型驱动更加符合面向对象的编程风格，使得我们获得的是对象而不是一个个离散的值。
**小结：推荐使用属性驱动编写Action**


服务器端代码的单元测试有俩种模式：
1）容器内测试(Jetty)
2) Mock测试 (继承HttpServletRequest,HttpSession,HttpServletReponse等ServletAPI   jmock   代理等EasyMock)


Preparable接口的作用是让Action完成一些初始化工作，这些初始化工作是放在Preparable接口的prepare方法中完成的，该方法会在execute方法执行之前得到调用.

采取请求转发的方式完成表单内容的数据的添加，会导致数据重复  **采取重定向的方式实现数据的添加不会导致数据重复**

 防止表单重复提交的方式：
 1. 通过重定向
 2. 通过session token(Session令牌):当客户端请求页面时，服务器通过token标签生成一个随机数，并且将该随机数放置到session当中，然后将该随机数发向客户端;如果客户第一次提交，会将随机数发往服务器端，服务器会接收该随机数并且与session中所保存的随机数进行比较，这时俩者的值是相同的，服务器认为第一次提交，并且将更新服务器的这个随机数值；如果此时重复提交，客户端随机不变，服务器的随机数已经发生改变.服务器认为重复提交，进而转向invalid.token所指向的结果页面。


##### 拦截器的配置  must be stateless (思考，无状态即没有成员变量  为什么struts.xml中可设置parm，然后获取)
1.  编写实现Interceptor接口的类
2.  在struts.xml中定义拦截器
3.  在action中使用拦截器

一旦定义了自己的拦截器，将其配置到aciton后，我们需要在action的最后加上默认的拦截器栈：defaultStack

定义拦截器时可以直接继承AbstractInterceptor抽象类（该类实现类Interceptor接口，并且对init和destroy方法进行类空实现），然后实现其抽象方法 interceptor即可

方法过滤拦截器（可以对指定方法进行拦截的拦截器）

**在方法过滤拦截器中，如果既没有指定includeMethods参数，也没有指定execludeMethods参数，那么所有的方法都会被拦截，也就是说所有的方法都被认为是includeMethods的。** 如果仅仅指定类includeMethods,那么只会拦截includeMthods中的，没有包含在includeMethods中的方法就不会被拦截.


struts2,fileUpload request.getInputStream readLine() = null???

###### struts2文件上传
1. 首先将客户端上传的文件保存到struts.multipart.saveDir建所指定的目录中，如果该建所对应的目录不存在，那么保存到javax.servlet.context.tempdir环境变量所指的目录中

2. action中所定义的File类型的成员变量file实际上指向的是临时目录中的临时文件，然后在服务器通过IO的方式将临时文件写入到指定的服务器目录中。

#### OGNL(Object Graph Navigation Language):对象图导航语言  表达式语言


struts.properties优先级大于strus.xml constant


1. [java.lang.ClassNotFoundException: org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter](https://blog.csdn.net/njnu_mjn/article/details/6684661)

2. Caused by: java.lang.IllegalArgumentException: Javassist library is missing in classpath! Please add missed dependency! 
```
缺少javassist-ga.jar包
```

