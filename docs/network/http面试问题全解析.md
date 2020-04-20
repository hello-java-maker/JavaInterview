HTTP协议定义了浏览器怎么向万维网服务器请求万维网文档，以及服务器怎么样把文档传送给浏览器。

举个例子来说，**用户单击鼠标后发生的事件**按顺序如下（以访问清华大学为例）：

1.  **浏览器分析链接指向页面的URL。**
2.  **浏览器向DNS请求解析www.tsinghua.edu.c的IP地址。**
3.  **DNS解析出该IP地址。**
4.  **浏览器与该服务器建立TCP链接**（默认port:80）。
5.  **浏览器发出HTTP请求：GET/chn/index.htm**（HTTP请求通过TCP套接字，发送请求报文，该请求报文作为TCP三次握手第三个报文数据发送给服务器）。
6.  **服务器通过HTTP响应把文件index.html发送给服务器。**
7.  **TCP连接释放**（释放连接若connection若为close，服务端主动关闭，客户端被动关闭，若为keepalive，则该连接会保持一段事件，该时间内可以继续接收请求）。
8.  **浏览器将文件进行解析，并将Web页面显示给用户**（先解析状态行，看状态码是否请求成功，然后解析每一个响应头，告知编码规范，对其进行格式化，有脚本还要加载脚本）。

### HTTP 状态码含义：

*   **200** OK 服务器已经成功处理了请求并提供了请求的网页。
*   **202** Accpted 已经接受请求，但处理尚未完成。
*   **204** No Content 没有新文档，浏览器应该继续显示原来的文档。
*   **206** Partial Content 客户端进行了范围请求。响应报文中由Content-Range指定实体内容的范围。实现断点续传
*   **301** Moved Permanently 永久性重定向。请求的网页已永久移动到新位置。
*   **302**（307） Moved Temporatily 临时性重定向。
*   **304** Not Moidfied 未修改。自上次请求后，请求的内容未修改过。
*   **401** Unauthorized 客户试图未经授权访问受密码保护的页面。该应答中会包含一个WWW-Authenticate头，浏览器由此显示login框，填写合适Authorization头以后再次发出请求。
*   **403** Forbidden 服务器拒绝请求。
*   **404** Not Found 服务器不存在客户机所请求的资源。
*   **500** Intern Server Error 服务器遇到一个错误，使其无法为请求提供服务。

### HTTP的请求方法

*   **GET** 向指定的资源发出显示请求，使用GET方法应该只用于在读取数据。
*   **HEAD** 与GET方法一样，都是向服务器发出指定资源的请求。只不过HEAD不包含有呈现数据，而仅仅是头部信息（关于该资源的信息）。
*   **PUT** 向指定资源位置上传最新位置（与POST相比，PUT指定了存放位置，而PSOT由服务器指定）。
*   **DELETE**请求服务器删除某一个资源
*   **POST** 向指定资源提交数据，请求服务器进行处理（例如提交表单或者上传文件）。
*   **TRACE** 请求服务器回送收到的请求信息，主要用于测试和诊断。
*   **OPTIONS** 使服务器传回该资源所支持的所有HTTP请求方法。若请求成功，则会在HTTP头中包含一个名为“Allow”的头，值是支持的方法，如“GET,POST”。

### GET和POST的区别：

*   **对资源的影响**：GET一般用于获取或者查询资源信息，意味着对同一个URL的多个请求返回的结果一样（幂等），没有修改资源的状态（安全）；而POST一般用于更新资源信息，POST既不是安全的也不是幂等的
*   **传递的信息量**：采用GET方法时，客户端把发送的数据添加到URL后面（即HTTP协议头中），使用“？”连接，各个变量用“&”连接，但是由于有些浏览器和服务器对URL的长度和字符格式存在限制，所以传递的信息有限；POST则把需要传递的数据放到请求报文的消息体中，HTTP协议对此没有限制，因此可以传递更多信息。
*   **安全性**：GET提交的数据，消息以明文出现在URL上，如密码等信息可能被浏览器缓存，从而从历史记录中得到；POST把消息存放在消息体中，安全性高，但是也存在被抓包软件抓取看到内容。

### cookie 和 session ：

　　HTTP协议是无状态：无状态是指协议对于事务处理没有记忆能力，简单来说，即使第一次和服务器连接后并且登陆成功后，第二次请求服务器依然不知道当前请求是哪个用户。为此，cookie和session的使用为此提供了解决方案。

**cookie:**

　　以文件的形式存在硬盘中的永久性cookie（设置了一定的时限）和停留在浏览器内存中的临时性cookie，当用户访问网站时，浏览器就会在本地寻找相关cookie。如果该cookie存在，浏览器就会将其与页面请求一起通过报头信息发送到站点。

**session:**

　　session与cookie的作用有点类似，不同的时cookie存储在本地，而session存储在服务器。当程序需要为某个客户端的请求创建一个session的时候，服务器首先会检查这个客户端的请求里是否包含了一个session标识session-id，如果已经包含一个session-id则为此客户端创建过session，服务器就按照session-id把这个session检索出来，如果不包含，就创建一个新的键值对<session,session-id>，并把session-id返回客户端保存。

　　浏览器提供了三种方式来保存ssesion-id:

*   cookie
*   url重写（把session-id附加在url后面，即使用GET）
*   增加隐藏域（使用POST）

　　session什么时候被创建？事实上session并不是在有客户端访问的时候被创建，而是知道某sever端程序调用类似function getSession()方法时才会创建。

### HTTP请求报文结构和响应报文结构

**HTTP请求报文结构如下：**

![](https://img2020.cnblogs.com/blog/704066/202004/704066-20200413194724299-117704646.png)

*   请求行：
*   请求头：为请求报文添加了一些附加信息，如 Host（接受请求的服务器地址 ip;port 或者 域名 等） / User-Agent / Connection（连接属性，如Keep-Alive） / Accept-Charset / Accpet-Encoding / Accept-Language
*   空行：请求头的最后一行会有一个空行，表示请求头部结束，接下俩的请求为正文。
*   请求正文：可选部分，如GET就没有请求正文

一个报文实体例子如下：

```java
GET /562f25980001b1b106000338.jpg HTTP/1.1
Host    img.mukewang.com
User-Agent    Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.106 Safari/537.36
Accept    image/webp,image/*,*/*;q=0.8
Referer    http://www.imooc.com/
Accept-Encoding    gzip, deflate, sdch
Accept-Language    zh-CN,zh;q=0.8
```

HTTP响应报文

![](https://img2020.cnblogs.com/blog/704066/202004/704066-20200413201951732-952331574.png)

*   状态行：协议版本，状态码等
*   响应头：与请求头类似，为响应报文添加了一些附加信息。
*   空行
*   响应正文

一个报文实体例子如下：

```java

HTTP/1.1 200 OK
Server: nginx
Date: Mon, 20 Feb 2017 09:13:59 GMT
Content-Type: text/plain;charset=UTF-8
Vary: Accept-Encoding
Cache-Control: no-store
Pragrma: no-cache
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Cache-Control: no-cache
Content-Encoding: gzip
Transfer-Encoding: chunked
Proxy-Connection: Keep-alive

{"code":200,"notice":0,"follow":0,"forward":0,"msg":0,"comment":0,"pushMsg":null,"friend":{"snsCount":0,"count":0,"celebrityCount":0},"lastPrivateMsg":null,"event":0,"newProgramCount":0,"createDJRadioCount":0,"newTheme":true}
```

### 参考文章：

http详解

[https://www.cnblogs.com/an-wen/p/11180076.html](https://www.cnblogs.com/an-wen/p/11180076.html)

cookie和sessions

[https://www.cnblogs.com/xxtalhr/p/9053906.html](https://www.cnblogs.com/xxtalhr/p/9053906.html)

关于HTTP协议

[https://www.cnblogs.com/ranyonsue/p/5984001.html](https://www.cnblogs.com/ranyonsue/p/5984001.html)


#### Tip：本来有很多我准备的资料的，但是都是外链，或者不合适的分享方式，所以大家去公众号回复【资料】好了。

![](http://image.ouyangsihai.cn/FszE5cIon6eHHexBEgOSBGBWeoyP)

现在免费送给大家，在我的公众号 **好好学java** 回复 **资料** 即可获取。

有收获？希望老铁们来个三连击，给更多的人看到这篇文章

1、老铁们，关注我的原创微信公众号「**好好学java**」，专注于Java、数据结构和算法、微服务、中间件等技术分享，保证你看完有所收获。

![](http://image.ouyangsihai.cn/FgUUPlQOlQtjbbdOs1RZK9gWxitV)

2、给俺一个 **star** 呗，可以让更多的人看到这篇文章，顺便激励下我继续写作，嘻嘻。