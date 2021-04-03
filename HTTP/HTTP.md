# HTTP

## 1. HTTP常见的状态码有哪些？

- 1xx表示服务器收到请求
- 2xx请求成功，如200
- 3xx重定向，如302
- 4xx,客户端错误，如404
- 5xx,服务端错误，如500

常见的`200`，表示成功；`301`永久重定向（配合`location`，浏览器自动处理）；`302`临时重定向（配合`location`，浏览器自动处理）；`304`资源未被修改；`404`资源未找到；`403`没有权限；`500`服务器错误；`504`网关超时。

## 2. 什么是Restful API

`Restful API`是一种早已推广使用的API设计方式；

- 传统API设计：把每个url当做一个功能

- `Restful API`设计：把每个url当做**唯一的资源**

传统API设计只使用`get`获取服务器的数据，`post`向服务器提交数据；

Restful API规范：

- 尽量不用`URL`参数
- 用`method`表示操作类型

使用`get`获取数据，`post`新建数据，`patch/put`更新数据，`delete`删除数据

例如：**传统API设计**是这样：

- post请求  /api/create-blog
- post请求 /api/update-blog?id=100
- get请求  /api/get-blog?id=100

**Restful API**设计是这样：

- post请求  /api/blog  （通过post method让服务器知道是新建博客）
- patch请求  /api/blog/100 （通过patch或者put让服务器知道是更新第100条博文）
- get请求  /api/blog/100 （通过get 让服务器知道是获取第100条博文）

本质上只是一种API的设计规范，不遵守规范也没问题，但是这种把API url当做唯一资源的观点更符合目前开发潮流。

## 3. http headers

可分为`request headers` 和 `response headers `两种请求头，都说几个就OK了

`request headers`有：

- `Accept` 浏览器可接收的数据格式
- `Accept-Encoding` 浏览器可接收的压缩算法，如gzip
- `Accept-Language` 浏览器可接收的语言，如zh-CN
- `Connection `  浏览器与服务器连接 ，设置为keep-alive表示 一次TCP连接重复使用
- `cookie`
- `Host`
- `User-Agent` 浏览器信息
- `Content-type` 发送数据的格式，如`application/json`

`response headers `有：

- `Content-type` 返回数据的格式，如`application/json`
- `Content-length` 返回数据的大小，多少字节
- `Content-Encoding` 返回数据的压缩算法，如gzip

缓存相关的headers

- `Cache-Control Expires`
- `Last-Modified/If-Modified-since`
- `Etag/If-None-Match`

## 4. HTTP缓存

### 4.1 什么是缓存？

缓存是一种保存资源并在下次请求时直接使用该资源的技术，当web 缓存发现请求的资源已经被存储，它会拦截请求，返回该资源的拷贝，而不会去源服务器重新下载。

### 4.2 为什么需要缓存？

**首先减少带宽**

使用缓存就不需要再去下载资源，这样使得网络带宽大幅减少。

**其次缓解服务器压力**

服务器不需要给每次请求都返回数据，这样可以降低服务器的压力。

**最重要的是提升网站的性能**

使用缓存可以减少用户等待的时间，这样可以使得网站的性能上升，给用户更好的体验。

### 4.3 HTTP缓存策略（强制缓存 + 协商缓存）

#### 4.3.1 强制缓存

`expires`可以用来设置缓存的时间，它的值是一个格林尼治时间，比如Mon, 20 Jul 2017 10:08:35 GMT 来告诉浏览器资源缓存过期时间，如果还没过该时间点则**不发请求**。

`expires`定义的缓存时间是相对服务器上的时间而言的，如果客户端上的时间跟服务器上的时间不一致，所以出现了`cache-control`，服务器返回该响应头字段，有许多取值：

```text
Cache-Control: max-age,no-cache, no-store, must-revalidate
```

#### 4.3.2 协商缓存

客户端发送请求之后，服务器端就必须要将全部的资源发回去。如果客户端发请求之后，服务器端资源并没有改变呢？这时就需要一个缓存校验字段了。

服务器将资源传递给客户端时，会将资源最后更改的时间以“Last-Modified: GMT”的形式加在实体首部上一起返回给客户端。例如

```text
Last-Modified: Fri, 23 Jul 2017 01:47:00 GMT
```

客户端在请求头发送`If-Modified-Since`，值就是之前返回的`Last-Modified`的值

该字段告诉服务器如果客户端传来的最后修改时间和服务器上的一致，直接返回 304 状态码，当前各大浏览器均是使用该字段来向服务器传递保存的 `Last-Modified` 的值。如果时间不一致的话，服务器会返回所有的这个资源并返回 200 状态码。



使用` Last-Modified `也存在一些问题，比如：

1、如果资源被重复生成，而且内容不变，则`Etag`更精确

2、` Last-Modified `只能精确到秒级

3、某些服务器不能精确的得到文件的最后修改时间。

为解决上面这些问题，又双叒叕有 `ETag `字段来标注**资源的唯一性**。

客户端在请求头发送`If-None-Match`，值就是之前返回的`ETag`的值

该字段告诉服务器如果客户端传来的最后修改时间和服务器上的一致，直接返回 304 状态码，当前各大浏览器均是使用该字段来向服务器传递保存的 `ETag`的值。如果时间不一致的话，服务器会返回所有的这个资源并返回 200 状态码。



