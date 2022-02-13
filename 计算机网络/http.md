### Request：请求行+请求头+请求体

1. Referer：表示这个请求从哪个url跳过来的，通过百度搜索淘宝，在进入淘宝网的请求报文中，Referer就是www.baidu.com，如果是直接访问就不会有这个头。常用语防盗链

2. Accept：告诉服务器，该请求所能支持的相应数据类型

3. if-Modified-Since：浏览器通知服务器本地缓存的最后变更时间，与另一个响应头组合控制浏览器页面的缓存，MIME类型

4. Cookie：客户端的Cookie就是通过这个发送给服务器，用于保存客户端的状态

5. User-Agent：浏览器通知服务器，客户端浏览器与操作系统相关信息

6. Connection：表示客户端与服务区连接的类型，keep-alive表示持久连接，close已关闭

7. Host：请求的服务器主机名

8. Content-Length：请求体的长度

9. Content-Type：请求的与实体对应的MIME信息。如果是post请求，会有这个头，默认值为**application/x-www-form-urlencoded**，表示请求体内容使用url编码

10. Accept-Encoding：浏览器通知服务器，浏览器支持的数据压缩格式

11. Accept-Language：浏览器通知服务器，浏览器支持的语言

12. Cach-Control：指定请求和响应遵循的缓存机制

### Response：响应行+响应头+响应体

响应行：报文协议及版本号：HTTP/1.1 200 OK，状态码以及状态描述

| 状态码                         |                                                              |
| ------------------------------ | ------------------------------------------------------------ |
| 1XX                            | 指示信息，表示请求已接收，继续处理                           |
| 2XX                            | 成功，表示请求已被成功接收，处理                             |
| 200 OK                         | 客户端请求成功                                               |
| 204 No Content                 | 无内容，服务器成功处理，但未返回内容。一般用在只是客户端向服务器发送信息，而服务器不向客户端返回信息的情况，不会刷新内容 |
| 206 Partial Content            | 服务器已经完成了部分GET请求。响应报文中包含Content-Range指定范围的实体内容 |
| 3XX                            | 重定向                                                       |
| 301 Moved Permanently          | 永久重定向，表示请求的资源已经永久的搬到了其他位置           |
| 302 Found                      | 临时重定向，表示请求的资源临时搬到了其他位置                 |
| 303 See Other                  | 临时重定向，应使用GET定向获取请求资源，303和302功能一样，区别指示303明确客户端应该使用GET访问 |
| 304 Not Modified               | 表示客户端发送附带条件的请求（GET方法中的if-Modified-Since），条件不满足时返回304，不包含任何响应主体 |
| 305 Use Proxy                  | 使用代理，所请求的资源必须通过代理访问                       |
| 4XX                            | 客户端错误                                                   |
| 400 Bad Request                | 客户端请求有语法错误，服务器无法理解                         |
| 401 Unauthorized               | 请求未授权，这个状态码必须和WWW-Authenticate报头域一起使用   |
| 402 Payment Required           | 保留，将来使用                                               |
| 403 Forbidden                  | 服务器收到请求，但是拒绝提供服务                             |
| 404 Not Found                  | 请求资源不存在，比如，输入了错误的url                        |
| 415 Unsupported media type     | 不支持的媒体类型                                             |
| 5XX                            | 服务端错误，服务器未能实现合法的请求                         |
| 500 Internal Server Error      | 服务器发生不可预期的错误                                     |
| 501 Not Implemented            | 服务器不支持请求的功能，无法完成请求                         |
| 502 Bad Gateway                | 走位网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应 |
| 503 Server Unavailable         | 由于超载或者系统维护，服务器暂时无法处理客户端的请求。       |
| 504 Gateway Time-out           | 充当网关或者代理的服务器，未及时从远端服务器获取请求         |
| 505 HTTP Version not supported | 服务器不支持请求的HTTP协议的版本，无法完成处理               |

响应头：

1.  Location：指定响应的路径，需要与状态码302配合使用，完成跳转
2. Content-Type：响应正文的类型（MIME类型）
3. Content-Disposition：通过浏览器以下载的方式解析正文
4. Set-Cookie：与会话相关技术。服务器向浏览器写入cookie
5. Content-Encoding：服务器使用的压缩格式，gzip
6. Content-length：相应正文的长度
7. Refresh：定时刷新，格式：秒数;url=路径。url可省略，默认值为当前页面
8. Server：指定是服务器的名称
9. Last-Modified：服务器通知浏览器，文件的最后修改事件，与if-Modified-Since一起使用
10. Cache-Control：响应输出到客户端后，服务端通过该报文头告诉客户端如何控制相应内容的缓存，

HTTP/1.1 持久连接在默认情况下是激活的，除非特别指明，否则 HTTP/1.1 假定所有的连接都是持久的，要在事务处理结束之后将连接关闭，HTTP/1.1 应用程序必须向报文中显示地添加 一个 Connection：close 首部。

HTTP1.1 客户端加载在收到响应后，除非响应中包含了 Connection：close 首部，不然 HTTP/1.1 连接就仍然维持在打开状态。但是，客户端和服务器仍然可以随时关闭空闲的连接。不发送 Connection：close 并不意味这服务器承诺永远将连接保持在打开状态。
