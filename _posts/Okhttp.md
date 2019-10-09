##### 1. 简介 **HTTP/2 support allows all requests to the same host to share a socket.**

如果服务有多个地址，okhttp在第一次请求失败后，会自动寻址。
首先使用TLS如果握手失败，回退到TLS1.0 。OkHttp initiates new connections with modern TLS features (SNI, ALPN), and falls back to TLS 1.0 if the handshake fails.
支持同步&异步
OkHttp supports Android 2.3 and above. For Java, the minimum requirement is 1.7.
```
HTTP/2 support allows all requests to the same host to share a socket.
http不可时，减少连接池请求延迟Connection pooling reduces request latency (if HTTP/2 isn’t available).
gzip减少下载大小
响应缓存避免重复请求
```

##### 2. 示例  
1. **new Request.Builder().url(url).post(body).build();** ----- **new Request.Builder().url(url).build();**

```
GET
This program downloads a URL and print its contents as a string

OkhttpClient client = new OkhttpClient();
String getContent(String url) throws IOException{
  Request request = new Request.Builder().url(url).build();

  Response response = client.newCall(request).execute();
  return response.body().string();
}
repose.body head ...

POST
public static final MediaType JSON = MediaType.parse("application/json;charset=utf-8");
String post(String url,String json) throws IOException{
  RequestBody body = RequestBody.create(JSON,json);
  Request request = new Request.Builder().url(url).post(body).build();

  Response response = client.newCall(request).execute();
  return response.body().string();
}
```