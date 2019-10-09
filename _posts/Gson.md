##### 1. 简介
```
java对象 <--> Json
使用简单toJson() fromJson()
```

##### 2. 使用说明
1. 性能以及扩展性  **性能测试  模仿**
```
Here are some metrics that we obtained on a desktop (dual opteron, 8GB RAM, 64-bit Ubuntu) running lots of other things along-with the tests. You can rerun these tests by using the class PerformanceTest.
Strings: Deserialized strings of over 25MB without any problems (see disabled_testStringDeserializationPerformance method in PerformanceTest)
Large collections:
Serialized a collection of 1.4 million objects (see disabled_testLargeCollectionSerialization method in PerformanceTest)
Deserialized a collection of 87,000 objects (see disabled_testLargeCollectionDeserialization in PerformanceTest)
Gson 1.4 raised the deserialization limit for byte arrays and collection to over 11MB from 80KB.
Note: Delete the disabled_ prefix to run these tests. We use this prefix to prevent running these tests every time we run JUnit tests.
```

2. 使用 new Gson() 或者通过GsonBuilder配置

3. demo
```
// Serialization
Gson gson = new Gson();
gson.toJson(1);            // ==> 1
gson.toJson("abcd");       // ==> "abcd"
gson.toJson(new Long(10)); // ==> 10
int[] values = { 1 };
gson.toJson(values);       // ==> [1]
// Deserialization
int one = gson.fromJson("1", int.class);
Integer one = gson.fromJson("1", Integer.class);
Long one = gson.fromJson("1", Long.class);
Boolean false = gson.fromJson("false", Boolean.class);
String str = gson.fromJson("\"abc\"", String.class);
String[] anotherStr = gson.fromJson("[\"abc\"]", String[].class);
//Other
class BagOfPrimitives {
  private int value1 = 1;
  private String value2 = "abc";
  private transient int value3 = 3;
  BagOfPrimitives() {
    // no-args constructor
  }
}
// Serialization
BagOfPrimitives obj = new BagOfPrimitives();
Gson gson = new Gson();
String json = gson.toJson(obj);  
// ==> json is {"value1":1,"value2":"abc"}
```

4. **注意**
```
Note that you can not serialize objects with circular references since that will result in infinite recursion.
// Deserialization
BagOfPrimitives obj2 = gson.fromJson(json, BagOfPrimitives.class);
// ==> obj2 is just like obj
```
5. Finer Points with Objects **If a field is synthetic, it is ignored and not included in JSON serialization or deserialization.**
 &**Fields corresponding to the outer classes in inner classes, anonymous classes, and local classes are ignored and not included in serialization or deserialization.**
```
私有
不需要注解There is no need to use any annotations to indicate a field is to be included for serialization and deserialization. All fields in the current class (and from all super classes) are included by default.
临时变量默认被忽略If a field is marked transient, (by default) it is ignored and not included in the JSON serialization or deserialization.
可正确处理null
序列化时，a null field is omitted（忽略） from the output.
**If a field is synthetic, it is ignored and not included in JSON serialization or deserialization.**
**Fields corresponding to the outer classes in inner classes, anonymous classes, and local classes are ignored and not included in serialization or deserialization.**
```

6. Nested Classes (including Inner Classes) Gson can serialize static nested classes quite easily.Gson can also deserialize static nested classes. However, Gson can **not** automatically deserialize the **pure inner classes since their no-args constructor also need a reference to the containing Object** which is not available at the time of deserialization. You can address this problem by either making the inner class static or by providing a custom InstanceCreator for it. Here is an example:
```
public class A {
  public String a;

  class B {

    public String b;

    public B() {
      // No args constructor for B
    }
  }
}
 The above class B can not (by default) be serialized with Gson.
 Gson can not deserialize {"b":"abc"} into an instance of B since the class B is an inner class. If it was defined as static class B then Gson would have been able to deserialize the string. Another solution is to write a custom instance creator for B.
 public class InstanceCreatorForB implements InstanceCreator<A.B> {
  private final A a;
  public InstanceCreatorForB(A a)  {
    this.a = a;
  }
  public A.B createInstance(Type type) {
    return a.new B();
  }
}
The above is possible, but not recommended.
```

7. 数组 We also support multi-dimensional arrays, with arbitrarily complex element types.
```
Gson gson = new Gson();
int[] ints = {1, 2, 3, 4, 5};
String[] strings = {"abc", "def", "ghi"};
// Serialization
gson.toJson(ints);     // ==> [1,2,3,4,5]
gson.toJson(strings);  // ==> ["abc", "def", "ghi"]
// Deserialization
int[] ints2 = gson.fromJson("[1,2,3,4,5]", int[].class);
// ==> ints2 will be same as ints
```

8. collections Examples **Fairly hideous: note how we define the type of collection. Unfortunately, there is no way to get around this in Java.** 可序列化集合，不可反序列化集合。因为无法预知返回值的类型。只可反序列化一些特殊的以及通用的集合类型。
```
Gson gson = new Gson();
Collection<Integer> ints = Lists.immutableList(1,2,3,4,5);
// Serialization
String json = gson.toJson(ints);  // ==> json is [1,2,3,4,5]
// Deserialization
Type collectionType = new TypeToken<Collection<Integer>>(){}.getType();
Collection<Integer> ints2 = gson.fromJson(json, collectionType);
// ==> ints2 is same as ints
```

9. Serializing and Deserializing Generic Types   **TypeToken** &&& **You can solve this problem by specifying the correct parameterized type for your generic type. You can do this by using the TypeToken class.**
```
class Foo<T> {
  T value;
}
Gson gson = new Gson();
Foo<Bar> foo = new Foo<Bar>();
gson.toJson(foo); // May not serialize foo.value correctly
gson.fromJson(json, foo.getClass()); // Fails to deserialize foo.value as Bar
The above code fails to interpret value as type Bar because Gson invokes list.getClass() to get its class information, but this method returns a raw class, Foo.class. This means that Gson has no way of knowing that this is an object of type Foo<Bar>, and not just plain Foo.
//Ok---------------
You can solve this problem by specifying the correct parameterized type for your generic type. You can do this by using the TypeToken class.
Type fooType = new TypeToken<Foo<Bar>>() {}.getType();
gson.toJson(foo, fooType);
gson.fromJson(json, fooType);
```

10. Serializing and Deserializing Collection with Objects of Arbitrary Types  序列化toJson(collection) 反序列化需要额外处理
```
三种方式解决
AAA
通过Jsonparser
public class RawCollectionsExample {
  static class Event {
    private String name;
    private String source;
    private Event(String name, String source) {
      this.name = name;
      this.source = source;
    }
    @Override
    public String toString() {
      return String.format("(name=%s, source=%s)", name, source);
    }
  }

  @SuppressWarnings({ "unchecked", "rawtypes" })
  public static void main(String[] args) {
    Gson gson = new Gson();
    Collection collection = new ArrayList();
    collection.add("hello");
    collection.add(5);
    collection.add(new Event("GREETINGS", "guest"));
    String json = gson.toJson(collection);
    System.out.println("Using Gson.toJson() on a raw collection: " + json);
    JsonParser parser = new JsonParser();
    JsonArray array = parser.parse(json).getAsJsonArray();
    String message = gson.fromJson(array.get(0), String.class);
    int number = gson.fromJson(array.get(1), int.class);
    Event event = gson.fromJson(array.get(2), Event.class);
    System.out.printf("Using Gson.fromJson() to get: %s, %d, %s", message, number, event);
  }
}
BBB
Register a type adapter for Collection.class that looks at each of the array members and maps them to appropriate objects. The disadvantage of this approach is that it will screw up deserialization of other collection types in Gson.
CCC
Register a type adapter for MyCollectionMemberType and use fromJson() with Collection<MyCollectionMemberType>.
This approach is practical only if the array appears as a top-level element or if you can change the field type holding the collection to be of type Collection<MyCollectionMemberType>.
```

11. **自定义序列化以及反序列化**
```
Json Serializers: Need to define custom serialization for an object
Json Deserializers: Needed to define custom deserialization for a type
Instance Creators: Not needed if no-args constructor is available or a deserializer is registered
//Demo
GsonBuilder gson = new GsonBuilder();
gson.registerTypeAdapter(MyType2.class, new MyTypeAdapter());
gson.registerTypeAdapter(MyType.class, new MySerializer());
gson.registerTypeAdapter(MyType.class, new MyDeserializer());
gson.registerTypeAdapter(MyType.class, new MyInstanceCreator());
//Writing a Serializer
Here is an example of how to write a custom serializer for JodaTime DateTime class.
private class DateTimeSerializer implements JsonSerializer<DateTime> {
  public JsonElement serialize(DateTime src, Type typeOfSrc, JsonSerializationContext context) {
    return new JsonPrimitive(src.toString());
  }
}
Gson calls serialize() when it runs into a DateTime object during serialization.
Writing a Deserializers
private class DateTimeDeserializer implements JsonDeserializer<DateTime> {
  public DateTime deserialize(JsonElement json, Type typeOfT, JsonDeserializationContext context)
      throws JsonParseException {
    return new DateTime(json.getAsJsonPrimitive().getAsString());
  }
}
```

12. finer points with Serializers and Deserializers**
```
Often you want to register a single handler for all generic types corresponding to a raw type
For example, suppose you have an Id class for id representation/translation (i.e. an internal vs. external representation).
Id<T> type that has same serialization for all generic types
Essentially write out the id value
Deserialization is very similar but not exactly the same
Need to call new Id(Class<T>, String) which returns an instance of Id<T>
Gson supports registering a single handler for this. You can also register a specific handler for a specific generic type (say Id<RequiresSpecialHandling> needed special handling). The Type parameter for the toJson() and fromJson() contains the generic type information to help you write a single handler for all generic types corresponding to the same raw type.
```

13. Writing an Instance Creator ***a library class that does NOT define a no-argument constructor***
```
Typically, Instance Creators are needed when you are dealing with a library class that does NOT define a no-argument constructor
private class MoneyInstanceCreator implements InstanceCreator<Money> {
  public Money createInstance(Type type) {
    return new Money("1000000", CurrencyCode.USD);
  }
}
//TODO
InstanceCreator for a Parameterized Type 参数化类型
Sometimes the type that you are trying to instantiate is a parameterized type. Generally, this is not a problem since the actual instance is of raw type. Here is an example:
class MyList<T> extends ArrayList<T> {
}
class MyListInstanceCreator implements InstanceCreator<MyList<?>> {
    @SuppressWarnings("unchecked")
  public MyList<?> createInstance(Type type) {
    // No need to use a parameterized list since the actual instance will have the raw type anyway.
    return new MyList();
  }
}
//*****
However, sometimes you do need to create instance based on the actual parameterized type. In this case, you can use the type parameter being passed to the createInstance method. Here is an example:
public class Id<T> {
  private final Class<T> classOfId;
  private final long value;
  public Id(Class<T> classOfId, long value) {
    this.classOfId = classOfId;
    this.value = value;
  }
}
class IdInstanceCreator implements InstanceCreator<Id<?>> {
  public Id<?> createInstance(Type type) {
    Type[] typeParameters = ((ParameterizedType)type).getActualTypeArguments();
    Type idType = typeParameters[0]; // Id has only one parameterized type T
    return Id.get((Class)idType, 0L);
  }
}
//TODO
In the above example, an instance of the Id class can not be created without actually passing in the actual type for the parameterized type. We solve this problem by using the passed method parameter, type. The type object in this case is the Java parameterized type representation of Id<Foo> where the actual instance should be bound to Id<Foo>. Since Id class has just one parameterized type parameter, T, we use the zeroth element of the type array returned by getActualTypeArgument() which will hold Foo.class in this case.
```

14. Compact Vs. Pretty Printing for JSON Output Format 默认json无空白**JsonPrintFormatter 代替JsonFormat**
```
For now, we only provide a default JsonPrintFormatter that has default line length of 80 character, 2 character indentation, and 4 character right margin.
e.g.Gson gson = new GsonBuilder().setPrettyPrinting().create();
String jsonOutput = gson.toJson(someObject);
```

15. Null Object Support
```
public class Foo {
  private final String s;
  private final int i;
  public Foo() {
    this(null, 5);
  }
  public Foo(String s, int i) {
    this.s = s;
    this.i = i;
  }
}
Gson gson = new GsonBuilder().serializeNulls().create();
Foo foo = new Foo();
String json = gson.toJson(foo);
System.out.println(json);
json = gson.toJson(null);
System.out.println(json);
输出：
{"s":null,"i":5}
null
```

16. 不同版本控制。@Since  默认全部serialize和deserialize。可根据版本选择序列化，反序列的变量
```
public class VersionedClass {
  @Since(1.1) private final String newerField;
  @Since(1.0) private final String newField;
  private final String field;

  public VersionedClass() {
    this.newerField = "newer";
    this.newField = "new";
    this.field = "old";
  }
}

  VersionedClass versionedObject = new VersionedClass();
  Gson gson = new GsonBuilder().setVersion(1.0).create();
  String jsonOutput = gson.toJson(someObject);
  System.out.println(jsonOutput);
  System.out.println();

  gson = new Gson();
  jsonOutput = gson.toJson(someObject);
  System.out.println(jsonOutput);
  输出:
  {"newField":"new","field":"old"}

  {"newerField":"newer","newField":"new","field":"old"}
```

17. 部分域 排除序列化、反序列化 **@Expose** ***User Defined Exclusion Strategies***
```
import java.lang.reflect.Modifier;
Gson gson = new GsonBuilder()
    .excludeFieldsWithModifiers(Modifier.STATIC)
    .create();

    Gson gson = new GsonBuilder()
    .excludeFieldsWithModifiers(Modifier.STATIC, Modifier.TRANSIENT, Modifier.VOLATILE)
    .create();

    @Retention(RetentionPolicy.RUNTIME)
    @Target({ElementType.FIELD})
    public @interface Foo {
      // Field tag only annotation
    }

    public class SampleObjectForTest {
      @Foo private final int annotatedField;
      private final String stringField;
      private final long longField;
      private final Class<?> clazzField;

      public SampleObjectForTest() {
        annotatedField = 5;
        stringField = "someDefaultValue";
        longField = 1234;
      }
    }

    public class MyExclusionStrategy implements ExclusionStrategy {
      private final Class<?> typeToSkip;

      private MyExclusionStrategy(Class<?> typeToSkip) {
        this.typeToSkip = typeToSkip;
      }

      public boolean shouldSkipClass(Class<?> clazz) {
        return (clazz == typeToSkip);
      }

      public boolean shouldSkipField(FieldAttributes f) {
        return f.getAnnotation(Foo.class) != null;
      }
    }

    public static void main(String[] args) {
      Gson gson = new GsonBuilder()
          .setExclusionStrategies(new MyExclusionStrategy(String.class))
          .serializeNulls()
          .create();
      SampleObjectForTest src = new SampleObjectForTest();
      String json = gson.toJson(src);
      System.out.println(json);
    }
    The output is:

    {"longField":1234}
```

18. JSON Field Naming Support **FieldNamingPolicy**  
```
private class SomeObject {
  @SerializedName("custom_naming") private final String someField;
  private final String someOtherField;

  public SomeObject(String a, String b) {
    this.someField = a;
    this.someOtherField = b;
  }
}
SomeObject someObject = new SomeObject("first", "second");
Gson gson = new GsonBuilder().setFieldNamingPolicy(FieldNamingPolicy.UPPER_CAMEL_CASE).create();
String jsonRepresentation = gson.toJson(someObject);
System.out.println(jsonRepresentation);
输出
{"custom_naming":"first","SomeOtherField":"second"}
```

19. Sharing State Across Custom Serializers and Deserializers
```
Store shared state in static fields
Declare the serializer/deserializer as inner classes of a parent type, and use the instance fields of parent type to store shared state
Use Java ThreadLocal
1 and 2 are not thread-safe options, but 3 is.
```

20. Streaming
```
you can use Gson to read from and write to a stream. You can also combine streaming and object model access to get the best of both approaches.
```
