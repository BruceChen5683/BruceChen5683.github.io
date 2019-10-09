##### 层级
1. 应用程序层、数据库层
2. 表述层、业务逻辑层、数据库层
3. 表述层、业务逻辑层、持久化层、数据库层
4. N层应用

#### 数据库　　
- 数据库事务指的是一组相互依赖的操作行为，只要一个操作行为失败，就意味着整个事务失败。e.g. A转账给Ｂ，俩步Ａ减钱、B加钱作为一个事务。
- 4个特性　**原子atom  持久性consistence　隔离性lsolation　持久性duration**
- Ehcache 
```
transactional 必须在受管的环境下使用，保存可重复读的事务隔离级别，对于读写比例大、很少更新的数据通常采用这个方式

read-write 使用timstamp机制维护已提交事务级别，对于读写比例大、很少更新的数据通常采用这个方式

nonstrict-read-write 二级缓存与数据库中的数据可能会出现不一致情况。在使用这种策略的时候，应该设置足够短的缓存过期时间，
                    否则就有可能从缓存读取到脏数据。当一些数据如果出现数据库与缓存不一致的情况下影响并不大的时候，那么可以采取这种缓存策略

read-only 当确定数据不会被改变时，可使用这个策略

```
- autocommit 事务自动提交
```
若没有效果　　查看默认引擎以及当前表的引擎
MyISAM和InnoDB区别：
MyISAM效率更高，但不支持事务，不支持外键。
InnoDB效率略低，支持事务和外键。
```
- 多个事务并发运行时的并发问题 **第一类丢失更新、脏读、虚读、不可重复读、第二类丢失更新**



##### 概念模型  关系数据模型
不要将具有业务含义的字段作为主键

中间表  外键
关联、依赖、聚集、一般化

##### JDBC链接步骤
```
DriverManger->Connection-->Statement           ->ResultSet
                        -->PreparedStatement   ->ResultSet
```
Hibernate要求持久化类，提供默认不带参数的构造

##### HQL SQL
Session get、load区别前者无数据返回空，后者抛出异常

## Ognl
1. Ognl  Object Graph Navigation Language 对象图导航语言
2. OgnlContext,存在唯一的叫做根的对象,可以通过程序设定上下午对象的跟对象
3. 在Ognl中，若表达式没有使用#号，会从根对象中找概述下对应的get方法
4. 当使用OGNL调用静态方法的时候，需要按照如下语法编写表达式：@packagename@methodname
5. 对于OGNL来说，java.lang.Math是默认类,如果调用java.lang.Math的静态方法时，无需指定类的名字，比如：@@min(4,10)
6. 对于OGNL来说，数组与集合是一样的，都是通过下标索引来访问的。构造集合的时候使用{...}形式
7. 使用OGNL来处理映射Map的语法形式如下：#{'key1':'value','key2':'value2',...}
8. 过滤 filtering :collection.{? expression}   ?符合表达式  ^符合条件的第一个 $符合条件的最后一个
9. Ognl针对集合提供来一些伪属性(e.g. size  isEmpty),让我们可以通过属性的方式来调用方法（本质原因在于集合中的许多方法命名不符合JavaBean的命名规则）
10. 在使用过滤操作时，我们通常都会使用#this,表达式用于代表当前正在迭代的集合中的对象（联想增强的for循环）
11. `投影（projection）: collection.{expression}`
12. 过滤与投影的差别，类似数据库中   过滤是取行 投影是取列
13. 在Struts2中有一个
14. 在Struts2中，根对象就是ValueStack。在Struts2的任何流程中，Valuestack中的最顶层对象一定是Action对象
15. parameters,#parameters.value    
16. 访问静态成员或静态变量
17. 关于struts2标签库属性值的%与#号的关系：
```
如果标签的属性值是 OGLNL的表达式，那么无需加上%{}
如果标签的属性值是字符串类型，那么在字符串当中凡是出现%{}都会被解析OGNL表达式，解析完毕后再与其他的字符串进行拼接构造出最后的字符串值
我们可以在所有的属性值上加上%{}，这样若属性值是OGNL表达式，那么标签处理类就会将%{}忽略掉.
```

##### Session的缓存
1. load & get
```
load  缓存   代理对象，在session关闭前，使用  回去数据库查询
get  数据库  对象本身
作用：减少访问数据库的频率 、保证缓存中的对象与数据库中的相关记录保持同步。多条相关的sql语句，合并为一条
```

2. 清理缓存的时间点：transaction commit | session flush

3. Session级别的缓存又叫做一级缓存；SessionFactory级别的缓存叫做二级缓存

4. **临时状态transient**

5. **持久化状态persistent**

6. **游离状态detached**

##### FAQ
1. org.hibernate.boot.MappingException: column attribute may not be specified along with 
```
 property元素不需要column子元素
```

2. 对于Query接口的list()方法和iterator()方法来说，都可以实现获取查询的对象，但是list()方法返回的每个对象都是完整的（对象中的每个属性都被表中的字段填充上了），
而iterator()方法所返回的对象中仅包含了主键值(标识符)，只有当你对iterator()中的对象进行操作时，Hibernate才会向数据库再次发送SQL语句来获取该对象的属性值.  赖加载

3. Hibernate中的延迟加载（lazy loading）,当我们在程序中获取到一的一方，但是不需要多的一方，那么使用延迟加载就是非常适合的。
```
多的一方 lazy默认true延迟加载  关闭，使用lazy true
```

4. mysql max_allowed_packet向mysql传入最大的数据
Timestamp时间

5. .cfg.xml运行时找不到
```
重置artifacts
```

## Hibernate
- 一对多
- 多对一
- 一对一
1. 主键关联 一对一默认使用的是立即加载，如果需要使用延迟加载，需要在one-to-one中将constrained属性设为true,并且将待加载的一方的class元素中的lazy属性设为true（或者不去设置，因为该属性默认值就是true）。一对一加载时默认使用左外连接，可以通过修改fetch属性为select修改成每次发送一条select语句的形式   constrained true,先次后主update  无contrained 先主后此无update

2. 外键关联。本质是一对多的锐化形式。在many-to-one元素中在增加unique=true

3. 外键不可主动设置

- 检索策略
```
立即检索：
延迟检索：适用范围一对多，多对多，不需要立即访问或根本不需要访问的对象
左外连接检索：适用范围多对一或者一对一关联、应用需要立即访问的对象、数据库具有良好的表连接性能
```
- 自动创建数据库
```
Hibernate5.2.10使用SchemaExport创建数据库

	//默认读取hibernate.cfg.xml文件
	Configuration cfg = new Configuration().configure();
	
	SchemaExport export = new SchemaExport(cfg);
	export.create(true, true);

可是到了5.2之后，SchemaExport类变化已经挺大的了，再使用上面的方式已经没法实现了
    ServiceRegistry registry = new StandardServiceRegistryBuilder().configure().build();
    Metadata metadata = new MetadataSources(registry).buildMetadata();
    SchemaExport export = new SchemaExport();
    export.create(EnumSet.of(TargetType.DATABASE),metadata);


```

- Bag(结合List与Set),可以重复且没有顺序的一种集合，是Hibernate提供的。

- 查询排序（内存排序以及数据库排序）
```
数据库排序使用order-by 属性
内存排序使用sort属性它有三个属性值 unsorted,natural,其中natural指的事按照自然的升序排序。第三个属性值是我们自己定义的排序规则类。
```

- 联合主键的映射规则 **联合主键的实体类实现Serializable接口的原因在于使用get或load方法的时候需要先构建出来该实体的对象，并且将查询依据（联合主键）设置进去，然后作为get或load方法的第二个参数传进去即可。**
```
类中的每个主键属性都对应到数据表中的每个主键列。Hibernate要求具有联合主键的实体类实现Serializable接口，并且重写hashCode与equals方法，重写这俩个方法的原因在于Hibernate要根据数据库的联合主键来判断某俩行记录是否是一样的，如果一样那么就认为是同一个对象，
如果不一样，那么就认为是不同的对象。这反映到程序领域中就是根据hashCode与equals方法来判断某俩个对象是否能够放到诸如Set这样的集合当中。

联合主键的实体类实现Serializable接口的原因在于使用get或load方法的时候需要先构建出来该实体的对象，并且将查询依据（联合主键）设置进去，然后作为get或load方法的第二个参数传进去即可。
```
方法链编程风格 简洁

#### 继承映射
1. 每一个子类对应一张表 
无hbm文件，需要加入包的名字
2. 一张表存储继承体系中所有类的信息（数据库实际上是继承体系中所有类的属性的并集所构成的字段）
需要在hbm文件中增加如下一行：discriminator column="PersonType" type="string"
3. 公共信息放在父类表中，独有信息放在子类中，每个子类对应一张表

##### query.setParameter?

#### 连接池
1. 数据库连接池（ConnectionPool）。  CP30,Apache DBCP
2. JNID(java命名与目录接口)