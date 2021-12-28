> Http的请求报文结构和响应报文结构

Http的请求报文主要由`请求行、请求头、空行、请求正文`。

**请求行**由请求方法、URL以及协议版本组成。
请求方法：GET、HEAD、PUT、POST、TRACE、OPTIONS、DELETE以及扩展方法
协议版本：常用的有HTTP/1.0和HTTP/1.1

**HTTP请求头**
| Header | 解释 | 示例 |
| --- | --- | --- |
| Accept | 指定客户端能够接收的内容类型 | Accept: text/plain, text/html |
| Accept-Charset | 浏览器可以接受的字符编码集。 | Accept-Charset: iso-8859-5 |
| Accept-Encoding | 指定浏览器可以支持的web服务器返回内容压缩编码类型。 | Accept-Encoding: compress, gzip |
| Accept-Language | 浏览器可接受的语言 | Accept-Language: en,zh |
| Accept-Ranges | 可以请求网页实体的一个或者多个子范围字段 | Accept-Ranges: bytes |
| Authorization | HTTP授权的授权证书 | Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ== |
| Cache-Control | 指定请求和响应遵循的缓存机制 | Cache-Control: no-cache |
| Connection | 表示是否需要持久连接。（HTTP 1.1默认进行持久连接） | Connection: close |
| Cookie | HTTP请求发送时，会把保存在该请求域名下的所有cookie值一起发送给web服务器。 | Cookie: $Version=1; Skin=new; |
| Content-Length | 请求的内容长度 | Content-Length: 348 |
| Content-Type | 请求的与实体对应的MIME信息 | Content-Type: application/x-www-form-urlencoded |
| Date | 请求发送的日期和时间 | Date: Tue, 15 Nov 2010 08:12:31 GMT |
| Expect | 请求的特定的服务器行为 | Expect: 100-continue |
| From | 发出请求的用户的Email | From: user@email.com |
| Host | 指定请求的服务器的域名和端口号 | Host: www.zcmhi.com |
| If-Match | 只有请求内容与实体相匹配才有效 | If-Match: “737060cd8c284d8af7ad3082f209582d” |
| If-Modified-Since | 如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回304代码 | If-Modified-Since: Sat, 29 Oct 2010 19:43:31 GMT |
| If-None-Match | 如果内容未改变返回304代码，参数为服务器先前发送的Etag，与服务器回应的Etag比较判断是否改变 | If-None-Match: “737060cd8c284d8af7ad3082f209582d” |
| If-Range | 如果实体未改变，服务器发送客户端丢失的部分，否则发送整个实体。参数也为Etag | If-Range: “737060cd8c284d8af7ad3082f209582d” |
| If-Unmodified-Since | 只在实体在指定时间之后未被修改才请求成功 | If-Unmodified-Since: Sat, 29 Oct 2010 19:43:31 GMT |
| Max-Forwards | 限制信息通过代理和网关传送的时间 | Max-Forwards: 10 |
| Pragma | 用来包含实现特定的指令 | Pragma: no-cache |
| Proxy-Authorization | 连接到代理的授权证书 | Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ== |
| Range | 只请求实体的一部分，指定范围 | Range: bytes=500-999 |
| Referer | 先前网页的地址，当前请求网页紧随其后,即来路 | Referer: http://www.zcmhi.com/archives/71.html |
| TE | 客户端愿意接受的传输编码，并通知服务器接受接受尾加头信息 | TE: trailers,deflate;q=0.5 |
| Upgrade | 向服务器指定某种传输协议以便服务器进行转换（如果支持） | Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11 |
| User-Agent | User-Agent的内容包含发出请求的用户信息 | User-Agent: Mozilla/5.0 (Linux; X11) |
| Via | 通知中间网关或代理服务器地址，通信协议 | Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1) |
| Warning | 关于消息实体的警告信息 | Warn: 199 Miscellaneous warning |

**请求正文**

请求正文不一定都有的，比如，get请求方法就没有请求正文部分。

![](http://image.ouyangsihai.cn/FhKqcF6tPKzSLW_lstiSDGoNgDqa)

例如，上图就是一个请求的实例，分别有：**请求行、请求头**，由于是一个Get请求，所以是没有请求体的，当然，比如post、put等请求，就会有请求体。

那么，http的响应报文结构，也是类似的。

主要由**状态行、响应头、空行、响应正文**组成。

![](http://image.ouyangsihai.cn/Fu2xla40yiujD2W7i-JaEhzbIIQT)

常见的响应头有：



响应头 | 说明 | 
---------|----------|
 Server | 服务器应用程序软件的名称和版本 | 
 Content-Type | 响应正文的类型（是图片或者二进制字符串等） | 
 Content-Length | 响应正文长度 | 
 Content-Charset | 响应正文使用的编码 | 
 Content-Encoding | 文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。Java的GZIPOutputStream可以很方便地进行gzip压缩，但只有Unix上的Netscape和Windows上的IE 4、IE 5才支持它。因此，Servlet应该通过查看Accept-Encoding头（即request.getHeader("Accept-Encoding")）检查浏览器是否支持gzip，为支持gzip的浏览器返回经gzip压缩的HTML页面，为其他浏览器返回普通页面。 | 
 Content-Language | 响应正文使用的语言 | 
 Expires 	|应该在什么时候认为文档已经过期，从而不再缓存它？
 Last-Modified 	| 文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置。

 > http状态码

**HTTP状态码分类**
HTTP状态码共分为5种类型：
| 分类 | 分类描述 |
|-----|-----|
| 1** | 信息，服务器收到请求，需要请求者继续执行操作 |
| 2** | 成功，操作被成功接收并处理 |
| 3** | 重定向，需要进一步的操作以完成请求 |
| 4** | 客户端错误，请求包含语法错误或无法完成请求 |
| 5** | 服务器错误，服务器在处理请求的过程中发生了错误 |

HTTP的状态码列表：

| 状态码 | 状态码英文名称 | 中文描述 |
|-----|-----|-----|
| 100 | Continue | 继续。客户端应继续其请求 |
| 101 | Switching Protocols | 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议 |
|   |
| 200 | OK | 请求成功。一般用于GET与POST请求 |
| 201 | Created | 已创建。成功请求并创建了新的资源 |
| 202 | Accepted | 已接受。已经接受请求，但未处理完成 |
| 203 | Non-Authoritative Information | 非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本 |
| 204 | No Content | 无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档 |
| 205 | Reset Content | 重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域 |
| 206 | Partial Content | 部分内容。服务器成功处理了部分GET请求 |
|   |
| 300 | Multiple Choices | 多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择 |
| 301 | Moved Permanently | 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替 |
| 302 | Found | 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI |
| 303 | See Other | 查看其它地址。与301类似。使用GET和POST请求查看 |
| 304 | Not Modified | 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源 |
| 305 | Use Proxy | 使用代理。所请求的资源必须通过代理访问 |
| 306 | Unused | 已经被废弃的HTTP状态码 |
| 307 | Temporary Redirect | 临时重定向。与302类似。使用GET请求重定向 |
|   |
| 400 | Bad Request | 客户端请求的语法错误，服务器无法理解 |
| 401 | Unauthorized | 请求要求用户的身份认证 |
| 402 | Payment Required | 保留，将来使用 |
| 403 | Forbidden | 服务器理解请求客户端的请求，但是拒绝执行此请求 |
| 404 | Not Found | 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 |
| 405 | Method Not Allowed | 客户端请求中的方法被禁止 |
| 406 | Not Acceptable | 服务器无法根据客户端请求的内容特性完成请求 |
| 407 | Proxy Authentication Required | 请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权 |
| 408 | Request Time-out | 服务器等待客户端发送的请求时间过长，超时 |
| 409 | Conflict | 服务器完成客户端的 PUT 请求时可能返回此代码，服务器处理请求时发生了冲突 |
| 410 | Gone | 客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置 |
| 411 | Length Required | 服务器无法处理客户端发送的不带Content-Length的请求信息 |
| 412 | Precondition Failed | 客户端请求信息的先决条件错误 |
| 413 | Request Entity Too Large | 由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息 |
| 414 | Request-URI Too Large | 请求的URI过长（URI通常为网址），服务器无法处理 |
| 415 | Unsupported Media Type | 服务器无法处理请求附带的媒体格式 |
| 416 | Requested range not satisfiable | 客户端请求的范围无效 |
| 417 | Expectation Failed | 服务器无法满足Expect的请求头信息 |
|   |
| 500 | Internal Server Error | 服务器内部错误，无法完成请求 |
| 501 | Not Implemented | 服务器不支持请求的功能，无法完成请求 |
| 502 | Bad Gateway | 作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应 |
| 503 | Service Unavailable | 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中 |
| 504 | Gateway Time-out | 充当网关或代理的服务器，未及时从远端服务器获取请求 |
| 505 | HTTP Version not supported | 服务器不支持请求的HTTP协议的版本，无法完成处理 |

> http的缓存机制了解吗？有关缓存的首部字段有哪些呢？

**Last-Modified和If-Modifed-since**

Last-Modified：是由服务器发往客户端的最后的时间
If-Modified-since：是由客户端发往服务器的最后时间

用于记录页面最后修改的时间的Http头信息：服务器将请求的Last-Modified发往客户端，客户端如果没有缓存，则缓存在浏览器，然后，客户端每次将If-Modified-Since发往服务端，每次如果服务端发现服务端资源没有发生改变，则自动返回304，内容为空，这样节省了传输的数据量，如果服务端发现有新的资源，则返回200，并且返回新的资源，然后浏览器丢弃旧的资源，缓存新的资源。

**ETag和if-None-Match**

ETag是服务器会为每个资源分配对应的ETag值，当资源内容发生改变时，其值也会发生改变。

ETag和if-None-Match的工作原理是在Http的Response中添加ETags，当客户端再次请求该资源时，将在Http的Request中加入if-None-Match信息（ETag值）。如果服务器资源验证的ETags没有改变，将返回一个304状态；否则，服务器返回一个200状态，并返回该资源和新的ETags。

**Expires/Cache-Control(优先使用)**

Expires用于控制缓存的失效时间，但是有一个缺点就是失效时间是服务器返回的时间，跟客户端会存在延时，所以，在http1.1中，加入了Cache-Control，这声明是一种相对秒级，可以避免服务端和客户端时间不一致的问题。

Expires的格式：`expires=Fri, 17 Jul 2020 21:15:42 GMT`。

Cache-Control的格式：`'Cache-Control': 'max-age=2000000, no-store'`


> Last-Modified和Etag 如何帮助提高性能？

一起使用，利用客户端缓存，服务端判断页面是否被修改。

> 有了Last-Modified为什么还要用ETag字段呢？

- 文件修改非常频繁时，If-Modifed-since能检查到的粒度是秒级的，这种修改无法使用If-Modifed-since。
- 某些文件周期性的修改，但是内容没有改变，这种无法体现，导致重新get。
- 某些服务器不能精准的得到文件的最后修改时间，利用ETag提供了更加严格的验证。


> http1.1和http1.0的区别?

- 长连接与短连接的区别：http1.1使用长连接，在请求消息中会包含Connection：Keep-Alive的头域。
- 分块传输：
在http1.0中，用于指定实体长度的唯一机制是通过content-length，静态资源没有问题，但对于动态资源，为了获取长度，只有等他完全生成以后才能获取content-length的值，这要求缓存整个响应，延长了响应用户的时间。
在http1.1中，引入了分块的传输方法，该方法使得发送方可以将消息实体分割为任意大小的组块，在每个组块的前面都加上长度，最后的一块为空块，标识完成传输，这样避免了服务器端大量的占用缓存。
- http状态码100 Continue：
当客户端发送的post请求的数据大于1024字节是，客户端并不是直接发起POST请求，而是分为两步，1）发送一个请求，包含Expect：100-continue，询问服务端是否接受数据；2）接受到服务端返回的100 continue后，才将数据发送给服务端。
这是为了避免发送一个冗长的请求，占用服务端的资源。
- Host域：
http1.1中，随着虚拟主机技术的发展，物理服务器上可以存在多个虚拟主机，所以，在1.1中可以在host中存在多个ip地址。

> http常用方法有哪些？

|方法 |   说明 |   支持的HTTP协议版本|
|-----|------|------|
|GET  |  获取资源|    1.0、1.1|
|POST   | 传输实体主体 |   1.0、1.1|
|PUT |   传输文件  |  1.0、1.1|
|HEAD  |  获得报文首部 |   1.0、1.1|
|DELETE   | 删除文件  |  1.0、1.1|
|OPTIONS |   询问支持的方法  |  1.1|
|TRACE  |  追踪路径  |  1.1|
|CONNECT   | 要求用隧道协议连接代理 |   1.1|

> http哪些是幂等的呢？

幂等性是指一次或者多次操作所产生的副作用是一样的。

GET、Put、Delete是幂等的，Post不是幂等的。

> http的请求方法get和post的区别？

- 从幂等性和安全性来说：Get是指查询或者获取资源，它是幂等的，同时也是安全的，而post一般是更新资源，既不是幂等的也不是安全的。
- 从传输方式和传输数量大小限制方面：get请求放在请求行后面，有传输大小限制；post请求放在消息体中，没有传输大小限制。
- 从是否缓存的角度：get请求的数据会被浏览器缓存起来，post则不会。

> 为什么http是无状态的，如何保持状态?

无状态性是指：对事务处理没有记忆的能力，每一次的请求信息返回之后，就会丢弃，服务器无法知道与上次请求的联系，
好处：不用分配内存记录大量状态，节省服务端资源；
缺点：每次需要重新传输大量的数据。

保持状态采用的是会话跟踪技术（解决无状态性），主要的方法有：Cookie、Url重写、session和利用html嵌入表单域。

> http短连接和长连接原理

http1.0默认使用短连接，http1.1默认使用长连接，长连接是request中添加Connection：keep-alive，这样就是长连接。

另外，http的短连接和长连接，实质上就是TCP协议的长连接和短连接。

http长连接的优点：
- 通过开启或者关闭更少的连接，节约cpu时间和内存
- 通过减少TCP开启和关闭引起的包的数量，降低网络阻塞

http长连接的缺点：服务器维护一个长连接会增加开销，消耗服务器资源。

> 说说http的特点

- 支持客户端、服务端通信模式
- 简单方便快捷：发送请求，只需要简单的输入请求路径和请求方法即可，然后通过浏览器发送就可以了。
- 灵活：http协议允许客户端和服务端传输任意类型任意合适的数据对象，这些不同类型由Content-Type标记。
- 无连接：每次客户端发送请求到服务端，服务端响应之后，就会断开连接。
- 无状态：http是无状态的协议，请求的处理没有记忆能力。

> http存在哪些安全性问题？怎么解决

- 通过使用明文不加密，内容可能被窃听
- 不验证通信方的身份，可能遭到伪装
- 无法验证报文的完整性，可能被篡改

可以采用https进行解决。

> https的作用，https中的安全性技术有哪些

https可以对内容进行加密，进行身份验证，同时可以检验数据完整性。

https的安全性技术：
- 对称性加密算法：用于真正传输的数据进行加密。
- 非对称性加密算法：用于在握手过程中加密生成的密码，非对称性加密算法会生成公钥和私钥。
- 散列算法：用于验证数据的完整性。
- 数字证书：使用数字证书进行证明自己的身份。

> https和http的区别？

- https更加安全：https可以通过加密、数据完整性验证和身份验证的方法保证数据的安全。
- https需要申请证书，在CA申请证书。
- 端口不同：http采用80，https采用443。
- http协议运行在tcp之上，https运行在ssl、TLS之上的http协议，ssl、TLS运行在TCP之上。

> http和socket的区别?

- http只能走tcp协议，socket可以走tcp和udp协议。
- http基于请求响应模式，只有客户端发送给了服务端，服务端才可以响应，socket则可以通过服务端推送给客户端。
- socket效率高，因为不需要解析报文头部字段等。

