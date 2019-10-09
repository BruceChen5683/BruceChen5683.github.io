1. Retrofit
```
动态 URL 地址就是用在连续请求里的。在第一个请求之后，如果返回的结果里有指明下个请求的地址的话，在之前，你可能得单独写个 interface 来处理这种情况，现在就无需那么费事了。


看我们上面的代码：第一个方法返回了一个 proto 对象。

SomeProtoResponse —> Proto? Yes!

原理很简单，其实就是对着每一个 converter 询问他们是否能够处理某种类型。我们问 proto 的 converter： “Hi, 你能处理 SomeProtoResponse 吗？”，然后它尽可能的去判断它是否可以处理这种类型。我们都知道：Protobuff 都是从一个名叫 message 或者 message lite 的类继承而来。所以，判断方法通常就是检查这个类是否继承自 message。

在面对 JSON 类型的时候，首先问 proto converter，proto converter 会发现这个不是继承子 Message 的，然后回复 no。紧接着移到下一个 JSON converter 上。JSON Converter 会回复说我可以！

SomeJsonResponse —> Proto? No! —> JSON? Yes!

因为 JSON 并没有什么继承上的约束。所以我们无法通过什么确切的条件来判断一个对象是否是 JSON 对象。以至于 JSON 的 converters 会对任何数据都回复说：我可以处理！这个一定要记住， JSON converter 一定要放在最后，不然会和你的预期不符。

```

2. 热修复
```
QQ空间超级补丁技术


微信Tinker



阿里百川HotFix

```