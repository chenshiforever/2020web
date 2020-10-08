# 2020-前端面试题大全
总结今年校招以来的所有前端面试题
```
## 1.HTTPS流程

* 首先客户端通过URL访问服务器建立SSL连接

* 服务端收到客户端请求后，会将网站支持的证书信息（证书中包含公钥）传送一份给客户端。  
* 客户端的服务器开始协商SSL连接的安全等级，也就是信息加密的等级。
* 客户端的浏览器根据双方同意的安全等级，建立会话密钥，然后利用网站的公钥将会话密钥加密，并传送给网站。
* 服务器利用自己的私钥解密出会话密钥。
* 服务器利用会话密钥加密与客户端之间的通信。

### HTTPS详细工作原理:
* 1.浏览器将自己支持的一套加密规则发送给网站，如RSA加密算法，DES对称加密算法，SHA1摘要算法
* 2.网站从中选出一组加密算法与HASH算法，并将自己的身份信息以证书的形式发回给浏览器。证书里面包含了网站地址，加密公钥，以及证书的颁发机构等信息（证书中的私钥只能用于服务器端进行解密，在握手的整个过程中，都用到了证书中的公钥和浏览器发送给服务器的随机密码以及对称加密算法）
* 3.获得网站证书之后浏览器要做以下工作：
  - a) 验证证书的合法性（颁发证书的机构是否合法，证书中包含的网站地址是否与正在访问的地址一致等），如果证书受信任，则浏览器栏里面会显示一个小锁头，否则会给出证书不受信的提示。
  - b) 如果证书受信任，或者是用户接受了不受信的证书，浏览器会生成一串随机数的密码（这其实就是用于之后数据通信的对称加密算法的秘钥），并用证书中提供的公钥加密（对秘钥的加密采用非对称的方法）。
  - c) 使用约定好的HASH算法计算握手消息，并使用生成的随机数（对称算法的秘钥）对消息进行加密，最后将之前生成的被公钥加密的随机数密码，HASH摘要值（已经被对称算法加密）一起发送给服务器
hash(握手),公钥(random)，random(hash(握手))
* 4.网站接收浏览器发来的数据之后要做以下的操作：
  - a) 使用自己的私钥将信息解密并取出浏览器发送给服务器的随机密码（得到了对称加密算法的秘钥），使用密码解密浏览器发来的握手消息（用对称算法的秘钥解HASH摘要值），并验证HASH是否与浏览器发来的一致。
  - b) 使用随机密码加密一段握手消息，发送给浏览器。
random(握手)，hash(握手)
* 5.浏览器解密并计算握手消息的HASH，如果与服务端发来的HASH一致，此时握手过程结束，之后所有的通信数据将由之前浏览器生成的随机密码并利用对称加密算法进行加密。

从上面的4个大的步骤可以看到，握手的整个过程使用到了数字证书、对称加密、HASH摘要，非对称加密算法。

## 2.http与https的区别

* http是超文本传输协议，构建与TCP/IP协议之上，默认端口号为80，处于网络体系结构的最顶层应用层上，Http协议采用的是请求/响应的工作方式。Http是无连接无状态的。
* HTTPS是HTTP协议的安全版本，HTTP协议的数据传输是明文的，是不安全的，HTTPS使用了SSL/TLS协议进行了加密处理和身份认证
* http和https使用连接方式不同，默认端口也不一样，http是80，https是443。
* https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。
* http的连接很简单，是无状态的；
* HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。
* HTTPS可以有效的防止运营商劫持，解决了防劫持的一个大问题。

## 3.https流程

客户端在使用HTTPS方式与Web服务器通信时有以下几个步骤，如图所示。
* 　　（1）客户使用https的URL访问Web服务器，要求与Web服务器建立SSL连接。
* 　　（2）Web服务器收到客户端请求后，会将网站的证书信息（证书中包含公钥）传送一份给客户端。
* 　　（3）客户端的浏览器与Web服务器开始协商SSL连接的安全等级，也就是信息加密的等级。
* 　　（4）客户端的浏览器根据双方同意的安全等级，建立会话密钥，然后利用网站的公钥将会话密钥加密，并传送给网站。
* 　　（5）Web服务器利用自己的私钥解密出会话密钥。
* 　　（6）Web服务器利用会话密钥加密与客户端之间的通信。

## 4.HTTPS的优点

* 　　（1）使用HTTPS协议可认证用户和服务器，确保数据发送到正确的客户机和服务器；
* 　　（2）HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全，可防止数据在传输过程中不被窃取、改变，确保数据的完整性。
* 　　（3）HTTPS是现行架构下最安全的解决方案，虽然不是绝对安全，但它大幅增加了中间人攻击的成本。

## 5.HTTPS的缺点

* 　　（1）HTTPS协议握手阶段比较费时，会使页面的加载时间延长近50%，增加10%到20%的耗电；
* 　　（2）HTTPS连接缓存不如HTTP高效，会增加数据开销和功耗，甚至已有的安全措施也会因此而受到影响；
* 　　（3）SSL证书需要钱，功能越强大的证书费用越高，个人网站、小网站没有必要一般不会用。
* 　   （4）SSL证书通常需要绑定IP，不能在同一IP上绑定多个域名，IPv4资源不可能支撑这个消耗。
* 　　（5）HTTPS协议的加密范围也比较有限，在黑客攻击、拒绝服务攻击、服务器劫持等方面几乎起不到什么作用。最关键的，SSL证书的信用链体系并不安全，特别是在某些国家可以控制CA根证书的情况下，中间人攻击一样可行。

## 6.cookie 和session 的区别：

* cookie数据存放在客户的浏览器上，session数据放在服务器上。
* cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗，考虑到安全应当使用session。
* session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能，考虑到减轻服务器性能方面，应当使用COOKIE。
* 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

## 7.http方法及功能（get post put..）

* GET 对这个资源的查操作
* POST  向指定资源提交数据进行处理请求。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。（适用于更新操作）
* PUT 从客户端向服务器传送的数据取代指定的文档的内容。（适用于添加操作）
* DELETE  对这个资源的删操作（注意：客户端无法保证删除操作一定会被执行，因为HTTP规范允许服务器在不通知客
* 户端的情况下撤销请求）
* HEAD 与GET方法的行为很类似，但服务器在响应中只返回实体的头部分。可以快速获取资源信息，比如资源类型；通过查看响应中的状态码，可以确定资源是否存在；通过查看首部，测试资源是否被修改；
* OPTIONS 用于获取当前URL所支持的方法。若请求成功，则它会在HTTP头中包含一个名为“Allow”的头，值是所支持的方法，如“GET, POST”
* TRACE 会在目的服务器端发起一个“回环”诊断，因为客户端在发起一个请求时，这个请求可能要穿过防火墙、代理、网关、或者其它的一些应用程序。这中间的每个节点都可能会修改原始的HTTP请求，TRACE方法允许客户端在最终将请求发送服务器时，它变成了什么样子。由于有一个“回环”诊断，在请求最终到达服务器时，服务器会弹回一条TRACE响应，并在响应主体中携带它收到的原始请求报文的最终模样。这样客户端就可以查看HTTP请求报文在发送的途中，是否被修改过了
* 

## 8.对 https 的了解

##### **首先讲HTTP的缺点：**
* 通信使用明文加密，容易遭到窃听
* 不验证对方身份，容易遭到伪装
* 无法确保报文完整性，容易遭到篡改
##### **https的出现就是为了解决这三个问题：**
* https = 加密 + 认证 + 完整性保护
* 采用非对称加密的方式实现秘钥的交换，后续混合加密，实现了机密性
* 采用摘要算法（MD5/SHA-2）确保报文完整性
* 采用数字签名+第三方证书机构实现身份认证

## 9.讲解http1.1 、http2.0、https
**管道机制**
因为http1.1支持长连接，发送一个请求之后还可以发送第二个请求，如果第二个请求要等待第一个请求返回之后才发送，效率就很低，所以引入了管道机制，多个请求可以按顺序同时发送，响应按顺序返回
**Content-Length**
因为一个tcp可以同时发送多个请求，我们需要一个字段来区分多个请求，Content-length表示当前请求响应共有多少字段，超过这个值的响应就不是该请求的响应
**分块传输**
因为http1.1是按顺序发送请求和响应，如果一个顺序靠前的请求响应时间较长，会阻碍后面请求的响应，造成很大的延迟，这就是线头阻塞。故http1.1支持分块传输：Transfer-Encoding: chunked，每个响应头前都有一个16进制数字表示该块儿大小，如果为0表示本次分块
http1.1存在问题
主要还是线头阻塞问题，为了避免这个问题，只有两种方法：一是减少请求数，二是同时多开持久连接。这导致了很多的网页优化技巧，比如合并脚本和样式表、将图片嵌入CSS代码、域名分片（domain sharding）等等。如果HTTP协议设计得更好一些，这些额外的工作是可以避免的。
##### http1.1和http1.0对比

* 缓存处理：http1.0中只有if-Modified-Since、Expires，http1.1中增加了E-tag、if-None-match、等
* 优化带宽和网络请求：如果我们的请求的只是一个资源的一部分，http1.0会全部返回整个资源，造成带宽的浪费，http1.1中增加了range字段，允许请求资源的某部分
* 错误通知机制：新增了24个错误状态码
* Host名增加：http1.0认为域名和主机是一一对应的，http1.1增加Host字段，支持多个域名对应一个机器，为虚拟主机的发展奠定了基础
* 长连接：支持长连接和管道机制，一定程度上弥补了http1.0每次请求都要重建tcp连接的缺陷

##### http2.0/SPDY
http2.0是基于SPDY的，两者没有太大的区别，主要是header压缩方式以及http2.0支持明文传输。对比http1.1，主要区别有以下几个
**多路复用**
http2.0一个tcp连接支持多个stream流同时传输
**请求优先级**
多路复用可能会带来一个新的问题，多个请求并发可能导致关键请求被阻塞，故http2.0支持request设置优先级
**header压缩**
header中的一些信息是重复的，采用合适的压缩算法减少请求大小
请求优先级（request prioritization）。多路复用带来一个新的问题是，在连接共享的基础之上有可能会导致关键请求被阻塞。SPDY允许给每个request设置优先级，这样重要的请求就会优先得到响应。比如浏览器加载首页，首页的html内容应该优先展示，之后才是各种静态资源文件，脚本文件等加载，这样可以保证用户能第一时间看到网页内容。
头部压缩。前面提到HTTP1.x的header很多时候都是重复多余的。选择合适的压缩算法可以减小包的大小和数量。
基于HTTPS的加密协议传输，大大提高了传输数据的可靠性。
**服务端推送（server push）**
例如我的网页有一个sytle.css的请求，在客户端收到sytle.css数据的同时，服务端会将sytle.js的文件推送给客户端，当客户端再次尝试获取sytle.js时就可以直接从缓存中获取到，不用再发请求了。
**HTTP2.0**

* HTTP2.0 支持明文 HTTP 传输，而 SPDY 强制使用 HTTPS
* HTTP2.0 消息头的压缩算法采用 HPACK，而非 SPDY 采用的 DEFLATE
*  新的二进制格式（Binary Format），HTTP1.x的解析是基于文本。
*   多路复用（MultiPlexing），即连接共享，即每一个request都是是用作连接共享机制的。一个request对应一个id，这样一个连接上可以有多个request，每个连接的request可以随机的混杂在一起，接收方可以根据request的 id将request再归属到各自不同的服务端请求里面。多路复用原理图：
* header压缩，如上文中所言，对前面提到过HTTP1.x的header带有大量信息，而且每次都要重复发送，HTTP2.0使用encoder来减少需要传输的header大小，通讯双方各自cache一份header fields表，既避免了重复header的传输，又减小了需要传输的大小。

##### 性能优化

1. 减少 HTTP 请求
2. 静态资源使用 CDN
3. 善用缓存
4. 压缩文件
5. 域名拆分

#### HTTP/2 连接建立过程
现在的主流浏览器 HTTP/2 的实现都是基于 SSL/TLS 的，也就是说使用 HTTP/2 的网站都是 HTTPS 协议的，所以本文只讨论基于 SSL/TLS 的 HTTP/2 连接建立过程。
基于 SSL/TLS 的 HTTP/2 连接建立过程和 HTTPS 差不多。在 SSL/TLS 握手协商过程中，客户端在 ClientHello 消息中设置 ALPN（应用层协议协商）扩展来表明期望使用 HTTP/2 协议，服务器用同样的方式回复。通过这种方式，HTTP/2 在 SSL/TLS 握手协商过程中就建立起来了。
##### 性能优化
使用 HTTP/2 代替 HTTP/1.1，本身就是一种巨大的性能提升。
这小节要聊的是在 HTTP/1.1 中的某些优化手段，在 HTTP/2 中是不必要的，可以取消的。
**取消合并资源**
在 HTTP/1.1 中要把多个小资源合并成一个大资源，从而减少请求。而在 HTTP/2 就不需要了，因为 HTTP/2 所有的请求都可以在一个 TCP 连接发送。合并文件、内联资源、雪碧图、域名分片对于 HTTP/2 来说是不必要的，使用 h2 尽可能将资源细粒化，文件分解地尽可能散，不用担心请求数多
**取消域名拆分**
取消域名拆分的理由同上，再多的 HTTP 请求都可以在一个 TCP 连接上发送，所以不需要采取多个域名来突破浏览器 TCP 连接数限制这一规则了。


## 10.HTTP劫持的实现原理
　　**一般来说HTTP劫持主要通过下面几个步骤来做：**

*   标识HTTP连接。在天上飞的很多连接中，有许多种协议，第一步做的就是在TCP连接中，找出应用层采用了HTTP协议的连接，进行标识；
*   篡改HTTP响应体，可以通过网关来获取数据包进行内容的篡改；
*   抢先回包，将篡改后的数据包抢先正常站点返回的数据包先到达用户侧，这样后面正常的数据包在到达之后会被直接丢弃。

#### 与DNS 劫持的区别举例
**DNS劫持的现象：** 你输入的网址是http://www.google.com，出来的是百度的页面；HTTP劫持的现象：你打开的是知乎的页面，右下角弹出唐老师的不孕不育广告（2018年更：右下角弹出：偶系渣渣辉）。
DNS劫持就是你想去存钱运营商却把你拉到了劫匪手中；而HTTP劫持就是你从服务器买了一包零食，横竖都很恶心人。
DNS劫持是你想去医院的时候，把你给丢到火车站；HTTP劫持是你去医院的时候，有人半途上车给你塞小广告。
**DNS劫持：** 在DNS服务器中，将www.xxx.com的域名对应的IP地址进行了变化。你解析出来的域名对应的IP，在劫持前后不一样；
**HTTP劫持：** 你DNS解析的域名的IP地址不变。在和网站交互过程中的劫持了你的请求。在网站发给你信息前就给你返回了请求。

#### HTTP劫持解决方法
　　对付HTTP劫持，最好的方法之一，就是使用HTTPS来连接网页。而使用HTTPS，在传输数据过程中，数据是加密的。就如同原先开车被人在车窗塞小广告，现在把窗都关紧，他人自然再也无法插足。
　　HTTPS不仅可以防止HTTP劫持，也能够较好地防止DNS劫持，这是由于HTTPS的安全是由SSL来保证的，需要正确的证书，连接才会成立。如果DNS把域名解析到了不对应的IP，是无法通过证书认证的，连接会被终止。实际上，现在已经有越来越多的网站支持HTTPS，但为了兼容等问题，不少网站也同时提供HTTP连接，例如著名的视频网站哔哩哔哩。主动使用HTTPS来进行连接，不但有效防止网页劫持，还能够保护隐私。
## 11.H5新特性和ES6新特性

* 语义化的区块和段落元素：<section>,<article>,<nav>,<header>,<footer>,<aside>和<hgroup>
* 音频和视频：<audio>和<video>元素嵌入和允许操作新的多媒体内容
* 阴影:使用box-shadow给逻辑框设置一个阴影，text-shadow文字加阴影
* 圆角：使用border-image和它关联的普通属性，而且可以通过border-radius属性来支持圆角边框
* 动画：为你的样式设置动画使用CSS Transitions以在不同的状态间设置动画，或者使用CSS Animations在页面的某些部分设置动画而不需要一个触发事件，你现在可以在页面中控制移动元素了
* flex布局：css多栏布局
* grid布局：网格布局
* 线性渐变：使用gradient设置线性渐变
* 媒体查询：根据显示设备的特性设置css
* 图片边框：使用border-image设置图片边框
* let和const关键字：let关键字定义块作用域变量，const定义常量
* 字符串模版：`${}`
* 箭头函数：左边是参数集合，右边是函数体
* 原生promise对象：将promise对象纳入规范
* symbol：增加symbol数据类型
* ES module: 引用ES module 模块化规范
* 变量结构赋值
* async函数
* set和map函数
* for..of循环：用来遍历实现迭代器接口的数据
* class

## 12.手写继承
```js
//原型链继承
核心： 将父类的实例作为子类的原型
function Cat(){
}
Cat.prototype = new Animal();
Cat.prototype.name = 'cat';

//构造继承
核心：使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
//实例继承
核心：为父类实例添加新特性，作为子类实例返回
function Cat(name){
  var instance = new Animal();
  instance.name = name || 'Tom';
  return instance;
}
//拷贝继承
function Cat(name){
  var animal = new Animal();
  for(var p in animal){
    Cat.prototype[p] = animal[p];
  }
  Cat.prototype.name = name || 'Tom';
}
//组合继承
核心：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal();
//寄生组合继承
核心：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点
function Animal(){
    this.eat = 'meat'
}
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();
//构造继承
function Cat(name){
  var instance = new Animal();
  instance.name = name || 'Tom';
  return instance;
}
//原型式继承

function createObj (o) {
  function F () { }
  F.prototype = o;
  return new F()
}
```
## 12.http状态码

* 1xx：信息响应类，表示接收到请求并且继续处理
* 2xx：处理成功响应类，表示动作被成功接收、理解和接受
* 3xx：重定向响应类，为了完成指定的动作，必须接受进一步处理
* 4xx：客户端错误，客户请求包含语法错误或者是不能正确执行
* 5xx：服务端错误，服务器不能正确执行一个正确的请求
* 100 Continue  继续。客户端应继续其请求
* 101 Switching Protocols 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议
* 200 OK  请求成功。一般用于GET与POST请求
* 201 Created 已创建。成功请求并创建了新的资源
* 202 Accepted  已接受。已经接受请求，但未处理完成
* 203 Non-Authoritative Information 非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本
* 204 No Content  无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档
* 205 Reset Content 重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域
* 206 Partial Content 部分内容。服务器成功处理了部分GET请求
* 300 Multiple Choices  多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择
* 301 Moved Permanently 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替
* 302 Found 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI
* 303 See Other 查看其它地址。与301类似。使用GET和POST请求查看
* 304 Not Modified  未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源
* 305 Use Proxy 使用代理。所请求的资源必须通过代理访问
* 306 Unused  已经被废弃的HTTP状态码
* 307 Temporary Redirect  临时重定向。与302类似。使用GET请求重定向
* 400 Bad Request 客户端请求的语法错误，服务器无法理解
* 401 Unauthorized  请求要求用户的身份认证
* 402 Payment Required  保留，将来使用
* 403 Forbidden 服务器理解请求客户端的请求，但是拒绝执行此请求
* 404 Not Found 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面
* 405 Method Not Allowed  客户端请求中的方法被禁止
* 406 Not Acceptable  服务器无法根据客户端请求的内容特性完成请求
* 407 Proxy Authentication Required 请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权
* 408 Request Time-out  服务器等待客户端发送的请求时间过长，超时
* 409 Conflict  服务器完成客户端的 PUT 请求时可能返回此代码，服务器处理请求时发生了冲突
* 410 Gone  客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置
* 411 Length Required 服务器无法处理客户端发送的不带Content-Length的请求信息
* 412 Precondition Failed 客户端请求信息的先决条件错误
* 413 Request Entity Too Large  由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息
* 414 Request-URI Too Large 请求的URI过长（URI通常为网址），服务器无法处理
* 415 Unsupported Media Type  服务器无法处理请求附带的媒体格式
* 416 Requested range not satisfiable 客户端请求的范围无效
* 417 Expectation Failed  服务器无法满足Expect的请求头信息
* 500 Internal Server Error 服务器内部错误，无法完成请求
* 501 Not Implemented 服务器不支持请求的功能，无法完成请求
* 502 Bad Gateway 作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应
* 503 Service Unavailable 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中
* 504 Gateway Time-out  充当网关或代理的服务器，未及时从远端服务器获取请求
* 505 HTTP Version not supported  服务器不支持请求的HTTP协议的版本，无法完成处理

## 13.ajax流程讲一下

* 1--启动  获取XMlHttpRequest对象       1. 创建 XMLHttpRequest 对象,也就是创建一个异步调用对象
* 2--open 打开url通道，并设置异步传输    2. 创建一个新的 HTTP 请求,并指定该 HTTP 请求的方法、URL 及验证信息
* 3--send 发送数据到服务器              3. 设置响应 HTTP 请求状态变化的函数
* 4--服务器接受数据并处理，处理完成后返回结果         4. 发送 HTTP 请求
* 5--客户端接收服务器端返回              5. 获取异步调用返回的数据
* 使用 JavaScript 和 DOM 实现局部刷新

readyState是XMLHttpRequest对象的一个属性，用来标识当前XMLHttpRequest对象处于什么状态。
readyState总共有5个状态值，分别为0~4，每个值代表了不同的含义

* 0：初始化，XMLHttpRequest对象还没有完成初始化
* 1：载入，XMLHttpRequest对象开始发送请求
* 2：载入完成，XMLHttpRequest对象的请求发送完成
* 3：解析，XMLHttpRequest对象开始读取服务器的响应
* 4：完成，XMLHttpRequest对象读取服务器响应结束
```js
var Ajax={
  get: function(url, fn) {
    // XMLHttpRequest对象用于在后台与服务器交换数据
    var xhr = new XMLHttpRequest();
    xhr.open('GET', url, true);
    xhr.onreadystatechange = function() {
      // readyState == 4说明请求已完成 readyState 0=>初始化 1=>载入 2=>载入完成 3=>解析 4=>完成
      //
      if (xhr.readyState == 4 && xhr.status == 200 || xhr.status == 304) {
        // 从服务器获得数据
        fn.call(this, xhr.responseText);
      }
    };
    xhr.send();
  },
  // datat应为'a=a1&b=b1'这种字符串格式，在jq里如果data为对象会自动将对象转成这种字符串格式
  post: function (url, data, fn) {
    var xhr = new XMLHttpRequest();
    xhr.open("POST", url, true);
    // 添加http头，发送信息至服务器时内容编码类型
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4 && (xhr.status == 200 || xhr.status == 304)) {
        fn.call(this, xhr.responseText);
      }
    };
    xhr.send(data);
  }
}
```
## 14.了解promise吗，简单说一下，手写promise
Promise 是一个对象，它在未来的某时会生成一个值：已完成（resolved）的值、或者一个没有完成的理由（例如网络错误）。一个 promise 会有 3 种可能的状态：fulfilled（已完成）、rejected（已拒绝）、pending（等待中）。Promise 的使用者可以附上回调函数来处理已完成的值或者拒绝的原因。
```js
function myPromise(constructor){
    let self=this;
    self.status="pending" //定义状态改变前的初始状态
    self.value=undefined;//定义状态为resolved的时候的状态
    self.reason=undefined;//定义状态为rejected的时候的状态
    function resolve(value){
        //两个==="pending"，保证了状态的改变是不可逆的
       if(self.status==="pending"){
          self.value=value;
          self.status="resolved";
       }
    }
    function reject(reason){
        //两个==="pending"，保证了状态的改变是不可逆的
       if(self.status==="pending"){
          self.reason=reason;
          self.status="rejected";
       }
    }
    //捕获构造异常
    try{
       constructor(resolve,reject);
    }catch(e){
       reject(e);
    }
}
myPromise.prototype.then=function(onFullfilled,onRejected){
   let self=this;
   switch(self.status){
      case "resolved":
        onFullfilled(self.value);
        break;
      case "rejected":
        onRejected(self.reason);
        break;
      default:
   }
   return self
}

function Promise(fn) {
  this.cbs = [];

  const resolve = (value) => {
    setTimeout(() => {
      this.data = value;
      this.cbs.forEach((cb) => cb(value));
    });
  }

  fn(resolve.bind(this));
}

Promise.prototype.then = function (onResolved) {
  return new Promise((resolve) => {
    this.cbs.push(() => {
      const res = onResolved(this.data);
      if (res instanceof Promise) {
        res.then(resolve);
      } else {
        resolve(res);
      }
    });
  });
};

new Promise((resolve) => {
    setTimeout(() => {
      resolve(1)
    })
  })
  .then((res) => {
    console.log("res", res)
    return 2
    // return new Promise((resolve) => {
    //   setTimeout(() => {
    //     resolve(2)
    //   })
    // })
  })
  .then((res) => {
    console.log("res", res)
  })
//注释
function Promise(fn) {
  // Promise resolve时的回调函数集
  this.cbs = [];

  // 传递给Promise处理函数的resolve
  // 这里直接往实例上挂个data
  // 然后把onResolvedCallback数组里的函数依次执行一遍就可以
  const resolve = (value) => {
    // 注意promise的then函数需要异步执行
    setTimeout(() => {
      this.data = value;
      this.cbs.forEach((cb) => cb(value));
    });
  }

  // 执行用户传入的函数
  // 并且把resolve方法交给用户执行
  fn(resolve);
}
Promise.prototype.then = function (onResolved) {
  // 这里叫做promise2
  return new Promise((resolve) => {
    this.cbs.push(() => {
      const res = onResolved(this.data);
      if (res instanceof Promise) {
        // resolve的权力被交给了user promise
        res.then(resolve);
      } else {
        // 如果是普通值 就直接resolve
        // 依次执行cbs里的函数 并且把值传递给cbs
        resolve(res);
      }
    });
  });
};

Promise.myAll = function(iterators) {
    const promises = Array.from(iterators);
    const num = promises.length;
    const resolvedList = new Array(num);
    let resolvedNum = 0;

    return new Promise((resolve, reject) => {
        promises.forEach((promise, index) => {
            Promise.resolve(promise)
                .then(value => {
                    // 保存这个promise实例的value
                    resolvedList[index] = value;
                    // 通过计数器，标记是否所有实例均 fulfilled
                    if (++resolvedNum === num) {
                        resolve(resolvedList);
                    }
                })
                .catch(reject);
        });
    });
};

//limited
function limitLoad(urls, handler, limit) {
    // 对数组做一个拷贝
    const sequence = [].concat(urls)
    let promises = [];

    //并发请求到最大数
    promises = sequence.splice(0, limit).map((url, index) => {
        // 这里返回的 index 是任务在 promises 的脚标，
        //用于在 Promise.race 之后找到完成的任务脚标
        return handler(url).then(() => {
            return index
        });
    });

    (async function loop() {
        let p = Promise.race(promises);
        for (let i = 0; i < sequence.length; i++) {
            p = p.then((res) => {
                promises[res] = handler(sequence[i]).then(() => {
                    return res
                });
                return Promise.race(promises)
            })
        }
    })()
}
limitLoad(urls, loadImg, 3)


Promise.myRace = function(iterators) {
    const promises = Array.from(iterators);

    return new Promise((resolve, reject) => {
        promises.forEach((promise, index) => {
            Promise.resolve(promise)
                .then(resolve)
                .catch(reject);
        });
    });
};

Promise.any = function(iterators) {
    const promises = Array.from(iterators);
    const num = promises.length;
    const rejectedList = new Array(num);
    let rejectedNum = 0;

    return new Promise((resolve, reject) => {
        promises.forEach((promise, index) => {
            Promise.resolve(promise)
                .then(value => resolve(value))
                .catch(error => {
                    rejectedList[index] = error;
                    if (++rejectedNum === num) {
                        reject(rejectedList);
                    }
                });
        });
    });
};
```
## 15.手写call,apply,bind
```js
//手写call
Function.prototype.myCall = function (obj) {
    const object = obj || window; //如果第一个参数为空则默认指向window对象
    let args = [...arguments].slice(1); //存放参数的数组
    object.func = this;
    const result =  object.func (...args);
    delete object.func; //记住最后要删除掉临时添加的方法，否则obj就无缘无故多了个fn
    return result;
}

//手写apply
Function.prototype.myApply = function (obj) {
    const object = obj || window; //如果第一个参数为空则默认指向window对象
    if (arguments.length > 1) {
        var args = arguments[1]; //存放参数的数组
    } else {
        var args = []; //存放参数的数组
    }
    object.func = this;
    const result = object.func(...args);
    delete object.func; //记住最后要删除掉临时添加的方法，否则obj就无缘无故多了个fn
    return result;
}
//手写bind
Function.prototype.myBind = function (obj) {
    const object = obj || window; //如果第一个参数为空则默认指向window对象
    let self = this;
    let args = [...arguments].slice(1); //存放参数的数组

    return function () {
        let newArgs = [...arguments]
        return self.apply(object, args.concat(newArgs))
    }
}
//add(1)(2)(3)(4)
function add(n) {
  var fn = function(m) {
    return add(n + m);
  };

  fn.toString = function() {
    return n;
  };


  return fn;
}
```

## 16.怎么理解es6箭头函数中的this，它和一般函数的this指向有什么区别呢=>？

* 没有this绑定，箭头函数没有自己的this，它会捕获自己在定义时所处的外层执行环境的this，并继承这个this值。所以，箭头函数中this的指向在它被定义的时候就已经确定了，之后永远不会改变。
* 没有arguments
* 不能通过 new 关键字调用
* 没有 new.target
* 没有原型
* 没有 super
* call/apply/bind方法无法改变箭头函数中this的指向
* 虽然箭头函数中的箭头不是运算符，但箭头函数具有与常规函数不同的特殊运算符优先级解析规则。
* 箭头函数不支持重名参数
* 不能使用 yield 关键字
## 17.let和const有什么区别
let与const都是只在声明所在的块级作用域内有效。 let声明的变量可以改变，值和类型都可以改变，没有限制。 const 声明的变量第一层不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值,

## 18.如何判断数据类型，如果遇到null呢
**数据类型分为基本类型和引用类型：**

* 基本类型：String、Number、Boolean、Null、Undefined
* 引用类型：Object、Array、Date、Function、Error、RegExp、Math、Number、String、Boolean、Globle 


* * *


**Object.prototype.toString**
prototype.constructor 会被修改Null和undefined不检测，不能垮iframe
instanceof不能检测出基本类型，且不能跨iframe

## 19.mvc，mvp和mvvm的区别
**MVC的优缺点**
优点：
1、把业务逻辑全部分离到Controller中，模块化程度高。当业务逻辑变更的时候，不需要变更View和Model，只需要Controller换成另外一个Controller就行了（Swappable Controller）。
2、观察者模式可以做到多视图同时更新。
缺点：
1、Controller测试困难。因为视图同步操作是由View自己执行，而View只能在有UI的环境下运行。在没有UI环境下对Controller进行单元测试的时候，Controller业务逻辑的正确性是无法验证的：Controller更新Model的时候，无法对View的更新操作进行断言。
2、View无法组件化。View是强依赖特定的Model的，如果需要把这个View抽出来作为一个另外一个应用程序可复用的组件就困难了。因为不同程序的的Domain Model是不一样的

**MVP（Passive View）的优缺点**
优点：
1、便于测试。Presenter对View是通过接口进行，在对Presenter进行不依赖UI环境的单元测试的时候。可以通过Mock一个View对象，这个对象只需要实现了View的接口即可。然后依赖注入到Presenter中，单元测试的时候就可以完整的测试Presenter业务逻辑的正确性。这里根据上面的例子给出了Presenter的单元测试样例。
2、View可以进行组件化。在MVP当中，View不依赖Model。这样就可以让View从特定的业务场景中脱离出来，可以说View可以做到对业务逻辑完全无知。它只需要提供一系列接口提供给上层操作。这样就可以做高度可复用的View组件。
缺点：
1、Presenter中除了业务逻辑以外，还有大量的View->Model，Model->View的手动同步逻辑，造成Presenter比较笨重，维护起来会比较困难。
**MVVM的优缺点**
优点：
1、提高可维护性。解决了MVP大量的手动View和Model同步的问题，提供双向绑定机制。提高了代码的可维护性。
2、简化测试。因为同步逻辑是交由Binder做的，View跟着Model同时变更，所以只需要保证Model的正确性，View就正确。大大减少了对View同步更新的测试。
缺点：
1、过于简单的图形界面不适用，或说牛刀杀鸡。
2、对于大型的图形应用程序，视图状态较多，ViewModel的构建和维护的成本都会比较高。
3、数据绑定的声明是指令式地写在View的模版当中的，这些内容是没办法去打断点debug的。

## 20.topK用什么排序？堆排序时间复杂度，稳定性以及什么是稳定排序
全局排序，取第 k 个数
我们能想到的最简单的就是将数组进行排序（可以是最简单的快排），取前 K 个数就可以了，so easy
构造前 k 个最大元素小顶堆，取堆顶
我们也可以通过构造一个前 k 个最大元素小顶堆来解决，小顶堆上的任意节点值都必须小于等于其左右子节点值，即堆顶是最小值。
所以我们可以从数组中取出 k 个元素构造一个小顶堆，然后将其余元素与小顶堆对比，如果大于堆顶则替换堆顶，然后堆化，所有元素遍历完成后，堆中的堆顶即为第 k 个最大值
具体步骤如下：
从数组中取前 k 个数（ 0 到 k-1 位），构造一个小顶堆
从 k 位开始遍历数组，每一个数据都和小顶堆的堆顶元素进行比较，如果小于堆顶元素，则不做任何处理，继续遍历下一元素；如果大于堆顶元素，则将这个元素替换掉堆顶元素，然后再堆化成一个小顶堆。
遍历完成后，堆顶的数据就是第 K 大的数据
对于一个基本有序数组用什么排序比较好？(答了冒泡)冒泡时间复杂度是多少，最好的情况是多少

## 21.实现三栏布局，中间200px，两边自适应
```css
.wrap{
  display:flex;
  flex-direction:row;
  margin-top:20px;
}
.center{
  width:800px;
  text-align:center;
  background:#ccc;
}
.left,.right{
  flex-grow: 1;
  line-height: 30px;
  background:red;
}
```
## 22.position有哪些值，定位参考哪一个元素

* static,正常流
* relative,定位的起始位置为此元素原先在文档流的位置。
* fixed,元素的位置相对于浏览器窗口是固定位置。
* absolute,定位的起始位置为最近的父元素(非static)
* sticky,基于用户的滚动位置来定位

## 23.让一个元素不可见的方法有哪些

* overflow:hidden
* opacity:0；
* visibility:hidden
* display:none
* position:absolute
* clip(clip-path):rect()/inset()/polygon()
* z-index:-1000
* transform:scaleY(0)

## 24.css3翻转
```css
.mirrorRotateLevel {
    transform: rotateY(180deg);   /* 水平镜像翻转 */
}
.mirrorRotateVertical {
    transform: rotateX(180deg);   /* 垂直镜像翻转 */
}
transform:scaleX(-1);
```
## 25.数组深拷贝，浅拷贝，对象深拷贝，浅拷贝
```js
Object.assign() Array.prototype.concat() JSON.parse(JSON.stringify())
function deepClone(source){
  const targetObj = source.constructor === Array ? [] : {}; // 判断复制的目标是数组还是对象
  for(let keys in source){ // 遍历目标
    if(source.hasOwnProperty(keys)){
      if(source[keys] && typeof source[keys] === 'object'){ // 如果值是对象，就递归一下
        targetObj[keys] = source[keys].constructor === Array ? [] : {};
        targetObj[keys] = deepClone(source[keys]);
      }else{ // 如果不是，就直接赋值
        targetObj[keys] = source[keys];
      }
    }
  }
  return targetObj;
}
function deepClone(source){
  if(typeof source == ('string'|| 'number')){
    return source;
  }
  if(!source || typeof source != 'object'){
    throw new Error("error arguments!")
  }
  var newSource = source.constructor === Array? [] : {};
  for(var key in source){
    if(source.hasOwnProperty(key)){
      if(typeof source[key] !== 'object'){
        newSource[key] = source[key]
      }else{
        newSource[key] = deepClone(source[key])
      }
    }
  }
  return newSource;
}
//循环引用
function clone(target, map = new Map()) {
    if (typeof target === 'object') {
        let cloneTarget = Array.isArray(target) ? [] : {};
        if (map.get(target)) {
            return map.get(target);
        }
        map.set(target, cloneTarget);
        for (const key in target) {
            cloneTarget[key] = clone(target[key], map);
        }
        return cloneTarget;
    } else {
        return target;
    }
};
```
## 26.webpack路由懒加载
路由懒加载也可以叫做路由组件懒加载，最常用的是通过 import() 来实现它。
function load(component) {
    return () => import(`views/${component}`)
}
然后通过Webpack编译打包后，会把每个路由组件的代码分割成一一个js文件，初始化时不会加载这些js文件，只当激活路由组件才会去加载对应的js文件。

## 27.es6中异步请求多个数据如何操作
promise.all需慎用，否则会造成其中一个请求失败而导致程序阻塞的风险，解决方法：封装的async/await中无论success/fail都是用resolve返回data，由返回值进行判断而非使用会导致阻塞的reject
## 28.对象的解构赋值
数组的元素是按次序排列的，变量的取值由它的位置决定；
对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
函数的rest参数
rest参数用于获取函数的多余参数，这样就不需要使用arguments对象了。rest参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

## 29.SPA优缺点
SPA：单页面web应用，一般整个应用只有一个html页面，通过前端路由实现无刷新跳转。
Vue就是SPA应用的典型代表，特别是配合webpack等前端构建工具，加载页面的时候将JavaScript、CSS统一加载，然后通过监听url的hash实现内容切换。
优点：
无刷新切换内容，提高用户体验。
符合前后端分离的开发思想，通过ajax异步请求数据接口获取数据，后台只需要负责数据，不用考虑渲染。前端使用vue等MVVM框架渲染数据非常合适。
减轻服务器压力，展示逻辑和数据渲染在前端完成，服务器任务更明确，压力减轻。
后端数据接口可复用，设计JSON格式数据可以在PC、移动端通用。
缺点：
不利于SEO（搜索引擎优化），应用数据是通过请求接口动态渲染，不利于SEO。
首页加载慢，SPA下大部分的资源需要在首页加载，造成首页白屏等问题。
解决首页白屏方案
优化 webpack 减少模块打包体积，code-split 按需加载
服务端渲染，在服务端事先拼装好首页所需的 html
首页加 loading 或 骨架屏 （仅仅是优化体验）
服务端开启gzip压缩
打包文件分包，提取公共文件包


## 30.mvc和mvvm的区别，mvvm是为了解决什么
ViewController太过臃肿，把业务逻辑剥离出来形成viewModel。由ViewController来调度ViewModel和View。
1、关注Model的变化，让MVVM框架去自动更新DOM的状态，从而把发者从操作DOM的繁琐步骤中解脱出来！
2、由于控制器的功能大都移动到View上处理，大大的对控制器进行了瘦身。
3、可以对View或ViewController的数据处理部分抽象出来一个函数处理model。这样它们专职页面布局和页面跳转，它们必然更一步的简化。
4、提高可维护性
5、可测试。界面素来是比较难于测试的，而现在测试可以针对ViewModel来写。
6、低耦合可重用：视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定不同的"View"上，当View变化的时候Model不可以不变，当Model变化的时候View也可以不变。你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑。

## 31.箭头函数的作用
更简短的函数；
更直观的作用域和this的绑定(不绑定this)
由于箭头函数没有自己的this指针，通过call()或者apply()方法调用一个函数时，只能传递参数(不能绑定this)，它们的第一个参数会被忽略。

## 32.xss和csrf如何防范
**XSS攻击可分为三类，分别为储存型、反射型、DOM型。**
**存储型**
1、储存型XSS攻击将恶意代码提交到网站数据库当中
2、当用户使用客户端向网站请求数据时，恶意代码从服务器传回，拼接在HTML中返回给客户端
3、客户端在解析代码时，恶意代码被执行
常见应用场景：论坛发帖、商品评论、用户私信等
**反射型**
1、反射型XSS攻击构造出包含恶意代码的url，url指向目标网站，但是参数可以拼接恶意代码
2、诱导用户点击，点击后会向服务端发送请求，同时查询参数携带恶意代码
3、服务端返回时将恶意代码直接拼接在HTML中
4、客户端接收并解析执行代码时，恶意代码也被执行
常见应用场景：通过 URL 传递参数的场景，如网站搜索、跳转
**DOM型**
DOM型XSS攻击是通过恶意代码修改DOM的结构，是单纯发生在客户端的攻击。
以下情况有可能会被利用：
1、<script>标签
2、javascript:??执行符(a标签的href属性,url上)
3、innerHTML=?? 或 .outerHTML=?? 或 setTimeout(??) 或 setInterval(??)(执行js)
4、document.write(??) 或 eval(??)
5、location、onclick、onerror、onload、onmouseover等事件（执行js）
6、在 style 属性和标签中，包含类似 background-image:url(“javascript:…”);的代码（新版本浏览器已经可以防范）。
7、在 style 属性和标签中，包含类似 expression(…) 的 CSS 表达式代码(新版本浏览器已经可以防范)

**防范方法**
现在主流的浏览器内置了防范 XSS 的措施，例如 CSP。但对于开发者来说，也应该寻找可靠的解决方案来防止 XSS 攻击。
**1、防范反射型、存储型 XSS：**
采用纯前端渲染
拼接 HTML 时，要对 HTML 进行充分转义(利用一些转义库)
**2、防范 DOM 型 XSS**
* 将用户输入插入 HTML 或拼接 js   执行时，要进行编码，将一些特殊字符转义。
* escapeHTML() 按照如下规则进行转义：
```html
& -> &amp;
<-> &lt;
> -> &gt;
"-> &quot;
' -> &#x27;
/ -> &#x2F;
```

* 对于 a 标签的 href 等外链请求，添加白名单进行过滤，禁止以 javascript: 开头的链接，和其他非法的 scheme。
* 不要相信用户的任何输入。 对于用户的任何输入要进行检查、过滤和转义。建立可信任的字符和 HTML 标签白名单，对于不在白名单之列的字符或者标签进行过滤或编码。
* 在 XSS 防御中，输入检查一般是检查用户输入的数据中是否包含 <，> 等特殊字符，如果存在，则对特殊字符进行过滤或编码，这种方式也称为 XSS Filter。

**3、其他防范措施**
HttpOnly 防止劫取 Cookie
输入检查
对于用户的任何输入要进行检查、过滤和转义。
输出检查
服务端的输出也可能会存在问题。一般来说，除富文本的输出外，在变量输出到 HTML 页面时，可以使用编码或转义的方式来防御 XSS 攻击。

##### CSRF原理
CSRF，即 Cross Site Request Forgery，中译是跨站请求伪造，是一种劫持受信任用户向服务器发送非预期请求的攻击方式。
举个例子：
小明在刷gmail，突然看到一条链接，好奇点开了发现是个空白链接，就不管他，继续刷其他的。看似没有什么事情发生，但实际上攻击者利用这个所谓的空白页给小明偷偷设置了一个过滤规则，这个规则使他收到的邮件都转发给了攻击者。
**1、攻击特点**
攻击一般发起在第三方网站，而不是被攻击的网站。被攻击的网站无法防止攻击发生。
攻击利用受害者在被攻击网站的登录凭证，冒充受害者提交操作；而不是直接窃取数据。
整个过程攻击者并不能获取到受害者的登录凭证，仅仅是“冒用”。
跨站请求可以用各种方式：图片URL、超链接、CORS、Form提交等等。部分请求方式可以直接嵌入在第三方论坛、文章中，难以进行追踪。
**2、攻击流程**
受害者登录a.com，并保留了登录凭证（Cookie）。
攻击者引诱受害者访问了b.com。
b.com 向 a.com 发送了一个请求：a.com/act=xx。浏览器会默认携带a.com的Cookie。
a.com接收到请求后，对请求进行验证，并确认是受害者的凭证，误以为是受害者自己发送的请求。
a.com以受害者的名义执行了act=xx。
攻击完成，攻击者在受害者不知情的情况下，冒充受害者，让a.com执行了自己定义的操作。
**3、防范措施**
CSRF通常从第三方网站发起，被攻击的网站无法防止攻击发生，只能通过增强自己网站针对CSRF的防护能力来提升安全性。

* 方法一：同源验证

既然CSRF大多来自第三方网站，那么我们就直接禁止外域（或者不受信任的域名）对我们发起请求。
判断是否来自外域：
在HTTP协议中，每一个异步请求都会携带两个Header，用于标记来源域名：
Origin Header
Referer Header
Referer Check防盗链
两个Header在浏览器发起请求时，大多数情况会自动带上，并且不能由前端自定义内容。 服务器可以通过解析这两个Header中的域名，确定请求的来源域。
注意：Origin 在 IE11 中不存在，在 302 重定向中也不存在，Referrer 可以被前端隐藏不携带，如果不存在这两个请求头时，需要利用其它方式验证：
当访问源是外域时，直接拒绝访问，但要注意的是，需要过滤掉 html 页面请求，因为当请求来自搜索引擎结果页面时，也是外域访问，会被当做 CSRF 请求。

* 方法二、CSRF Token 验证

1、服务器生成一个Token，并把这个Token利用算法加密。要注意，这个Token不能放在Cookie中，要不然又会被冒用，一般是放在session中。
2、在页面加载时，在每个a标签和form标签中放入Token
3、服务器验证Token是否正确
缺点：实现较为复杂，工作量大，可能出现遗漏。

* 方法三、双重Cookie验证

利用CSRF攻击不能获取到用户Cookie的特点，我们可以要求Ajax和表单请求携带一个Cookie中的值。
1、在用户访问网站页面时，向请求域名注入一个Cookie，内容为随机字符串。
2、在前端向后端发起请求时，取出Cookie，并添加到URL的参数中。
3、后端接口验证Cookie中的字段与URL参数中的字段是否一致，不一致则拒绝。
优点：
无需使用Session，适用面更广，易于实施。
Token储存于客户端中，不会给服务器带来压力。
相对于Token，实施成本更低，可以在前后端统一拦截校验，而不需要一个个接口和页面添加。
缺点：
Cookie中增加了额外的字段。
如果有其他漏洞（例如XSS），攻击者可以注入Cookie，那么该防御方式失效。
难以做到子域名的隔离。
为了确保Cookie传输安全，采用这种防御方式的最好确保用整站HTTPS的方式，如果还没切HTTPS的使用这种方式也会有风险。

* 方法四、Samesite Cookie属性

Google起草了一份草案来改进HTTP协议，那就是为Set-Cookie响应头新增Samesite属性，它用来标明这个 Cookie是个“同站 Cookie”，同站Cookie只能作为第一方Cookie，不能作为第三方Cookie，Samesite 有两个属性值，分别是 Strict 和 Lax：
方法五、验证码


## 33.es5和es6有什么区别

* 块级作用域 关键字let, 常量const
* 函数参数 - 默认值、参数打包、 数组展开（Default 、Rest 、Spread）
* 箭头函数 Arrow functions
* 字符串模板 Template strings
* 生成器 （Generators）
* Class


## 34.spa原理，为什么url改变不会刷新页面
单页的页面即为一个html页面，可以理解为，某个应用中所有的其他页面和单元均为一个预设好的根页面的子组件，通过js，css来控制众多子组件的替换和更新，从而达到模拟页面跳转的情景。我把这种应用称之为spa。
监听url中hash值得变化
HTML5 history模式

## 35.localStorage大小
我们可以看到5120KB是默认值，整个域的存储大小。

## 36.写个继承，es6继承是如何做到的，手写继承
```js

function f(phrase) {
  return class {
    sayHi() { alert(phrase) }
  }
}
class User extends f("Hello") {}
new User().sayHi(); // Hello

class CreateSoldier{
     constructor(name){
         this.type = 'soldier';
         this.name = name
     }
     shot(){
         console.log("shot")
     }
 }
 class CreateTreatSoldier extends CreateSoldier{
     constructor(name){
         super(name)
     }
     treat(){
         console.log('treat')
     }
 }
```
## 37.作用域和作用域链
在JavaScript中的作用域有全局作用域、局部作用域以及块级作用域。
局部作用域：和全局作用域相反，局部作用域的变量即是在特定代码块中才能过访问，对于外部是不能够访问的。
块级作用域：在代码块中使用let定义的变量，只能在当前代码块中进行访问。块级作用域可以形成暂时性死区。
作用域就是在函数内部可以访问外部变量的机制，使用链式查找哪些变量可以被函数内部访问。

**执行环境（Execution Context）执行上下文**
EC定义了变量和函数有权访问的其他数据。JavaScript中，函数在运行时都会产生一个执行环境，并且JS引擎还会产生一个与当前EC相关联的变量对象（Variable Object，即VO）。EC中所有定义的变量和方法都包含在VO中。全局执行环境是最外围的执行环境，它是一个“兜底”的执行环境。

* 首先，创建一个全局对象
* JS引擎会创建一个执行环境栈
* JS引擎会创造一个和EC相关连的变量对象VO
* 每一个函数在定义的时候，都会创建一个与之关联的[[scopes]]属性，该scope总是指向定义函数时的执行环境EC。
* 作用域链的作用就是保证当前环境对其有权访问的变量和方法进行有序的访问 ——JavaScript高级程序设计
* 作用域是在运行时代码中的某些特定部分中变量，函数和对象的可访问性。换句话说，作用域决定了代码区块中变量和其他资源的可见性。
* 执行上下文在运行时确定，随时可能改变；作用域在定义时就确定，并且不会改变。

## 38.什么是执行上下文？
执行上下文是评估和执行 JavaScript 代码的环境的抽象概念。每当 Javascript 代码在运行的时候，它都是在执行上下文中运行。
执行上下文的类型
JavaScript 中有三种执行上下文类型。
全局执行上下文 — 这是默认或者说基础的上下文，任何不在函数内部的代码都在全局上下文中。它会执行两件事：创建一个全局的 window 对象（浏览器的情况下），并且设置 this 的值等于这个全局对象。一个程序中只会有一个全局执行上下文。
函数执行上下文 — 每当一个函数被调用时, 都会为该函数创建一个新的上下文。每个函数都有它自己的执行上下文，不过是在函数被调用时创建的。函数上下文可以有任意多个。每当一个新的执行上下文被创建，它会按定义的顺序（将在后文讨论）执行一系列步骤。
Eval 函数执行上下文 — 执行在 eval 函数内部的代码也会有它属于自己的执行上下文，但由于 JavaScript 开发者并不经常使用 eval，所以在这里我不会讨论它。
执行栈
执行栈，也就是在其它编程语言中所说的“调用栈”，是一种拥有 LIFO（后进先出）数据结构的栈，被用来存储代码运行时创建的所有执行上下文。
当 JavaScript 引擎第一次遇到你的脚本时，它会创建一个全局的执行上下文并且压入当前执行栈。每当引擎遇到一个函数调用，它会为该函数创建一个新的执行上下文并压入栈的顶部。
引擎会执行那些执行上下文位于栈顶的函数。当该函数执行结束时，执行上下文从栈中弹出，控制流程到达当前栈中的下一个上下文。

## 39.vue数据绑定
vue是通过数据劫持的方式来做数据绑定的，其中最核心的方法便是通过Object.defineProperty()来实现对属性的劫持，达到监听数据变动的目的
Vue 的响应式，核心机制是 观察者模式。
数据是被观察的一方，发生改变时，通知所有的观察者，这样观察者可以做出响应，比如，重新渲染然后更新视图。

vue源码数据绑定以及diff算法
https://www.infoq.cn/article/uDLCPKH4iQb0cR5wGY7f

## 40.template转虚拟dom
baseCompile首先会将模板template进行parse得到一个AST语法树，再通过optimize做一些优化，最后通过generate得到render以及staticRenderFns。
```js
//dom转虚拟don
// 将dom树节点劫持到对象中中进行解析
function nodeToObject(vnode) {
  let parseProps = {}
  // 定义一个parseProps接收节点属性
  for (let [key, val] of Object.entries(vnode.attributes)) {
    parseProps[val.name] = val.value
  }
  let dealChildren = []
  // 定义一个数组接收节点的所有子节点，对于元素则递归调用nodeToObject方法
  let parsrChildren = Array.from(vnode.childNodes)
  parsrChildren.forEach((ele) => {
    if (ele instanceof Element) {
      dealChildren.push(nodeToObject(ele))
    } else {
      dealChildren.push(ele.nodeValue)
    }
  })
  let flag = {
    tag: vnode.tagName.toLowerCase(),
    props: parseProps,
    children: dealChildren
  }

  // 返回一个虚拟Dom Tree
  return h(flag.tag, flag.props, flag.children)
}
```
41.vue中计算属性如何根据data里的值发生改变
计算属性通常依赖于其他数据属性。对于依赖属性的任何改变都会触发计算属性的逻辑。计算属性基于它们的依赖关系进行缓存，因此只有当依赖项发生变化时，它们才会重新运行，否则他会使用缓存中的属性值。计算属性在默认情况下是getters，但是如果需要实现类似的功能，则可以设置setter函数。

42.vdom有什么缺点
大量DOM操作的情况下，性能肯定不如原生DOM。首次渲染大量DOM时，由于多了一层虚拟DOM的计算，会比innerHTML插入慢。
-虚拟DOM具有批处理和高效的Diff算法,最终表现在DOM上的修改只是变更的部分，可以保证非常高效的渲染,优化性能.

43.webpack用过哪些loader
raw-loader：加载文件原始内容（utf-8）
file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
url-loader：与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值会交给 file-loader 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
source-map-loader：加载额外的 Source Map 文件，以方便断点调试
svg-inline-loader：将压缩后的 SVG 内容注入代码中
image-loader：加载并且压缩图片文件
json-loader 加载 JSON 文件（默认包含）
handlebars-loader: 将 Handlebars 模版编译成函数并返回
babel-loader：把 ES6 转换成 ES5
ts-loader: 将 TypeScript 转换成 JavaScript
awesome-typescript-loader：将 TypeScript 转换成 JavaScript，性能优于 ts-loader
sass-loader：将 CSS 代码注入 JavaScript 中，通过 DOM 操作去加载 CSS
css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS
postcss-loader：扩展 CSS 语法，使用下一代 CSS，可以配合 autoprefixer 插件自动补齐 CSS3 前缀
eslint-loader：通过 ESLint 检查 JavaScript 代码
tslint-loader：通过 TSLint检查 TypeScript 代码
mocha-loader：加载 Mocha 测试用例的代码
coverjs-loader：计算测试的覆盖率
vue-loader：加载 Vue.js 单文件组件

44.有哪些常见的Plugin？你用过哪些Plugin？
define-plugin：定义环境变量 (Webpack4 之后指定 mode 会自动配置)
ignore-plugin：忽略部分文件
html-webpack-plugin：简化 HTML 文件创建 (依赖于 html-loader)
web-webpack-plugin：可方便地为单页应用输出 HTML，比 html-webpack-plugin 好用
uglifyjs-webpack-plugin：不支持 ES6 压缩 (Webpack4 以前)
terser-webpack-plugin: 支持压缩 ES6 (Webpack4)
webpack-parallel-uglify-plugin: 多进程执行代码压缩，提升构建速度

45.那你再说一说Loader和Plugin的区别？
Loader 本质就是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果。因为 Webpack 只认识 JavaScript，所以 Loader 就成了翻译官，对其他类型的资源进行转译的预处理工作。
Plugin 就是插件，基于事件流框架 Tapable，插件可以扩展 Webpack 的功能，在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。
Loader 在 module.rules 中配置，作为模块的解析规则，类型为数组。每一项都是一个 Object，内部包含了 test(类型文件)、loader、options (参数)等属性。
Plugin 在 plugins 中单独配置，类型为数组，每一项是一个 Plugin 的实例，参数都通过构造函数传入。

46.Webpack构建流程简单说一下
Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：
初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数
开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译
确定入口：根据配置中的 entry 找出所有的入口文件
编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理
完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系
输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会
输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统

47.模块打包原理知道吗？
Webpack 实际上为每个模块创造了一个可以导出和导入的环境，本质上并没有修改 代码的执行逻辑，代码执行顺序与模块加载顺序也完全一致。

48.localStorage有什么不好的
如果要做SEO，那么CSS必然不能进行LS(localstorage)的本地缓存优化。
兼容性不太好，不支持LS的浏览器比例仍然很大
网络速度快，协商缓存响应快，LS读取+eval很多时候会比不上304
通常需要SEO，导致css不能缓存，仅缓存js使得整个缓存方案意义进一步减小
浏览器本地缓存足够可靠持久
跨页面间共享缓存即便有浪费也差别不大

1. 执行速度，读取后使用eval或创建<script>标签的时间会比浏览器直接加载慢。
2. 版本控制，需要自己写一套版本控制机制。
3. localStorage是公共资源，如果你的产品域名下有很多应用共享这份资源会有风险。
4. localStorage以页面的域名划分，而常见的静态资源都以资源本身的域名来缓存，意味着如果你的应用有多个等价域名，它们之间的localStorage不互通，会造成缓存多份浪费。
5. 兼容性需要处理（不支持、隐私模式、写满、http/https、写的正确性等等）
   移动端webapp值得一试的原因在于：兼容性好网速慢，LS读取+eval大多数情况下快于304，都说是webapp了，不需要seo，css也可以缓存，再通过js加载浏览器缓存经常会被清理，LS被清理的几率低一些以模块文件为单位，缓存失效率低不同页面状态直接访问、二次访问、页面状态跳转资源组合是不确定的，不能通过url来缓存资源，否则就不“增量”啦

49.手写个简单的mvvm
手写装饰者模式
function iwatch () {
this.battery = 100;
this.getBattery = function() {
console.log(this.battery)
}
}
iwatch.prototype.getNewPart = function(part) {
this[part].prototype = this; //把this对象上的属性 指向 新对象的prototype
return new this[part]; //返回一个新对象，不修改原对象，新增了新对象的属性
}
iwatch.prototype.addNetwork = function() {
this.network = function() {
console.log('network')
}
}
iwatch.prototype.addSwim = function() {
this.swim = function() {
console.log('swim')
}
}
var watch = new iwatch();
watch.getBattery(); // 100
watch = watch.getNewPart('addNetwork'); // 添加新行为，network()
watch = watch.getNewPart('addSwim'); // 既有network方法，也有swim方法
//手写观察者
var Jack = {
subscribers: {
'any': []
},
//添加订阅
subscribe: function (type = 'any', fn) {
if (!this.subscribers[type]) {
this.subscribers[type] = [];
}
this.subscribers[type].push(fn); //将订阅方法保存在数组里
},
//退订
unsubscribe: function (type = 'any', fn) {
this.subscribers[type] =
this.subscribers[type].filter(function (item) {
return item !== fn;
}); //将退订的方法从数组中移除
},
//发布订阅
publish: function (type = 'any', ...args) {
this.subscribers[type].forEach(function (item) {
item(...args);  //根据不同的类型调用相应的方法
});
}
};

50.讲一下this绑定
当一个函数没有明确的调用对象的时候，也就是单纯作为独立函数调用的时候，将对函数的this使用默认绑定：绑定到全局的window对象
当函数被一个对象“包含”的时候，我们称函数的this被隐式绑定到这个对象里面了，这时候，通过this可以直接访问所绑定的对象里面的其他属性，
隐式丢失指的是函数中的 this 丢失绑定对象，即它会应用第 1 条的默认绑定规则，从而将 this 绑定到全局对象或者 undefined 上，取决于是否在严格模式下运行。
显式绑定的核心是 JavaScript 内置的 call(..) 和 apply(..) 方法，这两个方法在 JavaScript 提供的绝大多数函数以及开发者自己创建的所有函数上都可以使用。
call和bind的区别是：在绑定this到对象参数的同时：
1.call将立即执行该函数
2.bind不执行函数，只返回一个可供执行的函数

51.new做了什么事情
使用 new 来调用函数时，会自动执行下面的操作：
创建一个全新的对象
这个新对象会被执行 [[原型]] 连接
这个新对象会绑定到函数调用的 this
如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象
new 绑定 > 显示绑定 > 隐式绑定 > 默认绑定
手写new
function create() {
// 创建一个空的对象
let obj = new Object()
// 获得构造函数
let Con = [].shift.call(arguments)
// 链接到原型
obj.__proto__ = Con.prototype
// 绑定 this，执行构造函数
let result = Con.apply(obj, arguments)
// 确保 new 出来的是个对象
return typeof result === 'object' ? result : obj
}
52.http讲一下，缓存策略，etag了解吗
在客户端第一次请求数据时，此时缓存数据库中没有对应的缓存数据，需要请求服务器，服务器返回后，将数据存储至缓存数据库中。

53.prototype属性怎么用
prototype就是“一个给类的对象添加方法的方法”，使用prototype属性，可以给类动态地添加方法，以便在JavaScript中实现“继承”的效果。

54.jQuery和vue有哪些区别，分别使用场景
jQuery是使用选择器（$）选取DOM对象，对其进行赋值、取值、事件绑定等操作，其实和原生的HTML的区别只在于可以更方便的选取和操作DOM对象，而数据和界面是在一起的。比如需要获取label标签的内容：$("lable").val();,它还是依赖DOM元素的值。
Vue则是通过Vue对象将数据和View完全分离开来了。对数据进行操作不再需要引用相应的DOM对象，可以说数据和View是分离的，他们通过Vue对象这个vm实现相互的绑定。
vue适用的场景：复杂数据操作的后台页面，表单填写页面
jquery适用的场景：比如说一些html5的动画页面，一些需要js来操作页面样式的页面

55.为什么选择vue
Vue 给我带来的解决问题的能力

56.h5新特性
语义化更好的标签元素
结构元素：article、aside、header、hgroup、footer、figure、section、nav
其他元素：video、audio、canvas、embed、mark、progress、meter、time、command、details、datagrid、keygen、output、source、menu、ruby、wbr、bdi、dialog、
Canvas：首先获取canvas元素的上下文对象，然后使用该上下文对象中的绘图功能进行绘制
SVG：SVG是html5的另一项图形功能，是一种标准的矢量图形，是一种文件格式，有自己的API。
音频和视频：2大好处,一是作为浏览器原生支持的功能，新的audio和video元素无需安装；二是媒体元素向web页面提供了通用、集成和可脚本化控制的API。

57.你做前端有什么优势
优势在于可以帮业务人员快速定位和理顺一些需求逻辑和实现思路，帮大家提高代码的设计感，有后端服务开发经验，和运维后端人员交流合作无障碍，和客户端的团队交流也没有技术瓶颈，领会能力强，喜欢和善于交流。还有一个优势，就是遇到问题不服软的精神，能做攻关项目，善于做超前的技术改造和自研的技术方案。
工作态度端正，学会换位思考，处理问题先找自己问题，如果不是自己的问题，尽量帮他人解决问题或提供解决方案。
不管是软件开发项目，还是教学任务，都要去从一个更全面的视角考虑问题，而不仅仅是前端开发这个视角。

58.vuex原理
Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，每一个 Vuex 应用的核心就是 store（仓库）。store是一个容器，它包含着你的应用中大部分的状态 (state)。
Vuex用于管理页面数据状态、提供统一数据操作的生态系统。它集中于Model层，规定所有的数据操作必须通过 action - mutation - state change 的流程来进行，再结合Vue的数据视图双向绑定特性来实现页面的展示更新。统一的页面状态管理以及操作处理，可以让复杂的组件交互变得简单清晰，同时可在调试模式下进行时光机般的倒退前进操作，查看数据改变过程

1. Vue Components：Vue组件。HTML页面上，负责接收用户操作等交互行为，执行dispatch方法触发对应action进行回应。
2. dispatch：操作行为触发方法，是唯一能执行action的方法。
3. actions：操作行为处理模块。负责处理Vue Components接收到的所有交互行为。包含同步/异步操作，支持多个同名方法，按照注册的顺序依次触发。向后台API请求的操作就在这个模块中进行，包括触发其他action以及提交mutation的操作。该模块提供了Promise的封装，以支持action的链式触发。
4. commit：状态改变提交操作方法。对mutation进行提交，是唯一能执行mutation的方法。
5. mutations：状态改变操作方法。是Vuex修改state的唯一推荐方法，其他修改方式在严格模式下将会报错。该方法只能进行同步操作，且方法名只能全局唯一。操作之中会有一些hook暴露出来，以进行state的监控等。
6. state：页面状态管理容器对象。集中存储Vue components中data对象的零散数据，全局唯一，以进行统一的状态管理。页面显示所需的数据从该对象中进行读取，利用Vue的细粒度数据响应机制来进行高效的状态更新。
7. getters：state对象读取方法。图中没有单独列出该模块，应该被包含在了render中，Vue Components通过该方法读取全局state对象。

Vue组件接收交互行为，调用dispatch方法触发action相关处理，若页面状态需要改变，则调用commit方法提交mutation修改state，通过getters获取到state新值，重新渲染Vue Components，界面随之更新。

59.水平垂直居中
块级元素使用margin: 0 auto;可以在父元素的中间位置居中
position 元素已知宽度 绝对定位+margin反向偏移
position transform 元素未知宽度 如果元素未知宽度，只需将上面example2中的margin: -50px 0 0 -50px;替换为：transform: translate(-50%,-50%);
//https://juejin.im/post/6844903919144075278
.warp {
background-color: #FF8C00;
width: 200px;
height: 200px;
display: flex;
justify-content: center; /*使子项目水平居中*/
align-items: center; /*使子项目垂直居中*/
}
.example3 {
background-color: #F00;
width: 100px;
height: 100px;
}
div使用绝对布局，设置margin:auto;并设置top、left、right、bottom的值相等即可，不一定要都是0。给子元素相对定位，在通过translaY（）得到垂直居中
利用inline-block的vertical-align: middle去对齐after伪元素
display: flex-box

60.盒模型
一个盒子中主要的属性就5个：width、height、padding、border、margin。

61.数组去重
//https://segmentfault.com/a/1190000016418021
利用ES6 Set去重（ES6中最常用）
利用for嵌套for，然后splice去重
利用indexOf去重
利用对象的属性不能相同的特点进行去重
利用includes
利用hasOwnProperty
利用filter
利用递归去重
利用Map数据结构去重
利用reduce+includes
[...new Set(arr)]

62.将一个多维数组变为一维数组
function recursive(arr, newArr) {
arr.forEach(item => {
Array.isArray(item) ? recursive(item, newArr) : newArr.push(item)
})
}
let newArr = [];
recursive(arr, newArr)

let flattenDeep = (arr) => arr.reduce((start, current) => (
Array.isArray(current) ?
start.concat(...flattenDeep(current)) :
start.concat(current)
), [])

let newArr2 = flattenDeep(arr);

//二维变一位
let newArr = arr.reduce((start, current) => (start.concat(current)), [])
let newArr2 = [].concat.apply([], arr)
let newArr1 = [].concat(...arr)
//一维变二维
let group = (arr, length) => {
let index = 0;
let newArr = [];
while (index < arr.length) {
newArr.push(arr.slice(index, index += length))
}
return newArr;
}

63.JSONP的底层实现 手写jsonp
function jsonp() {
return new Promise((resolve, reject) => {
var fn = data => {
if (data.code === 200) {
resolve(data);
} else {
reject(data);
}
};
var script = document.createElement("script");
script.src = "url?callback=fn";
document.body.append(script);
});
}

64.ajax同步和异步的区别
当ajax为异步请求时，在ajax向服务端发去请求后，在等待server返回结果的时候，浏览器会继续执行ajax方法后面的代码，直到server返回结果后再执行ajax的success或者error。

65.window.onload何时执行，浏览器如何渲染原理
load 应该仅用于检测一个完全加载的页面 当一个资源及其依赖资源已完成加载时，将触发load事件
浏览器会对这个 html 文件进行编译
DOM 构建，将文档中的所有 DOM 元素构建成一个树型结构。
CSS 构建，render 树将 DOM 树和 CSS 合并成一棵渲染树，
render树在合适的时机会被渲染到页面中。

66.当我们输入一个页面地址时，发生了哪些事情呢？

1. 浏览器首先下载该地址所对应的 html 页面。
2. 浏览器解析 html 页面的 DOM 结构。
3. 开启下载线程对文档中的所有资源按优先级排序下载。
4. 主线程继续解析文档，到达 head 节点 ，head 里的外部资源无非是外链样式表和外链 js。
   发现有外链 css 或者外链 js，如果是外链 js ，则停止解析后续内容，等待该资源下载，下载完后立刻执行。如果是外链 css，继续解析后续内容。
5. 解析到 body
   body 里的情况比较多，body 里可能只有 DOM 元素，可能既有 DOM、也有 css、js 等资源，js 资源又有可能异步加载 图片、css、js 等。DOM 结构不同，浏览器的解析机制也不同，我们分开来讨论。
   只有 DOM 元素
   这种情况比较简单了，DOM 树构建完，页面首次渲染。
   有 DOM 元素、外链 js。
   当解析到外链 js 的时候，该 js 尚未下载到本地，则 js 之前的 DOM 会被渲染到页面上，同时 js 会阻止后面 DOM 的构建，即后面的 DOM 节点并不会添加到文档的 DOM 树中。所以，js 执行完之前，我们在页面上看不到该 js 后面的 DOM 元素。
   有 DOM 元素、外链 css
   外链 css 不会影响 css 后面的 DOM 构建，但是会阻碍渲染。简单点说，外链 css 加载完之前，页面还是白屏。
   有 DOM 元素、外链 js、外链 css
   外链 js 和外链 css 的顺序会影响页面渲染，这点尤为重要。当 body 中 js 之前的外链 css 未加载完之前，页面是不会被渲染的。
   当body中 js 之前的 外链 css 加载完之后，js 之前的 DOM 树和 css 合并渲染树，页面渲染出该 js 之前的 DOM 结构。
6. 文档解析完毕，页面重新渲染。当页面引用的所有 js 同步代码执行完毕，触发 DOMContentLoaded 事件。
7. html 文档中的图片资源，js 代码中有异步加载的 css、js 、图片资源都加载完毕之后，load 事件触发。

66.js字符串有哪些方法
str.charAt(1) //e 返回给定位置的字符
str.charcodeAt(1) //101 返回给定位置字符的字符编码
str[1] //e ie8+
concat() //可以接受任意多个参数拼接成新的字符串，但不会改变原字符串
slice() //截取字符串，接受一或两个参数(开始位置和结束位置)，接受负值时会将负值与字符串长度相加
substring() //截取字符串，接受一或两个参数(开始位置和结束位置，会将较小的参数作为起始位置)，接受负值时会将负的参数转换为零
substr() //截取字符串，接受一或两个参数(开始位置和截取的字符个数)，接受负值时会将第一个负的参数加上字符串长度，将第二个负的参数转换为0
indexOf() //可接受两个参数，要查找的子字符串和查找起点(可选)，找到返回位置，找不到返回-1
lastIndexOf() //从数组的末尾开始查找
trim() //删除前置和后缀的空格 返回的是字符串的副本，原始字符串不变
toLowerCase() //转小写
toUpperCase() //转大写
toLocaleLowerCase() //转小写，针对地区的方法
toLocaleUpperCase() //转大写，针对地区的方法
match() //接收一个参数，正则表达式或者RegExp对象
search() //接受一个正则，返回字符串中第一个匹配项的索引，没有返回-1
replace() //替换字符串。接受两个参数，第一个是一个字符串或者RegExp对象，
//第二个参数是一个字符串或者函数。如果第一个参数是一个字符串，
//那么只会替换第一个子字符串，要想替换所有唯一的方法就是提供一个
//正则表达式，指定全局g标志
//replace()方法的第二个参数也可以是一个函数
function(match,...,pos,originalText){
match //模式的匹配项
... //正则表达式定义了多个捕获组的情况下，是第二，三...匹配项
pos //模式的匹配项在字符串中的位置
originalText //原始字符串
}
split() //分割字符串，并返回一个数组。第一个参数接受一个分隔符(可以是字符串或者RegExp对象)，
//可选的第二个参数用于指定返回数组的大小
localeCompare() //比较两个字符串，如果字符串在字母表中应该排在字符串参数之前，返回一个负数。相等返回0，之后返回正数
String.fromCharcode() //构造函数本身的静态方法，接收一个或多个字符编码，转换成字符串，与charCodeAt相反

67.jq的domready是怎么实现的
var $ = ready = window.ready = function(fn){
if(document.addEventListener){//兼容非IE
document.addEventListener("DOMContentLoaded",function(){
//注销事件，避免反复触发
document.removeEventListener("DOMContentLoaded",arguments.callee,false);
fn();//调用参数函数
},false);
}else if(document.attachEvent){//兼容IE
document.onreadystatechange = function() {
if (document.readyState == 'complete') {
document.onreadystatechange = null;
document.attachEvent("onreadystatechange",function(){
document.detachEvent("onreadystatechange",arguments.callee);
fn();//调用参数函数
});
}
};
}
}
ready(function(){
alert(1);
});

68.怎么获取一个元素的宽高

1. window.getComputedStyle()、element.currentStyle
   offsetHeight、offsetWidth、offsetTop、offsetLeft、officeParent
   clientWidth、clientHeight
   scrollHeight、 scrollWidth、 scrollTop、 scrollLeft
   getBoundingClientRect()

69.快速排序快排
var quickSort = function(arr) {
if (arr.length <= 1) {
return arr;
}
var pivotIndex = Math.floor(arr.length / 2);
var pivot = arr.splice(pivotIndex, 1)[0];
var left = [];
var right = [];

for (var i = 0; i < arr.length; i++) {
if (arr[i] < pivot) {
left.push(arr[i]);
} else {
right.push(arr[i]);
}
}
return quickSort(left).concat([pivot], quickSort(right));
};

//
function swap(A, i, j) {
const t = A[i];
A[i] = A[j];
A[j] = t;
}

/**
*

* @param {*} A  数组
* @param {*} p  起始下标
* @param {*} r  结束下标 + 1
  */
  function divide(A, p, r) {
  const x = A[r - 1];
  let i = p - 1;

for (let j = p; j < r - 1; j++) {
if (A[j] <= x) {
i++;
swap(A, i, j);
}
}

swap(A, i + 1, r - 1);

return i + 1;
}

/**
*

* @param {*} A  数组
* @param {*} p  起始下标
* @param {*} r  结束下标 + 1
  */
  function qsort(A, p = 0, r) {
  r = r || A.length;

if (p < r - 1) {
const q = divide(A, p, r);
qsort(A, p, q);
qsort(A, q + 1, r);
}

return A;
}
70.二分查找
function binarySearch(target,arr,start,end) {
if( start > end){return -1}
var start   = start || 0;
var end     = end || arr.length-1;

var mid = parseInt(start+(end-start)/2);
if(target==arr[mid]){
return mid;
}else if(target>arr[mid]){
return binarySearch(target,arr,mid+1,end);
}else{
return binarySearch(target,arr,start,mid-1);
}
return -1;
}

//二分快排查找
function binarySearch(target,arr) {
while (arr.length>0){
//使用快速排序。以mid为中心划分大小，左边小，右边大。
var left    = [];
var right   = [];
//选择第一个元素作为基准元素(基准元素可以为任意一个元素)
var pivot   = arr[0];
//由于取了第一个元素，所以从第二个元素开始循环
for(var i=1;i<arr.length;i++){
var item = arr[i];
//大于基准的放右边，小于基准的放左边
item>pivot ? right.push(item) : left.push(item);
}

```````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````
    //得到经过排序的新数组
    if(target==pivot){
        return true;
    }else if(target>pivot){
        arr     = right;
    }else{
        arr     = left;
    }
}
return false;
```````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````

}

71.盒模型有几种，什么区别，怎么相互转换
CSS盒子模型组成：外边距（margin）、边框（border）、内边距（padding）、内容（content）。
**1，****IE盒子模型**
　　　　　　CSS中的宽（width）=内容（content）的宽+（border+padding）*2
　　　　　　CSS中的高（height）=内容（content）的高+（border+padding）*2
**2，****W3C标准盒子模型**
　　　　　　CSS中的宽（width）=内容（content）的宽
　　　　　　CSS中的高（height）=内容（content）的高
**3，它们之间的区别**
假如我们给定一个宽度，在W3C标准盒子模型中，给定的宽度只作用在content上，而在IE盒子模型中则作用在content+padding+border上
**4**，**它们之间的相互转换**
可以用过这句代码实现它们之间的相互转换**box-sizing：border-box**

72.display的常见值有哪些
none：此元素不显示。
block：将元素显示为块级元素，前后会带换行符。
inline:默认值，元素会被显示为内联元素，前后没有换行符。
inline-block:行内块级元素。

73.display：none 和 visible：hidden的区别， 有没有事件，保不保留空间
使用display:none，在文档渲染时，该元素如同不存在（但依然存在文档对象模型树中）；而使用visibility :hidden，其占的空间会被空白占位。即一个（display:none）不会在渲染树中出现，一个（visibility :hidden）会。
display:none，会触发reflow（回流），进行渲染。
visibility:hidden，只会触发repaint（重绘），因为没有发现位置变化，不进行渲染。

74.map和foreach遍历的区别
map会返回一个新数组，不对原数组产生影响,foreach不会产生新数组，
map因为返回数组所以可以链式操作，foreach不能
foreach不能break

75.移动端适配，常用单位
vw : 1vw 为视口宽度的 1%
vh : 1vh 为视口高度的 1%
vmin : vw 和 vh 中的较小值
vmax : 选取 vw 和 vh 中的较大值
rem中的 r代表root，它表示以根（即html）元素的单位大小为基准来设置当前元素的单位大小，所以不管当前元素是任意子节点，一旦设单位大小为 rem 那么这个元素大小都是以根元素单位为参考的，这里的em 和 rem 均具有继承性。
"vw" 的全称是 viewport width 即视窗的宽度；"vh" 的全称是 viewport height 即视窗的高度。
1vw = viewportWidth * 1/100； 1vh = viewportHeight * 1/100；
所以元素使用 “vw” “vh” 作为宽度和高度单位，即可以保证适配不同的设备。
vmin 和 vmax“vmin” 即 “viewport” 宽度和高度相比较最小的那一个。
1 ÷ 父元素的font-size × 需要转换的像素值 = em值

76.移动端开法和pc端开发的区别
手机端的字体显示问题
iPhone上submit按钮bug
移动端的自动播放功能
移动端设备的分辨率
宽度的响应
图片的处理
多了触摸、滑动（这里我没自己写过原生的，只会用一些插件）
虚拟键盘弹出导致 fixed 元素错位
屏幕旋转事件：window.orientation
可点击标签点击时有默认样式
最小点击区域44px：移动端是有最小点击识别区的，元素大小低于这个值时被点击是不会触发click事件的。
js事件不同：click事件在移动端反应有300ms延迟，点击反应慢还会出现点透的bug。应尝试使用touch事件，或者使用fastclick.js库，也可以使用zeptojs中的tap事件。

77.css3动画做过哪些
animation、transition、transform、translate

78.flex
Flexible Box，弹性布局，用来为盒状模型提供最大的灵活性。可以实现类似垂直居中布局。
.box {
flex-direction: row | row-reverse | column | column-reverse;
}
// row（默认值）：主轴为水平方向，起点在左端
// row-reverse：主轴为水平方向，起点在右端
// column：主轴为垂直方向，起点在上沿
// column-reverse：主轴为垂直方向，起点在下沿
.box {
justify-content: flex-start | flex-end | center | space-between | space-around;
}
// 具体对齐方式与轴的方向有关。下面假设主轴为从左到右
// flex-start（默认值）：左对齐
// flex-end：右对齐
// center： 居中
// space-between：两端对齐，项目之间的间隔都相等。
// space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
.box {
align-items: flex-start | flex-end | center | baseline | stretch;
}
// 具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下
// flex-start：交叉轴的起点对齐。
// flex-end：交叉轴的终点对齐。
// center：交叉轴的中点对齐。
// baseline: 项目的第一行文字的基线对齐。
// stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

79.vue双向绑定是什么？手写一个vue双向绑定。
vue数据双向绑定是通过数据劫持Object.defineProperty( )结合发布者-订阅者模式的方式来实现的
实现过程：

1. 首先要对数据进行劫持监听，所以我们需要设置一个监听器Observer，用来监听所有属性
2. 如果属性发上变化了，就需要告诉订阅者Watcher看是否需要更新。
3. 因为订阅者是有很多个，所以我们需要有一个消息订阅器Dep来专门收集这些订阅者，然后在监听器Observer和订阅者Watcher之间进行统一管理的。
4. 我们还需要有一个指令解析器Compile，对每个节点元素进行扫描和解析，将相关指令对应初始化成一个订阅者Watcher，并替换模板数据或者绑定相应的函数，此时当订阅者Watcher接收到相应属性的变化，就会执行对应的更新函数，从而更新视图。
   手写双向绑定
   const obj = {};
   Object.defineProperty(obj, 'text', {
   get: function() {
   console.log('get val'); 
   },
   set: function(newVal) {
   console.log('set val:' + newVal);
   document.getElementById('input').value = newVal;
   document.getElementById('span').innerHTML = newVal;
   }
   });
   const input = document.getElementById('input');
   input.addEventListener('keyup', function(e){
   obj.text = e.target.value;
   })

80.两个ajax请求，怎么做能够最快完成，几种方法
//模拟ajax异步操作1
function ajax1() {
const p = new Promise((resolve, reject) => {
setTimeout(function() {
resolve('ajax 1 has be loaded!')
}, 1000)
})
return p

}
//模拟ajax异步操作2
function ajax2() {
const p = new Promise((resolve, reject) => {
setTimeout(function() {
resolve('ajax 2 has be loaded!')
}, 2000)
})
return p
}
//等待两个ajax异步操作执行完了后执行的方法
const myFunction = async function() {
const x = await ajax1()
const y = await ajax2()
//等待两个异步ajax请求同时执行完毕后打印出数据
console.log(x, y)
}
myFunction()

81.0-1000，乱序的数组。将其中一个改成-1，怎么找到被改的数字以及被改数字的位置
indexOf(-1),所有数字加起来差多少

82.输入一个网址到页面显示经历了那些步骤，输入url发生了什么
DNS解析
一个递归查询的过程。
查找顺序： 浏览器缓存--> 操作系统缓存--> 本地host文件 --> 路由器缓存--> ISP DNS缓存 --> 顶级DNS服务器/根DNS服务器
TCP连接
三次握手以建立TCP连接
第一次握手：建立连接时，客户端发送syn包（syn=j）到服务器，并进入SYN_SENT状态，等待服务器确认；
第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；
第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1），此包发送完毕，客户端和服务器进入ESTABLISHED（TCP连接成功）状态，完成三次握手。
发送HTTP请求
建立了TCP连接之后，发起一个http请求。
服务器处理请求并返回HTTP报文
后端从在固定的端口接收到TCP报文开始，它会对TCP连接进行处理，对HTTP协议进行解析，并按照报文格式进一步封装成HTTP Request对象，供上层使用。
经过前面的6个步骤，服务器收到了我们的请求，也处理我们的请求，到这一步，它会把它的处理结果返回，也就是返回一个HTPP响应。
浏览器解析渲染页面渲染原理
构建dom树 -> 构建render树 -> 布局render树 -> 绘制render树
连接结束
现在的页面为了优化请求的耗时，默认都会开启持久连接（keep-alive），那么一个TCP连接确切关闭的时机，是这个tab标签页关闭的时候。这个关闭的过程就是著名的四次挥手。关闭是一个全双工的过程，发包的顺序的不一定的。一般来说是客户端主动发起的关闭，过程如下。
对于一个已经建立的连接，TCP使用改进的三次握手来释放连接（使用一个带有FIN附加标记的报文段）。TCP关闭连接的步骤如下：
第一步，当主机A的应用程序通知TCP数据已经发送完毕时，TCP向主机B发送一个带有FIN附加标记的报文段（FIN表示英文finish）。
第二步，主机B收到这个FIN报文段之后，并不立即用FIN报文段回复主机A，而是先向主机A发送一个确认序号ACK，同时通知自己相应的应用程序：对方要求关闭连接（先发送ACK的目的是为了防止在这段时间内，对方重传FIN报文段）。
第三步，主机B的应用程序告诉TCP：我要彻底的关闭连接，TCP向主机A送一个FIN报文段。
第四步，主机A收到这个FIN报文段后，向主机B发送一个ACK表示连接彻底释放。

83.301和302的区别：
  301和302状态码都表示重定向，就是说浏览器在拿到服务器返回的这个状态码后会自动跳转到一个新的URL地址，这个地址可以从响应的Location首部中获取（用户看到的效果就是他输入的地址A瞬间变成了另一个地址B）——这是它们的共同点。
  他们的不同在于。301表示旧地址A的资源已经被永久地移除了（这个资源不可访问了），搜索引擎在抓取新内容的同时也将旧的网址交换为重定向之后的网址；
  302表示旧地址A的资源还在（仍然可以访问），这个重定向只是临时地从旧地址A跳转到地址B，搜索引擎会抓取新的内容而保存旧的网址。 SEO302好于301

84.http协议一些问题，请求头里面都有啥
Accept 请求报文可通过一个Accept报文头属性告诉服务端 客户端期望服务器返回的媒体格式
Accept-Charset 表示客户端期望服务器返回的内容的编码格式。
Content-Length 表示传输的请求／响应的Body的长度
Cookie 客户端的Cookie就是通过这个报文头属性传给服务端的哦
Referer 表示这个请求是从哪个URL过来的
Cache-Control 对缓存进行控制
响应头
Cache-Control 响应输出到客户端后，服务端通过该报文头属告诉客户端如何控制响应内容的缓存。
ETag 一个代表响应服务端资源（如页面）版本的报文头属性，如果某个服务端资源发生变化了，这个ETag就会相应发生变化。它是Cache-Control的有益补充，可以让客户端“更智能”地处理什么时候要从服务端取资源，什么时候可以直接从缓存中返回响应。 用来判断缓存资源的有效性
If-Match的值一般是上面提到的ETag的值，它常用于HTTP的乐观锁。所谓HTTP乐观锁，是指客户端先GET这个资源得到ETag中的版本号，然后发起一个资源修改请求PUT|PATCH时通过If-Match头来指定资源的版本号，如果服务器资源满足If-Match中指定的版本号，请求就会被执行。如果不满足，说明资源被并发修改了，就需要返回状态码为412 Precondition failed 的错误。客户端可以选择放弃或者重试整个过程。
Location  服务器向客户端发送302跳转的时候，总会携带Location头信息，它的值为目标URL。
Expires 服务器使用Expect头来告知对方资源何时失效
Content-Type 内容的媒体类型和编码格式，
Last-Modified标记资源的最近修改时间，它和Date比较类似，区别是Last-Modified代表修改时间，而Date是创建时间。
If-Modified-Since浏览器向服务器请求静态资源时，如果浏览器本地已经有了缓存，就会携带If-Modified-Since头，值为资源的Last-Modified时间，询问服务器该资源自从这个Last-Modified时间之后有没有被修改。如果没有修改过，就会向浏览器返回304 Not Modified通知浏览器可以放心使用缓存内的资源。如果资源修改过，那就像正常的GET请求一样，携带资源的内容返回200 OK。
Set-Cookie 服务端可以设置客户端的Cookie，其原理就是通过这个响应报文头属性实现的
Referer Referer是非常常用的头，它表示请求的发起来源URI，也就是当前页面资源的父页面。
User-Agent 携带当前的用户代理信息，
Cache-Control这可能是HTTP头里面最复杂的一个头了。这个头既可以用于请求，也可以用于响应。在请求和响应的取值不一样，分别代表了不同的意思。
no-cache 如果no-cache没有指定值，那就表示不允许缓存。对于请求来说，服务器不得使用缓存内容直接返回。对于响应来说，客户端不得缓存响应的资源内容。如果no-cache指定了值，那就表示值对应的头信息不得使用缓存，其它的信息还是可以缓存的。告知对方我只要新鲜刚出浴的数据。
no-store 告知对方不要持久化请求/响应数据到其它地方，这种信息是敏感的，要保持它的易失性。告知对方记在心里(memory)就行，别写在纸上(disk)。
no-transform 告知对方不要转换数据。比如客户端上传了raw图像数据，服务器一般都会选择性压缩图像数据进行存储。no-transform告知对方保留原始数据信息，不要进行任何转换。告知对方不要乱动我发过来的东西。
only-if-cached 用于请求头，告知服务器只要那些已经缓存的内容，不要去reload。如果没有缓存内容就返回504 Gateway Timeout错误。表示客户端不想太麻烦服务器，有就给，没就算了。
max-age 用于请求头。限制缓存内容的年龄，如果超过max-age年龄的，需要服务器去reload内容资源。这叫客户端的年龄歧视。
max-stale 用于请求头。客户端允许服务器返回缓存已过期的资源内容，但是限定了最大过期时间。表示客户端虽然很宽容，那是也是有限度的。
min-fresh 用于请求头。客户端限制服务器不要那些即将过期的资源内容。就好比我们去超市买牛奶，如果牛奶快过期了虽然还在保质期内咱们也就不会考虑。
public 用于响应头。表示允许客户端缓存响应信息，并可以给别人使用。比如代理服务器缓存静态资源供所有代理用户使用。
private 用于响应头。表示仅允许客户端缓存响应信息给自己使用，不得分享给别人。这样是为了禁止代理服务器进行缓存，而允许客户端自己缓存资源内容。意思是你个人留着用就行，别借给别人用。
Connection: keep-alive  当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。 Connection: close 代表一个Request完成后，客户端和服务器之间用于传输HTTP数据的TCP连接会关闭， 当客户端再次发送Request，需要重新建立TCP连接。

85.cookie是什么
Cookie是用来存储一些用户信息以便让服务器辨别用户身份的，比如cookie会存储一些用户的用户名和密码，当用户登录后就会在客户端产生一个cookie来存储相关信息，这样浏览器通过读取cookie的信息去服务器上验证并通过后会判定你是合法用户，从而允许查看相应网页。当然cookie里面的数据不仅仅是上述范围，还有很多信息可以存储是cookie里面，比如sessionid等。

86.说一说语义化编程
什么是语义化？其实简单说来就是让机器可以读懂内容。
我们可以让机器的理解能力越来越接近人类，人能看懂、听懂的东西，机器也能理解；
我们应该在发布内容的时候，就用机器可读的、被广泛认可的语义信息来描述内容，来降低机器处理 Web 内容的难度（HTML 本身就已经是朝这个方向迈出的一小步了）。
从广义上来说，不仅要使机器（搜索引擎等）易于理解，也要使人易于理解。在团队协作开发中，对人的易于理解显得尤为重要了，一个莫名其妙的 class 会让后续的开发或者维护者一头雾水，增加了协作成本

87.vue的优化
v-show，v-if 用哪个？
不要在模板里面写过多的表达式与判断
循环调用子组件时添加 key
人开发时尽量保持每个组件 export default {} 内的方法顺序一致，方便查找对应的方法。
钩子理解好生命周期的含义就好，什么时间应该请求，什么时间注销方法，哪些方法需要注销。简单易懂，官网都有写。
watch 和 computed 用哪个的问题
组件有明确含义，只处理类似的业务。复用性越高越好，配置性越强越好。
自己封装组件还是遵循配置 props 细化的规则。
组件分类，建议路由控制的每个模块都新建一个文件夹，然后对其每个组件再进行细化，一些小的组件，例如 icon, scrollTop 等，可以单独新建一个文件夹 common 存放。
通过require方式或者import()方式动态加载组件
组件懒加载
当网站足够大时，一个状态树下，根的部分字段繁多，解决这个问题就要模块化 vuex，官网提供了模块化方案，允许我们在初始化 vuex 的时候配置 modules。
屏蔽sourceMap
对项目代码中的JS/CSS/SVG(*.ico)文件进行gzip压缩
内容类系统的图片资源按需加载
SSR(服务端渲染)
better-click防止iphone点击延迟
骨架屏加载
慎用deep watch：

88.回流和重绘
回流必将引起重绘，重绘不一定会引起回流。
当Render Tree中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流。
当render tree中的一些元素需要更新属性，而这些属性只是影响元素的外观、风格，而不影响布局(例如：background-color)，则称为重绘(repaints)
回流的事件：
页面首次渲染
浏览器窗口大小发生改变
元素尺寸或位置发生改变
元素内容变化（文字数量或图片大小等等）
元素字体大小变化
添加或者删除可见的DOM元素
激活CSS伪类（例如：:hover）
查询某些属性或调用某些方法

1. 添加或者删除可见的DOM元素；
   重绘何时发生，元素的属性或者样式发生变化。
   会导致回流的操作：
   clientWidth、clientHeight、clientTop、clientLeft
   offsetWidth、offsetHeight、offsetTop、offsetLeft
   scrollWidth、scrollHeight、scrollTop、scrollLeft
   scrollIntoView()、scrollIntoViewIfNeeded()
   getComputedStyle()
   getBoundingClientRect()
   scrollTo()
   有时即使仅仅回流一个单一的元素，它的父元素以及任何跟随它的元素也会产生回流。
   现代浏览器会对频繁的回流或重绘操作进行优化：浏览器会维护一个队列，把所有引起回流和重绘的操作放入队列中，如果队列中的任务数量或者时间间隔达到一个阈值的，浏览器就会将队列清空，进行一次批处理，这样可以把多次回流和重绘变成一次。
   当你访问以下属性或方法时，浏览器会立刻清空队列：
   clientWidth、clientHeight、clientTop、clientLeft
   offsetWidth、offsetHeight、offsetTop、offsetLeft
   scrollWidth、scrollHeight、scrollTop、scrollLeft
   width、height
   getComputedStyle()
   getBoundingClientRect()
   因为队列中可能会有影响到这些属性或方法返回值的操作，即使你希望获取的信息与队列中操作引发的改变无关，浏览器也会强行清空队列，确保你拿到的值是最精确的。

CSS
避免使用table布局。
尽可能在DOM树的最末端改变class。
避免设置多层内联样式。
将动画效果应用到position属性为absolute或fixed的元素上。
避免使用CSS表达式（例如：calc()）。

JavaScript
避免频繁操作样式，最好一次性重写style属性，或者将样式列表定义为class并一次性更改class属性。
避免频繁操作DOM，创建一个documentFragment，在它上面应用所有DOM操作，最后再把它添加到文档中。
也可以先为元素设置display: none，操作结束后再把它显示出来。因为在display属性为none的元素上进行的DOM操作不会引发回流和重绘。
避免频繁读取会引发回流/重绘的属性，如果确实需要多次使用，就用一个变量缓存起来。
对具有复杂动画的元素使用绝对定位，使它脱离文档流，否则会引起父元素及后续元素频繁回流。
如何避免、减少回流和重绘
减少对render tree的操作【合并多次多DOM和样式的修改】
减少对一些style信息的请求，尽量利用好浏览器的优化策略

添加css样式，而不是利用js控制样式
让要操作的元素进行“离线处理”，处理完后一起更新
当用DocumentFragment进行缓存操作，引发一次回流和重绘
使用display:none技术，只引发两次回流和重绘
使用cloneNode(true or false)和replaceChild技术，引发一次回流和重绘
直接改变className，如果动态改变样式，则使用cssText（考虑没有优化的浏览器）
// bad
elem.style.left = x + "px";
elem.style.top = y + "px";
// good
elem.style.cssText += ";left: " + x + "px;top: " + y + "px;";
不要经常访问会引起浏览器flush队列的属性，如果你确实要访问，利用缓存
// bad
for (var i = 0; i < len; i++) {
el.style.left = el.offsetLeft + x + "px";
el.style.top = el.offsetTop + y + "px";
}
// good
var x = el.offsetLeft,
y = el.offsetTop;
for (var i = 0; i < len; i++) {
x += 10;
y += 10;
el.style = x + "px";
el.style = y + "px";
}
让元素脱离动画流，减少回流的Render Tree的规模
$("#block1").animate({left:50});
$("#block2").animate({marginLeft:50});
将需要多次重排的元素，position属性设为absolute或fixed，这样此元素就脱离了文档流，它的变化不会影响到其他元素。例如有动画效果的元素就最好设置为绝对定位；
避免使用table布局：尽量不要使用表格布局，如果没有定宽表格一列的宽度由最宽的一列决定，那么很可能在最后一行的宽度超出之前的列宽，引起整体回流造成table可能需要多次计算才能确定好其在渲染树中节点的属性，通常要花3倍于同等元素的时间。
尽量将需要改变DOM的操作一次完成
let box = document.getElementById("box").style;
// bad
box.color = "red";    // 重绘
box.size = "14px";    // 回流、重绘
// good
box.bord = '1px solid red'
尽可能在DOM树的最末端改变class，尽可能在DOM树的里面改变class（可以限制回流的范围）
IE中避免使用JavaScript表达式
当我们需要对DOM对一系列修改的时候，可以通过以下步骤减少回流重绘次数：
使元素脱离文档流
对其进行多次修改
将元素带回到文档中。
使用css3硬件加速，可以让transform、opacity、filters这些动画不会引起回流重绘 。但是对于动画的其它属性，比如background-color这些，还是会引起回流重绘的，不过它还是可以提升这些动画的性能。

89. get和post的区别
    GET的语义是请求获取指定的资源。GET方法是安全、幂等、可缓存的（除非有 Cache-ControlHeader的约束）,GET方法的报文主体没有任何语义。POST的语义是根据请求负荷（报文主体）对指定的资源做出处理，具体的处理方式视资源类型而不同。POST不安全，不幂等，（大部分实现）不可缓存。
    GET 用于获取信息，是无副作用的，是幂等的，且可缓存
    POST 用于修改服务器上的数据，有副作用，非幂等，不可缓存
    GET在浏览器回退时是无害的，而POST会再次提交请求。
    GET产生的URL地址可以被Bookmark，而POST不可以。
    GET请求会被浏览器主动cache，而POST不会，除非手动设置。
    GET请求只能进行url编码，而POST支持多种编码方式。
    GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
    GET请求在URL中传送的参数是有长度限制的，而POST么有。
    对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
    GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
    GET参数通过URL传递，POST放在Request body中。
    使用Get请求时,参数在URL中显示,而使用Post方式,则放在send里面
    使用Get请求发送数据量小,Post请求发送数据量大
    使用Get请求安全性低，会被缓存，而Post请求反之
90. 两个排序数组合并为一个排序数组，不能重新申请空间，第一个数组足够大
    var merge = function(nums1, m, nums2, n) {
    let len1 = m - 1;
    let len2 = n - 1;
    let len = m + n - 1;
    while(len1 >= 0 && len2 >= 0) {

    ``````````````````````````````````````````````````````````````````````````
    nums1[len--] = nums1[len1] > nums2[len2] ? nums1[len1--] : nums2[len2--];
    ``````````````````````````````````````````````````````````````````````````


    }
    function arrayCopy(src, srcIndex, dest, destIndex, length) {
    dest.splice(destIndex, length, ...src.slice(srcIndex, srcIndex + length));
    }
    // 表示将nums2数组从下标0位置开始，拷贝到nums1数组中，从下标0位置开始，长度为len2+1
    arrayCopy(nums2, 0, nums1, 0, len2 + 1);
    }
91. 八个小球，有一个比较重，用天平称两次，找出重的那个
    3 3 2

92.栈和堆的区别
堆是在程序运行时，而不是在程序编译时，申请某个大小的内存空间,即动态分配内存，对其访问和对一般内存的访问没有区别。堆是指程序运行是申请的动态内存，而栈只是指一种使用堆的方法(即先进后出)。
(1）管理方式不同。栈由操作系统自动分配释放，无需我们手动控制；堆的申请和释放工作由程序员控制，容易产生内存泄漏；
（2）空间大小不同。每个进程拥有的栈的大小要远远小于堆的大小。理论上，程序员可申请的堆大小为虚拟内存的大小，进程栈的大小 64bits 的 Windows 默认 1MB，64bits 的 Linux 默认 10MB；
（3）生长方向不同。堆的生长方向向上，内存地址由低到高；栈的生长方向向下，内存地址由高到低。
（4）分配方式不同。堆都是动态分配的，没有静态分配的堆。栈有2种分配方式：静态分配和动态分配。静态分配是由操作系统完成的，比如局部变量的分配。动态分配由alloca函数进行分配，但是栈的动态分配和堆是不同的，他的动态分配是由操作系统进行释放，无需我们手工实现。
（5）分配效率不同。栈由操作系统自动分配，会在硬件层级对栈提供支持：分配专门的寄存器存放栈的地址，压栈出栈都有专门的指令执行，这就决定了栈的效率比较高。堆则是由C/C++提供的库函数或运算符来完成申请与管理，实现机制较为复杂，频繁的内存申请容易产生内存碎片。显然，堆的效率比栈要低得多。
（6）存放内容不同。栈存放的内容，函数返回地址、相关参数、局部变量和寄存器内容等。堆中具体存放内容是由程序员来填充的

93. 垃圾回收机制是怎样的
    现在各大浏览器垃圾回收有两种方法：标记清除（mark and sweep）、引用计数(reference counting)。
    1.标记清除（mark and sweep）是当变量进入环境时，将这个变量标记为“进入环境”。当变量离开环境时，则将其标记为“离开环境”。标记“离开环境”的就回收内存。
    引用计数(reference counting)跟踪记录每个值被引用的次数。
    什么情况会引起内存泄漏？
94. 意外的全局变量引起的内存泄漏。
95. 闭包引起的内存泄漏
96. 没有清理的DOM元素引用
97. 被遗忘的定时器或者回调
98. 子元素存在引用引起的内存泄漏

94、什么放在内存中？什么不放在内存中？
基本类型是：Undefined/Null/Boolean/Number/String
引用类型：object

1. 堆栈空间分配区别：
   　　1、栈（操作系统）：由操作系统自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈；
   　　2、堆（操作系统）： 一般由程序员分配释放，若程序员不释放，程序结束时可能由OS回收，分配方式倒是类似于链表。
2. 堆栈缓存方式区别：
   　　1、栈使用的是一级缓存， 他们通常都是被调用时处于存储空间中，调用完毕立即释放；
   　　2、堆是存放在二级缓存中，生命周期由虚拟机的垃圾回收算法来决定（并不是一旦成为孤儿对象就能被回收）。所以调用这些对象的速度要相对来得低一些。
3. 堆栈数据结构区别：
   　　堆（数据结构）：堆可以被看成是一棵树，如：堆排序；
   　　栈（数据结构）：一种先进后出的数据结构。

95、如何实现一个倒计时功能，类似于蘑菇街中的秒杀。
用setTimeout模拟setInterval消除时间误差
用Workjs开新线程避免阻塞，消除误差

96、一个矩形，里面一个樱桃，过樱桃做一条直线， 并且没有数据和测量工具，如果做到评分矩形呢？
对角线焦点

97、说一下同源策略
所谓同源是指，域名，协议，端口相同。
当一个浏览器的两个tab页中分别打开来 百度和谷歌的页面当浏览器的百度tab页执行一个脚本的时候会检查这个脚本是属于哪个页面的，即检查是否同源，只有和百度同源的脚本才会被执行。
如果非同源，那么在请求数据时，浏览器会在控制台中报一个异常，提示拒绝访问。
非同源受到的限制
cookie不能读取 （如我在自己的站点无法读取博客园用户的cookie）
dom无法获得
ajax请求不能发送

98.JSONP的基本原理
动态添加一个<script>标签，而script标签的src属性是没有跨域的限制的。
这样一来,这种跨域方式就与ajax XmlHttpRequest协议无关了。

99、vue中的路由时如何管理的？ 你知道他的实现方式吗？
更新视图而不重新请求页面;vue-router在实现单页面前端路由时，提供了两种方式：Hash模式和History模式；根据mode参数来决定采用哪一种方式。
vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。 Hash模式通过锚点值的改变，根据不同的值，渲染指定DOM位置的不同数据
可以通过配置子链接的方式去一层层的加载组件布局
可以为hash的改变添加监听事件：原生js实现hashRouter主要是监听它的hashchange事件的变化，然后拿到对应的location.hash更新对应的视图
每一次改变hash(window.location.hash)，都会在浏览器访问历史中增加一个记录。
transitionTo()方法是用来处理路由变化中的基础逻辑的，push()方法最主要的是对window的hash进行了直接赋值：
history模式
能够实现history路由跳转不刷新页面得益与H5提供的pushState(),replaceState()等方法，这些方法都是也可以改变路由状态（路径），但不作页面跳转，我们可以通过location.pathname来显示对应的视图
一般的需求场景中，hash模式与history模式是差不多的，根据MDN的介绍，调用history.pushState()相比于直接修改hash主要有以下优势：
pushState设置的新url可以是与当前url同源的任意url,而hash只可修改#后面的部分，故只可设置与当前同文档的url
pushState设置的新url可以与当前url一模一样，这样也会把记录添加到栈中，而hash设置的新值必须与原来不一样才会触发记录添加到栈中
pushState通过stateObject可以添加任意类型的数据记录中，而hash只可添加短字符串
pushState可额外设置title属性供后续使用
实现方式
https://juejin.im/post/6844903906024095751
'abstract'模式，不涉及和浏览器地址的相关记录，流程跟'HashHistory'是一样的，其原理是通过数组模拟浏览器历史记录栈的功能

100、retina屏幕的了解
window.devicePixelRatio，使用矢量图

101、说一说移动端的布局。 flexible。
我们在页面上统一使用rem来布局。
// set 1rem = viewWidth / 10
function setRemUnit () {
var rem = docEl.clientWidth / 10
docEl.style.fontSize = rem + 'px'
}
setRemUnit();
var metaEL= doc.querySelector('meta[name="viewport"]');
var dpr = window.devicePixelRatio;
var scale = 1 / dpr
metaEl.setAttribute('content', 'width=device-width, initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
vh、vw方案即将视觉视口宽度 window.innerWidth和视觉视口高度 window.innerHeight 等分为 100 份
vw(Viewport's width)：1vw等于视觉视口的1%
vh(Viewport's height) :1vh 为视觉视口高度的1%
vmin : vw 和 vh 中的较小值
vmax : 选取 vw 和 vh 中的最大值

102、知道原理吗，怎么自己去实现一个flexible。
首先通过设置meta，其主要作用的是width=device-width，使用这个之后，document.documentElement.clientWidth就等于设备独立像素的宽度
然后给root元素设置fontSize为document.documentElement.clientWidth的十分之一，这样1rem就等于document.documentElement.clientWidth/10,以此做适配。
rem布局的实现原理。移动端的点透是什么，有没有了解
rem 的font size of the root element (根元素的字体大小) ，
rem作用于非根元素时，相对于根元素字体大小；rem作用于根元素字体大小时，相对于其出初始字体大小其实
rem布局的本质是等比缩放，一般是基于宽度，试想一下如果UE图能够等比缩放，那该多么美好啊
假设我们将屏幕宽度平均分成100份，每一份的宽度用x表示，x = 屏幕宽度 / 100，如果将x作为单位，x前面的数值就代表屏幕宽度的百分比

103、说一说异步编程的方式有哪些。
Async/Await
Promise
回调函数（Callback）
事件监听
这种方式下，异步任务的执行不取决于代码的顺序，而取决于某个事件是否发生。
发布订阅
我们假定，存在一个"信号中心"，某个任务执行完成，就向信号中心"发布"（publish）一个信号，其他任务可以向信号中心"订阅"（subscribe）这个信号，从而知道什么时候自己可以开始执行。这就叫做"发布/订阅模式"（publish-subscribe pattern），又称"观察者模式"（observer pattern）。
生成器Generators/ yield
Generator 函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同，Generator 最大的特点就是可以控制函数的执行。

104、有没有遇到过这样的问题： 一个有border的div，里面有一个图片，发现图片和下面的border有一定的空隙（baseline）。
第一，给图片img标签display:block。
　　img{display:block}
第二，定义容器里的字体大小为0。
　　div {
　　width:110px;
　　border:1px solid #000000;
　　font-size:0
　　}
第三，定义图片img标签vertical-align:bottom，vertical-align:middle，vertical-align:top
　　img{vertical-align:bottom}
图片文字等inline元素默认是和父级元素的baseline对齐的，而baseline又和父级底边有一定距离（这个距离和font-size，font-family 相关，不一定是 5px），所以设置vertical-align:top/bottom/text-top/text-bottom都可以避免这种情况出现。而且不光li，其他的block元素中包含img也会有这个现象。
至于这里的HTML属性align=”center”（对于图片浏览器会处理成align=”middle”），就相当于vertical-align:middle;所以道理也是一样的，只要vertical-align不取baseline，这个空隙就消失了。
vertical-align:bottom;

105、函数调用的方式有哪些。他们的区别是什么。
普通调用
方法调用
new
call、apply

106、说一说原型链
JavaScript 中没有类的概念的，主要通过原型链来实现继承。通常情况下，继承意味着复制操作，然而 JavaScript默认并不会复制对象的属性，相反，JavaScript
只是在两个对象之间创建一个关联（原型对象指针），这样，一个对象就可以通过委托访问另一个对象的属性和函数，所以与其叫继承，委托的说法反而更准确些。
原型链的作用
访问对象实例属性，有则返回，没有就通过 __proto__ 去它的原型对象查找。
原型对象找到即返回，找不到，继续通过原型对象的 __proto__ 查找。
一层一层一直找到 Object.prototype ，如果找到目标属性即返回，找不到就返回 undefined，不会再往下找，因为在往下找 __proto__ 就是 null 了。
__proto__ 是非标准属性，如果要访问一个对象的原型，建议使用 ES6 新增的 Reflect.getPrototypeOf 或者 Object.getPrototypeOf() 方法，而不是直接 obj.__proto__，因为非标准属性意味着未来可能直接会修改或者移除该属性。同理，当改变一个对象的原型时，最好也使用 ES6 提供的 Reflect.setPrototypeOf 或 Object.setPrototypeOf。
每个对象都有一个__proto__属性，并且指向它的prototype原型对象
每个构造函数都有一个prototype原型对象
prototype原型对象里的constructor指向构造函数本身
hasOwnProperty 是 JavaScript 中唯一一个处理属性但是不查找原型链的函数.

107.什么是继承
子类的创建可以增加新数据、新功能，可以继承父类全部的功能，但是不能选择性的继承父类的部分功能。继承是类与类之间的关系，不是对象与对象之间的关系。
原型链继承，就是让对象实例通过原型链的方式串联起来，当访问目标对象的某一属性时，能顺着原型链进行查找，从而达到类似继承的效果。

108.类型转换规则+法规则
如果有一个是对象，则遵循对象对原始值的转换过程(Date对象直接调用toString完成转换，其他对象通过valueOf转化，如果转换不成功则调用toString)
如果两个都是对象，两个对象都遵循步骤1转换到字符串
两个数字，进行算数运算
两个字符串，直接拼接
一个字符串一个数字，直接拼接为字符串
valueOf偏向于运算，toString偏向于显示。

1. 在进行对象转换时（例如:alert(a)）,将优先调用toString方法，如若没有重写toString将调用valueOf方法，如果两方法都没有重写，按Object的toString输出。
2. 在进行强转字符串类型时将优先调用toString方法，强转为数字时优先调用valueOf。
3. 在有运算操作符的情况下，valueOf的优先级高于toString。

109.说一说异步编程。
程序的执行顺利能按照编程的顺序从上至下执行，程序执行到某些复杂的、耗时的任务时，往往要开启异步任务去执行，这样就不会阻塞当前任务，让当前任务继续执行，当异步任务执行完后，再将异步任务执行完的结果传给当前任务使用。所以异步编程主要为提高程序整体的执行效率。

110.说一说回调地狱是什么，有什么问题。异常捕获怎么做。
回调函数嵌套回调函数的问题
不阻止后续执行的捕获,提供一个失败的回调函数
假设有这样一个提交按钮，当你点击之后，就会提交一次任务。当你点下按钮的那一刻，首先要先判断是否有权限提交，没有权限就弹出错误。有权限提交之后，还要请求一次，判断当前任务是否已经存在，如果存在，弹出错误。如果不存在，这个时候就可以安心提交任务了。

111.说一说promise。 一个promise有多个then，如果第一个then出错，后面的还会执行吗，如何捕获异常。
不会，用catch接收异常如，果没有使用catch方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。Promise 内部的错误不会影响到 Promise 外部的代码，通俗的说法就是“Promise 会吃掉错误”

112.登录状态怎么保持？
session机制保持会话
cookie机制保持会话
token机制保持会话

113.你所知道的前端优化有哪些内容？
优化手段1：合并请求
优化手段2：域名拆分
优化手段3：开启Gzip
优化手段4：开启KeepAlive
开启KeepAlive能够减少浏览器与服务器建立连接的次数，从而节省建立连接时间。本组实验的对象有两个
优化手段5：Minify
Minify指的是将JS和CSS等文本文件进行最小化处理，一般对于CSS来说就是去除空格去除换行，资源泄漏，大图片检测
规则07：避免使用CSS表达式
规则05：将CSS样式表放在顶部
规则09：减少DNS查询
规则03：添加Expires头
规则02：使用内容发布网络（CDN的使用）
规则14：使Ajax可缓存
针对页面中主动的Ajax请求返回的数据要缓存到本地，当然这个是针对短期内不会变化的数据
使用 HTTP2
样式置顶，脚本置底
样式文件的引用（<link/>）放在html文档的head标签中，脚本文件的引用（<script />）放在body的底部；
另外尽量减少在页面中出现片段性（内联）的style和script代码。
精简资源
压缩、合并CSS，JS文件
对图片文件进行精简优化，方式如：
OptiPNG工具无损优化PNG图片；
使用webp格式；
减少http请求次数
合并css、js文件；
使用css sprites技术；
icon图片过多的情况下，考虑使用iconfont技术；
延迟加载
使用reauire.js或sea.js按需加载js模块；
使用lazyload.js插件延迟加载图片；
删除重复脚本
重复脚本的问题一般出现在规范稀松的团队或者多部门协作开发的团队中，尽量减少这类问题的出现；重复脚本一般在开发时即可发现，因为大多数情况下这会导致变量覆盖（如多次引用jQuery），功能无法正常使用。
缓存Ajax 最快的Ajax请求就是没有请求。

114.vue中v-if和v-show的区别是什么？
为v-if在显示隐藏过程中有DOM的添加和删除，v-show就简单多了，只是操作css

115.一个DOM树，其中有两个节点，找出这两个节点公共的父节点？ （视频面试）
function commonParentNode(oNode1, oNode2) {
if(oNode1.contains(oNode2)) {
return oNode1;
}
else {
return commonParentNode(oNode1.parentNode,oNode2);  // 递归的使用
}
}

116.什么是虚拟DOM
Virtual dom, 即虚拟DOM节点。它通过JS的Object对象模拟DOM中的节点，然后再通过特定的render方法将其渲染成真实的DOM节点。
Diff算法中有很多种情况，接下来我们以常见的几种情况做下讨论：
当节点类型相同时，去看一下属性是否相同 产生一个属性的补丁包 {type:'ATTRS', attrs: {class: 'list-group'}}
新的dom节点不存在 {type: 'REMOVE', index: xxx}
节点类型不相同 直接采用替换模式  {type: 'REPLACE', newNode: newNode}
文本的变化：{type: 'TEXT', text: 1}

117、反转二叉树？
var invertTree = function(root) {
if(!root) return null
if(root) {
var left = root.left
root.left = root.right
root.right = left
}
invertTree(root.left)
invertTree(root.right)
return root
};

118.说一下强缓存和协商缓存？

1. 浏览器在加载资源时，根据请求头的expires和cache-control判断是否命中强缓存，是则直接从缓存读取资源，不会发请求到服务器。
2. 如果没有命中强缓存，浏览器一定会发送一个请求到服务器，通过last-modified和etag验证资源是否命中协商缓存，如果命中，服务器会将这个请求返回，但是不会返回这个资源的数据，依然是从缓存中读取资源
3. 如果前面两者都没有命中，直接从服务器加载资源
4. 相同点
   如果命中，都是从客户端缓存中加载资源，而不是从服务器加载资源数据；
5. 不同点
   强缓存不发请求到服务器，协商缓存会发请求到服务器。
   强缓存
   强缓存通过Expires和Cache-Control两种响应头实现
   协商缓存
   当浏览器对某个资源的请求没有命中强缓存，就会发一个请求到服务器，验证协商缓存是否命中，如果协商缓存命中，请求响应返回的http状态为304并且会显示一个Not Modified的字符串
   协商缓存是利用的是【Last-Modified，If-Modified-Since】和【ETag、If-None-Match】这两对Header来管理的
6. Last-Modified，If-Modified-Since
   Last-Modified 表示本地文件最后修改日期，浏览器会在request header加上If-Modified-Since（上次返回的Last-Modified的值），询问服务器在该日期后资源是否有更新，有更新的话就会将新的资源发送回来
   但是如果在本地打开缓存文件，就会造成 Last-Modified 被修改，所以在 HTTP/1.1 出现了 ETag
7. ETag、If-None-Match
   Etag就像一个指纹，资源变化都会导致ETag变化，跟最后修改时间没有关系，ETag可以保证每一个资源是唯一的
   If-None-Match的header会将上次返回的Etag发送给服务器，询问该资源的Etag是否有更新，有变动就会发送新的资源回来
   ETag的优先级比Last-Modified更高
   具体为什么要用ETag，主要出于下面几种情况考虑：
   一些文件也许会周期性的更改，但是他的内容并不改变(仅仅改变的修改时间)，这个时候我们并不希望客户端认为这个文件被修改了，而重新GET；
   某些文件修改非常频繁，比如在秒以下的时间内进行修改，(比方说1s内修改了N次)，If-Modified-Since能检查到的粒度是s级的，这种修改无法判断(或者说UNIX记录MTIME只能精确到秒)；
   某些服务器不能精确的得到文件的最后修改时间。
   200：强缓Expires/Cache-Control存失效时，返回新的资源文件
   200(from cache): 强缓Expires/Cache-Control两者都存在，未过期，Cache-Control优先Expires时，浏览器从本地获取资源成功
   304(Not Modified )：协商缓存Last-modified/Etag没有过期时，服务端返回状态码304
8. Expires
   Expires是http1.0提出的一个表示资源过期时间的header，它描述的是一个绝对时间，由服务器返回。
   Expires 受限于本地时间，如果修改了本地时间，可能会造成缓存失效
   Cache-Control
   Cache-Control 出现于 HTTP/1.1，优先级高于 Expires ,表示的是相对时间
   Cache-Control: no-cache
   在发布缓存副本之前，强制要求缓存把请求提交给原始服务器进行验证(协商缓存验证)。
   Cache-Control: no-store才是真正的不缓存数据到本地
   Cache-Control: public可以被所有用户缓存（多用户共享），包括终端和CDN等中间代理服务器
   Cache-Control: private只能被终端浏览器缓存（而且是私有缓存），不允许中继缓存服务器进行缓存
   各类缓存技术优缺点
9. cookie
   优点：对于传输部分少量不敏感数据，非常简明有效
   缺点：容量小（4K），不安全（cookie被拦截，很可能暴露session）；原生接口不够友好，需要自己封装；需要指定作用域，不可以跨域调用
10. Web Storage
    容量稍大一点（5M），localStorage可做持久化数据存储
    支持事件通知机制，可以将数据更新的通知发送给监听者
    缺点：本地储存数据都容易被篡改，容易受到XSS攻击
    缓存读取需要依靠js的执行，所以前提条件就是能够读取到html及js代码段，其次文件的版本更新控制会带来更多的代码层面的维护成本，所以LocalStorage更适合关键的业务数据而非静态资源
    Cookie的作用是与服务器进行交互，作为HTTP规范的一部分而存在 ，而Web Storage仅仅是为了在本地“存储”数据而生
11. indexDB
    IndexedDb提供了一个结构化的、事务型的、高性能的NoSQL类型的数据库，包含了一组同步/异步API，这部分不好判断优缺点，主要看使用者。
12. Manifest（已经被web标准废除）
    优点
    可以离线运行
    可以减少资源请求
    可以更新资源
    缺点
    资源，需要二次刷新才会被页面采用
    不支持增量更新，只有manifest发生变化，所有资源全部重新下载一次
    缺乏足够容错机制，当清单中任意资源文件出现加载异常，都会导致整个manifest策略运行异常
    Manifest被移除是技术发展的必然，请拥抱Service Worker吧
13. PWA(Service Worker)
    这位目前是最炙手可热的缓存明星，是官方建议替代Application Cache（Manifest）的方案
    作为一个独立的线程，是一段在后台运行的脚本，可使web app也具有类似原生App的离线使用、消息推送、后台自动更新等能力
    目前有三个限制
    不能访问 DOM
    不能使用同步 API
    需要HTTPS协议

119.localStorage，sessionStorage和cookie的区别

1. cookie：4K，可以手动设置失效期
2. localStorage：5M，除非手动清除，否则一直存在
3. sessionStorage：5M，不可以跨标签访问，页面关闭就清理
4. indexedDB：浏览器端数据库，无限容量，除非手动清除，否则一直存在
   共同点：都是保存在浏览器端、且同源的
   区别：
   cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递，而sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下
   存储大小限制也不同，cookie数据不能超过4K，同时因为每次http请求都会携带cookie、所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大
   数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭之前有效；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie：只在设置的cookie过期时间之前有效，即使窗口关闭或浏览器关闭
   作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localstorage在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的
   web Storage支持事件通知机制，可以将数据更新的通知发送给监听者
   web Storage的api接口使用更方便

120.跨域问题？
https://segmentfault.com/a/1190000018017118
https://juejin.im/post/6844903767226351623
当协议、子域名、主域名、端口号中任意一个不相同时，都算作不同域。
CORS需要浏览器和服务器同时支持
整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。
因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。
对于简单请求，浏览器直接发出CORS请求。
具体来说，就是在头信息之中，增加一个Origin字段:
Access-Control-Allow-Origin
Access-Control-Allow-Credentials
Access-Control-Expose-Headers
非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。
非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。
浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。
只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。
服务器收到"预检"请求以后，检查了Origin、Access-Control-Request-Method和Access-Control-Request-Headers字段以后，确认允许跨源请求，就可以做出回应。
CORS与JSONP的使用目的相同，但是比JSONP更强大。

JSONP和CORS比较
JSONP只支持GET请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。
JSONP优缺点
JSONP优点是简单兼容性好，可用于解决主流浏览器的跨域数据访问的问题。缺点是仅支持get方法具有局限性,不安全可能会遭受XSS攻击。
postMessage
postMessage是HTML5 XMLHttpRequest Level 2中的API，且是为数不多可以跨域操作的window属性之一，它可用于解决以下方面的问题：
页面和其打开的新窗口的数据传递
多窗口之间消息传递
页面与嵌套的iframe消息传递
上面三个场景的跨域数据传递
postMessage()方法允许来自不同源的脚本采用异步方式进行有限的通信，可以实现跨文本档、多窗口、跨域消息传递。

121.说一下设计模式？
https://juejin.im/post/6844903897790677006#heading-5
工厂模式
工厂模式的设计思想是通过一个工厂函数，快速批量地建立一系列相同的类。我们也可以用来创建对象、方法。
单例模式
单例模式，即保证一个类有且仅有一个实例。作用就是避免重复创建对象，优化性能。
观察者模式/订阅-发布模式
订阅-发布模式，发布者维护一份存有订阅者信息的列表，当满足某个触发条件时，就会通知列表里的所有订阅者。实现起来也很简单：

122、websocket聊天室如果发送失败了，你怎么解决这个问题？如何做到发送图片？ 有了文字、图片等不同的数据类型之后，你如何实现数据的存储，如何设计，前端如何获取？
自己做个缓存验证，也就是每一条发送的消息带一个请求编号，收到服务器回复了这条消息发送成功，才把发送缓存里面对应的消息删掉。有error报出来时自动取最后一条消息，填充到输入框。

123、websocket聊天室有输入框，那么怎么获取的，对于使用div来模仿textarea，我说使用正则去掉div，那么用户输入的也是div呢？   如果发送失败了呢？  究竟应该是先发送，还是应该先存储到redux中，考虑用户体验（仿照微信）。
用 Blob 或 ArrayBuffer 对象。

124、websocket的使用，底层是如何处理的。（类似于xhr的。）、
Ajax轮询 的原理非常简单，让浏览器隔个几秒就发送一次请求，询问服务器是否有新信息。
long poll 其实原理跟 ajax轮询 差不多，都是采用轮询的方式，不过采取的是阻塞模型（一直打电话，没收到就不挂电话），也就是说，客户端发起连接后，如果没消息，就一直不返回Response给客户端。直到有消息才返回，返回完之后，客户端再次建立连接，周而复始。
WebSocket是HTML5下一种新的协议。它实现了浏览器与服务器全双工通信，能更好的节省服务器资源和带宽并达到实时通讯的目的。它与HTTP一样通过已建立的TCP连接来传输数据，但是它和HTTP最大不同是：WebSocket是一种双向通信协议。在建立连接后，WebSocket服务器端和客户端都能主动向对方发送或接收数据，就像Socket一样；WebSocket需要像TCP一样，先建立连接，连接成功后才能相互通信。

127、手写代码 判断是否是浏览器环境。
function isapp() {
if (/^.+(Mobile\/\w+)\s?$/.test(navigator.userAgent)) {
// IOS端APP
return true;
} else {
// IOS端浏览器
return false;
}
}
exports = typeof window === 'undefined' ? global : window ;

128、https需要多少时间 比http慢多少 怎么优化
HTTPS未经任何优化的情况下要比HTTP慢几百毫秒以上，特别在移动端可能要慢500毫秒以上，
首先第一步也是最简单的一个优化策略，就是减少完全握手的发生，因为完全握手它非常消耗时间；
对于不能减少的完全握手，对于必须要发生的完全握手，对于需要直接消耗CPU进行的握手，我们使用代理计算；
第二个策略是Session Ticket，同样它也是客户端发起握手时会携带上的扩展，服务器拿到Session Ticket后会对它进行解密，如果解密成功了就意味着它是值得信任的，从而可以进行简化握手，直接传输应用层数据。
分布式缓存（分布式Session Cache）
CDN接入：CDN 节点通过和业务服务器维持长连接、会话复用和链路质量优化等可控方法，极大减少接入延时。
硬件加速：采用专用的 SSL 解密卡，能够具有更高的 HTTPS 接入能力且不影响业务程序的。
升级成 HTTP2：HTTP2 利用 TLS/SSL 带来的优势，通过修改协议的方法来提升 HTTPS 的性能，提高下载速度等。前面提到的又拍云在 HTTPS 协议的基础上已实现全平台支持 HTTP2。开启 TLS 1.3：相比 TLS 1.2 ，TLS 1.3 的握手时间会减半。这意味着访问一个移动端网站，使用 TLS 1.3 协议，可能会减少将近 100ms 的时间。另外TLS 1.3 的开启，也能让网站变得更安全。

129.https有什么缺点
HTTPS 多次握手，会一定程度上降低用户访问速度
网站改用 HTTPS 以后，由 HTTP 跳转到 HTTPS 的方式增加了用户访问耗时（多数网站采用 301、302 跳转）
HTTPS 涉及到的安全算法会消耗 CPU 资源，需要增加大量机器（HTTPS 访问过程需要加解密）
SSL 证书费用较很高，以及其在服务器上的部署、更新维护非常繁琐

130.http2有哪些特性 头部压缩怎么回事
维护一份相同的静态字典（Static Table），包含常见的头部名称，以及特别常见的头部名称与值的组合；
维护一份相同的动态字典（Dynamic Table），可以动态地添加内容；
支持基于静态哈夫曼码表的哈夫曼编码（Huffman Coding）；
静态字典的作用有两个：1）对于完全匹配的头部键值对，例如 :method: GET，可以直接使用一个字符表示；2）对于头部名称可以匹配的键值对，例如 cookie: xxxxxxx，可以将名称使用一个字符表示。
同时，浏览器可以告知服务端，将 cookie: xxxxxxx 添加到动态字典中，这样后续整个键值对就可以使用一个字符表示了。类似的，服务端也可以更新对方的动态字典。需要注意的是，动态字典上下文有关，需要为每个 HTTP/2 连接维护不同的字典。
同一个连接上产生的请求和响应越多，动态字典积累得越全，头部压缩效果也就越好。所以，针对 HTTP/2 网站，最佳实践是不要合并资源，不要散列域名。

131.100层 1个花瓶仍 找到n层不碎 n + 1层碎了的情况 2个花瓶呢
14

132.事件代理机制说一下 事件绑定说一下
DOM事件流（event flow ）存在三个阶段：事件捕获阶段、处于目标阶段、事件冒泡阶段。
事件捕获（event capturing）： 通俗的理解就是，当鼠标点击或者触发dom事件时，浏览器会从根节点开始由外到内进行事件传播，即点击了子元素，如果父元素通过事件捕获方式注册了对应的事件的话，会先触发父元素绑定的事件。
事件冒泡（dubbed bubbling）： 与事件捕获恰恰相反，事件冒泡顺序是由内到外进行事件传播，直到根节点。无
把div中的a放在平级其中的事件会怎么样 冒泡会到document还是window
window

133.tap是怎么回事、和click的区别。。。
移动端的主要问题是click会有300ms的延迟，主要原因是苹果手机在设计时，考虑到用户在浏览网页时需要放大，所以，在用户点击的300ms之后，才触发click，如果300ms之内还有click，就会进行放大缩小。
如何解决这300ms的延迟？ 在移动端，最容易想到的就是使用touchend来替代click，但是touchend是存在很大的问题的，因为touchend之前可能是touchstart、touchmove，最后才是touchstart，具体情境可能是用户滑动页面时，不小心在一个按钮那里触发了touchend，这样就执行了，但是用户的本意不是如此。 那么该怎么解决呢？
tap事件
为了减少这300ms的延迟，tap事件被很多框架（如zepto）封装，来减少这延迟问题， tap事件不是原生的，所以是封装的，那么具体是如何实现的呢？
主要考虑到下面两点：
按住的事件不能超过延时时间，因为长时间可能就是浏览器的复制、粘贴等操作了。
不能在页面中移动，移动是不能触发tap事件的。
　如果我们在移动端所有的click都替换为了tap事件，还是会触发点透问题的，因为实质是： 在同一个z轴上，z-index不同的两个元素，上面的元素是一个绑定了tap事件的，下面是一个a标签，一旦tap触发，这个元素就会display: none，而从上面的tap可以看出，有touchstart、touchend，所以会300ms之后触发click事件，而z-index已经消失了，所以，触发了下面的a的click事件，注意： 我们认为a标签默认是绑定了click事件的。而这种现象不是我们所期待的。
解决方案： （1）使用fastclick。 （2）添加一个延迟。

134.说一下restful api吧
1.每一个URI代表一种资源；
2.客户端和服务器之间，传递这种资源的某种表现层；
客户端通过四个HTTP动词（get、post、put、delete），对服务器端资源进行操作，实现表现层状态转化。

1. C-S架构
2. 无状态
   3.统一的接口
   4.一致的数据格式
   4.系统分层
   5.可缓存
   6.按需编码、可定制代码
   RESTful的7个最佳实践
3. 版本
   2.参数命名规范
   3.url命名规范
4. 统一返回数据格式
5. http状态码
6. 合理使用query parameter
7. 多表、多参数连接查询如何设计URL

135.css3了解多少，说到了渐进增强和优雅降级
渐进增强（Progressive Enhancement）：一开始就针对低版本浏览器进行构建页面，完成基本的功能，然后再针对高级浏览器进行效果、交互、追加功能达到更好的体验。

优雅降级（Graceful Degradation）：一开始就构建站点的完整功能，然后针对浏览器测试和修复。比如一开始使用 CSS3 的特性构建了一个应用，然后逐步针对各大浏览器进行 hack 使其可以在低版本浏览器上正常浏览。
优雅降级和渐进增强只是看待同种事物的两种观点。优雅降级和渐进增强都关注于同一网站在不同设备里不同浏览器下的表现程度。关键的区别则在于它们各自关注于何处，以及这种关注如何影响工作的流程。
优雅降级观点认为应该针对那些最高级、最完善的浏览器来设计网站。而将那些被认为“过时”或有功能缺失的浏览器下的测试工作安排在开发周期的最后阶段，并把测试对象限定为主流浏览器（如 IE、Mozilla 等）的前一个版本。在这种设计范例下，旧版的浏览器被认为仅能提供“简陋却无妨 (poor, but passable)” 的浏览体验。你可以做一些小的调整来适应某个特定的浏览器。但由于它们并非我们所关注的焦点，因此除了修复较大的错误之外，其它的差异将被直接忽略。
渐进增强观点则认为应关注于内容本身。请注意其中的差别：我甚至连“浏览器”三个字都没提。内容是我们建立网站的诱因。有的网站展示它，有的则收集它，有的寻求，有的操作，还有的网站甚至会包含以上的种种，但相同点是它们全都涉及到内容。这使得渐进增强成为一种更为合理的设计范例。

136.cookie和localStorage区别，如何把cookie写在一个对象中，其属性就是键值对
因此sessionStorage 和 localStorage 的主要区别在于他们存储数据的生命周期，sessionStorage 存储的数据的生命周期是一个会话，而 localStorage 存储的数据的生命周期是永久，直到被主动删除，否则数据永远不会过期的。

137.深拷贝和浅拷贝的区别，手写深拷贝
https://juejin.im/post/6844903929705136141#heading-5

138.兼容性说一说，你做的PC端兼容性是到哪的
https://www.jianshu.com/p/c8d0294f793d

139.说一说CSS3中的动画，animation中可以取哪些值
animation
下面各属性的简写（除了 animation-play-state）
animation-name
指定 @keyframes 动画的名称
animation-duration
动画完成一个周期的时间，默认为 0s
animation-timing-function
动画运行的'节奏'，默认是 ease
animation-delay
动画开始播放的延迟时间，默认是 0
animation-iteration-count
动画播放的次数，默认是 1
animation-direction
规定动画是否在下一个周期逆向播放
animation-fill-mode
规定动画的填充模式
animation-play-state
控制动画的运行或暂停，默认是 running

140.flex布局是什么，默认的方向是什么，如何改变方向
Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。
容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。
项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。
flex-direction属性决定主轴的方向（即项目的排列方向）。

141.说一说原型和原型链，object是最上面的吗？
null
142.你的node项目中有几个线程，有几个进程，如果node进程崩溃了，如何知道
你可以监听由process触发的unhandledRejection事件：

143.node中的process是什么
process是Node.js提供的一个对象，它代表当前Node.js进程

144.node中如何创建子进程
Node.js 的 child_process spawn() exec() fork()

145.100万个数据中，找出前10大数据，最快的方法是什么，堆排序怎么写
首先读入前10000个数来创建大小为10000的最小堆，建堆的时间复杂度为O（mlogm）（m为数组的大小即为10000），然后遍历后续的数字，并于堆顶（最小）数字进行比较。如果比最小的数小，则继续读取后续数字；如果比堆顶数字大，则替换堆顶元素并重新调整堆为最小堆。整个过程直至1亿个数全部遍历完为止。然后按照中序遍历的方式输出当前堆中的所有10000个数字。该算法的时间复杂度为O（nmlogm），空间复杂度是10000（常数）。

146.登录状态怎么使用cookie保持，最好的方法是什么
https://blog.csdn.net/weixin_40693643/article/details/104806130
cookie和session配合使用
首先，用户登录输入用户名和密码，浏览器发送post请求，服务器后台获取用户信息，查询数据库验证用户信息是否正确。如果验证通过，就会创建session来存储相关信息，并且生成一个cookie字符串，把sessionID放在cookie里面。然后返回给浏览器。
当用户下一次发起请求时，浏览器会自动携带cookie去请求服务器，服务器识别后，通过里面的sessionID，就可以直接读取session中的用户信息。这样，用户就可以直接访问，不需要再输入用户名和密码来验证身份
为什么不全部使用Cookies作为用户登陆信息的保存值？
初步考虑到Cookies值有大小的限制，有些属性也不应该作为Cookies存放到客户端，这里最好对Cookies进行一个加密的操作，保证数据的安全。
Session为何即使设计了20分钟，但往往会马上就过期了？
初步估计是因为Session是根据服务器的信息来的，是存放在服务器端的内存中的，当服务器端内存一吃紧在做释放工作之后，用户信息当然会丢失了。

147.页面中一个video，可能格式不支持，那么前端如何判断并给出提示？
<video src="https://chimee.org/vod/1.mp4" controls>
您的浏览器不支持Video标签。
</video>
通过比较img的onerror是一种方法， 通过服务器端也可以保存一些属性来标识哪些浏览器支持，哪些不支持

148.前端的表单中如何设置表单的方式，如multipart, www等，对于multipart具体是如何区分其中的不同的格式的
Content-Type: image/png

149.post、put、delete和get的区别什么
GET请求会向数据库发索取数据的请求，从而来获取信息，该请求就像数据库的select操作一样，只是用来查询一下数据，不会修改、增加数据，不会影响资源的内容，即该请求不会产生副作用。无论进行多少次操作，结果都是一样的。
与GET不同的是，PUT请求是向服务器端发送数据的，从而改变信息，该请求就像数据库的update操作一样，用来修改数据的内容，但是不会增加数据的种类等，也就是说无论进行多少次PUT操作，其结果并没有不同。
POST请求同PUT请求类似，都是向服务器端发送数据的，但是该请求会改变数据的种类等资源，就像数据库的insert操作一样，会创建新的内容。几乎目前所有的提交操作都是用POST请求的。
DELETE请求顾名思义，就是用来删除某一个资源的，该请求就像数据库的delete操作。

150.页面加载速度很慢，如何加速页面的渲染
我们需要快速的完成DOM+CSSOM - Layout - Paint - Composite Layers的整个过程。一切会阻塞DOM生成，阻塞CSSOM生成的动作都应该尽可能消除，或者延迟。
分割CSS
使用GPU加速
异步JavaScript
强制同步布局/回流

1) H5静态资源模板包预加载
   WebView初始化慢，初始化好一个全局的WebView待用
   服务端数据处理慢，可以让服务端进行分块输出，在后端进行数据处理时，让前端也可以有网络静态资源加载。
   JS脚本执行慢，让脚本最后运行，不要阻塞HTML页面解析。可以采用“基础库”+“页面代码”的方式把框架拆分出来。
   WebView初始化慢，可以通过客服端预先或者同时先请求数据，让后端和网络不要闲着等待WebView初始化。
   合理的预加载和缓存，增量更新可以让页面展示速度提升得更多。
   DNS和Connection慢，尽量复用客户端使用的域名和链接。

151.图片很大，如何进行优化
要执行图片压缩
调整图片大小并进行裁剪
从 PNG 转换成一个对这个图片而言更有效的文件格式，如 JPEG。
压缩图片，减少文件大小，而又不降低图片的视觉品质
一种更进一步的优化方法是将图片转换成最为优化的 WebP 文件格式
懒加载技术：先只加载可视窗口区域的图片，当用户向下拖动滚动条时再继续加载后面的图片（也是只加载目前可视窗口区域内的图片）
压缩，预加载，缓存，图床。
先加载低质量的图片，让浏览者可以看到，然后再在后台加载更高清的，等加载完了，浏览者还在观看，就插入替换掉。
prefetch 预读取
预读取就是很常见的资源预加载，当前页面加载完成之后，就会在后面偷偷的下载你指定的资源，一般是 JS 、CSS 和 图片 这类的，也可以下载页面：
prerender 预渲染
这个更厉害了，不仅偷偷的提前下载，而且还给你渲染出来，当用户点击链接的时候，立刻给你展现出来。
字体图库代替图标

152.手写二分查找
/**

* 无序的二分查找。返回true/false
* @param target
* @param arr
* @returns {boolean}
  */
  function binarySearch(target,arr) {
  while (arr.length>0){
  //使用快速排序。以mid为中心划分大小，左边小，右边大。
  var left    = [];
  var right   = [];
  //选择第一个元素作为基准元素(基准元素可以为任意一个元素)
  var pivot   = arr[0];
  //由于取了第一个元素，所以从第二个元素开始循环
  for(var i=1;i<arr.length;i++){
  var item = arr[i];
  //大于基准的放右边，小于基准的放左边
  item>pivot ? right.push(item) : left.push(item);
  }

  ```````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````
   //得到经过排序的新数组
   if(target==pivot){
       return true;
   }else if(target>pivot){
       arr     = right;
   }else{
       arr     = left;
   }
  ```````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````


  }
  return false;
  }

153.画出TCP三次握手的过程
第一次握手：建立连接时，客户端A发送SYN包（SYN=j）到服务器B，并进入SYN_SEND状态，等待服务器B确认。
第二次握手：服务器B收到SYN包，必须确认客户A的SYN（ACK=j+1），同时自己也发送一个SYN包（SYN=k），即SYN+ACK包，此时服务器B进入SYN_RECV状态。
第三次握手：客户端A收到服务器B的SYN＋ACK包，向服务器B发送确认包ACK（ACK=k+1），此包发送完毕，客户端A和服务器B进入ESTABLISHED状态，完成三次握手。

154.websocket是如何实现服务器端推送的
TCP是持久连接，建立TCP连接是3次握手，关闭TCP连接是4次挥手。TCP连接是由通信双方（应用层）来决定什么时候关闭，其本身是一个持久连接。TCP连接可以进行全双工通信，因为双方都知道对方是谁
HTTP只能单向通信、无状态
Http协议只能单向通信的原因是：Serve没有保存Http客户端的信息（无状态的），想要通信的时候找不到人。而Http1.1协议新增的keep-alive Header之后，Server会保存连接，即长连接。虽然Comet等基于长链接的轮询技术，实现了全双工通信；但是每次都是http请求，一堆没用的信息（http head），浪费资源。而且本质没有变化，都需要客户端请求才能获得数据，增加了keep-alive请求头只是可以通过一条通道请求多次。
而HTTP的 request - response 模式，纯粹是人为规定的，并不存在技术上的问题。说白了就是，HTTP规定，服务器只能响应请求，而不能主动发送数据。
当客户端要和服务端建立 WebSocket 连接时，在客户端和服务器的握手过程中，客户端首先会向服务端发送一个 HTTP 请求，包含一个 Upgrade 请求头来告知服务端客户端想要建立一个 WebSocket 连接。这样客户端和服务端建立了全双工通信，通过 WebSocket 的 send 方法和 onmessage 事件就可以通过这条通信连接交换信息。

155.websocket和http的区别是什么？websocket的优点是什么？
a). 相对于Http这种非持久的协议来说，WebSocket是一种持久化的协议。
b). 服务器与客户端之间交换的标头信息很小，大概只有2字节；
c). 客户端与服务器都可以主动传送数据给对方；
d). 不用频繁创建TCP请求及销毁请求，减少网络带宽资源的占用，同时也节省服务器资源；
举例：
（1）Http的生命周期通过Request来界定，也就是Request一个Response，那么在Http1.0协议中，这次Http请求就结束了。
在Http1.1中进行了改进，是的有一个Keep-alive，也就是说，在一个Http连接中，可以发送多个Request，接收多个Response。
但是必须记住，在Http中一个Request只能对应有一个Response，而且这个Response是被动的，不能主动发起。（相反， websocket是可以的）
（2）WebSocket是基于Http协议的，或者说借用了Http协议来完成一部分握手，在握手阶段与Http是相同的。
Websocket解决了HTTP的这几个难题。首先，被动性，当服务器完成协议升级后（HTTP->Websocket），服务端就可以主动推送信息给客户端啦。解决了上面同步有延迟的问题。
解决服务器上消耗资源的问题：其实我们所用的程序是要经过两层代理的，即HTTP协议在Nginx等服务器的解析下，然后再传送给相应的Handler（php等）来处理。简单地说，我们有一个非常快速的 接线员（Nginx） ，他负责把问题转交给相应的 客服（Handler） 。Websocket就解决了这样一个难题，建立后，可以直接跟接线员建立持久连接，有信息的时候客服想办法通知接线员，然后接线员在统一转交给客户。
由于Websocket只需要一次HTTP握手，所以说整个通讯过程是建立在一次连接状态中，也就避免了HTTP的非状态性，服务端会一直知道你的信息，直到你关闭请求，这样就解决了接线员要反复解析HTTP协议，还要查看identity info的信息。
目前唯一的问题是：不兼容低版本的IE，WebSocket是HTML5中的协议，支持持久连接；而Http协议不支持持久连接。

156.为什么使用websocket？ websocket是怎么连接的，一定需要通过http协议吗？
WebSocket协议实现全双工通信、以及持久连接的一个前提是，它是基于TCP的。
WebSocket协议也需要通过已建立的TCP连接来传输数据。具体实现上是通过http协议建立通道，然后在此基础上用真正的WebSocket协议进行通信。
WebSocket 本质上跟 HTTP 完全不一样，只不过为了兼容性，WebSocket 的握手是以 HTTP 的形式发起的，
. 即时聊天通信
· 多玩家游戏
· 在线协同编辑/编辑
· 实时数据流的拉取与推送
· 体育/游戏实况
· 实时地图位置

157.短轮询、commet、长轮询都介绍一下。各有什么优缺点。
Ajax轮询
实践简单，利用XHR，通过setInterval定时发送请求，但会造成数据同步不及时及无效的请求，增加后端处理压力。
Ajax长轮询
在Ajax轮询基础上做的一些改进，在没有更新的时候不再返回空响应，而且把连接保持到有更新的时候，客户端向服务器发送Ajax请求，服务器接到请求后hold住连接，直到有新消息才返回响应信息并关闭连接，客户端处理完响应信息后再向服务器发送新的请求，通常把这种实现也叫做comet。
SSE是HTML5新增的功能，全称为Server-Sent Events。它可以允许服务推送数据到客户端。SSE在本质上就与之前的长轮询、短轮询不同，虽然都是基于http协议的，但是轮询需要客户端先发送请求。而SSE最大的特点就是不需要客户端发送请求，可以实现只要服务器端数据有更新，就可以马上发送到客户端。

158.http1.1中的keep-alive是怎么理解的？
http keep-alive与tcp keep-alive，不是同一回事，意图不一样。http keep-alive是为了让tcp活得更久一点，以便在同一个连接上传送多个http，提高socket的效率。而tcp keep-alive是TCP的一种检测TCP连接状况的机制。
vue keep-live 组件缓存

159.setTimeout和setInterval
这两个函数的区别就在于，setInterval在执行完一次代码之后，经过了那个固定的时间间隔，它还会自动重复执行代码，而setTimeout只执行一次那段代码。

160.了解node吗？node在表现上和浏览器有什么不一样的地方
全局环境下this的指向
node中this指向global，
浏览器中this指向window。
V8.Node增加了Buffer类，
不会在node中操作DOM
在nodejs中提供了CMD的模块加载的API，node还提供了npm种种包管理工具，能更有效管理引用的库
I/O读写

161.说一下vue的生命周期，vue的特点，data和props的区别？
beforeCreate  实例初始化之后，this指向创建的实例，不能访问到data、computed、watch、methods上的方法和数据,常用于初始化非响应式变量
created 实例创建完成，可访问data、computed、watch、methods上的方法和数据，未挂载到DOM，不能访问到$el属性，$ref属性内容为空数组,常用于简单的ajax请求，页面的初始化
beforeMount 在挂载开始之前被调用，beforeMount之前，会找到对应的template，并编译成render函数,虚拟Dom已经创建完成，即将开始渲染
mounted 实例挂载到DOM上，此时可以通过DOM API获取到DOM节点，$ref属性可以访问  常用于获取VNode信息和操作，ajax请求
beforeupdate  响应式数据更新时调用，发生在虚拟DOM打补丁之前  适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器
updated 虚拟 DOM 重新渲染和打补丁之后调用，组件DOM已经更新，可执行依赖于DOM的操作  避免在这个钩子函数中操作数据，可能陷入死循环
beforeDestroy 实例销毁之前调用。这一步，实例仍然完全可用，this仍能获取到实例 常用于销毁定时器、解绑全局事件、销毁插件对象等操作
destroyed 实例销毁后调用，调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁

162.data和props的区别
data是每个组件的私有内存，可以在其中存储需要的任何变量。props是将数据从父组件传递到子组件的方式。
子组件中的data数据，不是通过父组件传递的是子组件私有的，是可读可写的。
子组件中的所有 props中的数据，都是通过父组件传递给子组件的，是只读的。

163.js中的事件委托事件代理？
事件委托就是利用事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。
浏览器的事件流分为事件捕获，处于目标阶段和事件冒泡，事件捕获阶段，事件由父级传递进子级，事件冒泡阶段由子级传递向父级，通过对父级事件的监听，就可以做到同一父级的子级事件的监听。
focus、blur之类的事件本身没有事件冒泡机制，所以无法委托；
mousemove、mouseout这样的事件，虽然有事件冒泡，但是只能不断通过位置去计算定位，对性能消耗高，因此也是不适合事件委托的
层级过多，冒泡过程中，可能会被某层阻止掉（建议就近委托）

164.浏览器渲染css的规则是什么？
css的渲染规则，是从上到下，从右到左渲染的。

165.如何去操作一个dom节点，更高效的方法？
使用innerHTML在大多数浏览器中会给你带来更快的执行效率
https://zhuanlan.zhihu.com/p/86153264

1. 预存储UL下的所有LI元素
2. 获取需显示的LI列表
3. 删除整个UL元素，这里用的是UL父元素.removeChild(UL)，千万不要用innerHTML=”,火狐里会挂的很惨。
4. 重建UL元素
5. 逐个添加步骤2获得的LI元素到UL
6. 添加UL到UL父元素。

166.css为什么放在前面，js为什么放在后面？css能不能放在后面，如果放在后面会发生什么情况
影响html的渲染。
浏览器解析到script的时候，就会立即下载执行，中断html的解析过程
浏览器一边下载HTML网页，一边开始解析。
解析过程中，发现script标签
暂停解析，网页渲染的控制权转交给JavaScript引擎
如果script标签引用了外部脚本，就下载该脚本，否则就直接执行
执行完毕，控制权交还渲染引擎，恢复往下解析HTML网页
其实js的执行是依赖css样式的。即只有css样式全部下载完成后才会执行js。如果有外链css，那么js的执行时需要等待外链css下载完

167.vue 生命周期有哪些？
beforeCreate（初始化界面前）
created（初始化界面后）
beforeMount（渲染dom前）
mounted（渲染dom后）
beforeUpdate（更新数据前）
updated（更新数据后）
beforeDestroy（卸载组件前）
destroyed（卸载组件后）

168.vue 响应式数据是如何实现的？
是基于 Object.defineProperty 去实现的；
但是有一个要求，就是在 实例化 Vue 构造函数前，所有要进行双向绑定的数据，都是要在 data 里面去做初始化的，哪怕是一个空值；
因为在每次实例化的时候，Vue 会去检测 data ，查看并把存在属性用 Object.defineProperty 进行监听；
在每一个需要判断或者展示当前响应式监听的数据时，初始化的时候，我绑定了一个 name 属性，它在一个 div 里面做了展示；
当我在 div 里面添加 name 展示的时候，其实在模板编译的时候，获取了一下 name 属性；
因为前面有提到，我给当前的属性绑定了 Object.defineProperty ，所以在获取的时候，我会调用到 get 方法；
在这之前，我有实例化一个 dep 队列，把每次获取 name 属性的地方，做一个 push ；
当我接下来要做数据修改的时候，比如把 zhangsan 变成了 lisi ，那么在 set 方法里，把我当前属性的队列有监听当前属性的位置，全部更新一遍；
最后把 data 对象里面的属性值做修改；
注：这是一个面试的回答，但这个不够详细，如果想很详细的去了解，具体都做了什么，请跳转：Vue 源码解析（实例化前） - 响应式数据的实现原理

169.一个 url 从输入按下回车键，到页面展示出来，都经历了什么？
首先，在输入网址按下回车以后，这个时候DNS服务器会通过当前的网址去解析网址的 ip；
在查找到真的 IP 以后，这个时候浏览器会向 web 服务器发起一个 tcp 连接请求（三次握手）：
第一次：建立链接时，客户端发送 syn 包到服务器，并进入SYN_SENT（传输控制协议）状态，等待服务器确认；
第二次：服务器收到 syn 包，必须确认客户的 syn ，同时自己也发送一个 ack 包，即 syn + ack 包，此时服务器进入 SYN_RECV （服务端被动打开后，接收到了客户端的 syn 并且发送了 ack 时的状态） 状态；
第三次：客户端收到了服务器的 syn + ack 包，向服务器发送确认包 ack ，此包发送完毕，客户端和服务器进入 ESTABLISHED （tcp 连接成功） 状态，完成三次握手；
当三次握手结束后，客户端和服务器就建立好了连接，这时候 tcp 协议断开；
开始访问当前服务器下默认的 index.html ，并调用该访问的资源文件，展示对应的内容。

170.你是怎么理解面向对象的，什么是面向对象，用面向对象做过什么
面向对象有三大特性：封装 、 继承 、多态 ，其实还有一个特性，就是 抽象 ，只不过我们平时很少说这一点；
面向对象有两个概念：类 和 实例；
面向对象的优势：可扩展，易维护，高内聚，低耦合；
面向对象的劣势：由于它的特点导致了它的整体体积会很大，哪怕拆分成了不同的类，这一点和 函数式编程 相比，是它的劣势，因为函数式编程有一个特点就是，每个函数只做一件事情；
在项目当中，用面向对象做的东西很多，印象最深刻的有两个，一个是 即时通讯 的功能封装，还有一个就是 音频视频播放 的统一处理，即时通讯涉及的功能比较多，也比较繁琐，我拿 音频视频播放 这个功能点来举例：
由于我们做的产品，会涉及到很多的音频和视频，但是音频和视频文件是不允许同时播放的，每次只能播放一个；
比如我现在播放了一个 音频a，当我接下来点击播放 音频b，那么这个时候 音频a 是要暂停的，而不是清空状态，下次点击是重新播放；
而且如果我再播放状态当中，一直去进行更新我的 data 对象当中获取到的数据，对 js引擎线程 也是会有影响的，比如我这个时候也在做别的事情，还得在事件流 event loop 中进行排队，这不是我们想要的；
所以我用了一个类去做这件事情，设计模式用的 单例模式 ；
在首次进入页面的时候，只加载文件，不去实例化该对象；
当我在发现有对媒体文件（音频视频）进行操作是，在去创建或者获取，写法如下：
class Media {
}
Media.instance = undefined;
Media.getInstance = function(){
if(!Media.instance){
Media.instance = new Media()
}
return Media.instance;
}
这是一个简单的单例模式的实现；
那么接下来如果我们要对媒体文件进行如上的操作，那么需要有一个单独的属性：
class Media {
constructor(){
this._onlyMedia = {
id:'',
url:'',
name:'',
curTime:'',
duration:'',
...
}
}
}
这样就知道了大概的一个意思，比如我要每次点击媒体文件的时候，要去对比是不是同一个，因为有不同的id；
具体的实现步骤我就不写了，大概的意思表述清楚就好

171. 刚才你提到了事件流（event loop），能简单的说一下，什么是事件流吗？
     我们都知道，js是单线程的，虽然现在有 worker 的存在，但是也只是可以进行运算，并不能操作 dom；
     js最一开始执行的线程，是主线程，然后主线程执行完毕后，是微队列 microtask 的循环执行，微队列执行完毕后，在执行宏队列 macrotask；
     宏队列的方法：setTimeout 、setInterval 、setImmediate 、I/O 、UI rendering
     微队列的方法：promise.then、process.nextTick 、Object.observe(已废弃)
     正好之前写过这方面的文章，想详细的理解这一块的知识，跳转：性能优化篇 - js事件循环机制（event loop）

172.说一说 xss 攻击
其实就是一种利用脚本把代码植入到提供给其他用户使用的页面中，一般就是利用表单提交；
如果我们自己去写这种攻击防御的方式，最简单的就是利用表单提交的内容，去做处理，不允许提交 script 标签内容等；
平时在实际项目当中，提交请求一般会用 axios，它对 xss 攻击已经做了拦截，我们不需要去在特意的做这方面的处理；

173.说一下SHA1算法 加密算法
https://juejin.im/post/6855462790413910023

174.给你一个数组，手写实现一个求和最大的连续子串，优化？
int maxsequence(int arr[], int len)
{
int max = arr[0]; //初始化最大值为第一个元素
for (int i=0; i<len; i++) {
int sum = 0; //sum必须清零
for (int j=i; j<len; j++) { //从位置i开始计算从i开始的最大连续子序列和的大小，如果大于max，则更新max。
sum += arr[j];
if (sum > max)
max = sum;
}
}
return max;
}
//动态规划算法
int MaxSum(int n)
{
int sum=0,b=0;
for(int i=0;i<n;i++)
{
if(b>0) b+=a[i];
else b=a[i];
if(b>sum) sum=b;
}
return sum;
}

175.求最长公共子序列？优化
/**

* @param {string} text1
* @param {string} text2
* @return {number}
  */
  var longestCommonSubsequence = function(text1, text2) {
  let n = text1.length;
  let m = text2.length;
  let dp = Array.from(new Array(n+1),() => new Array(m+1).fill(0));
  for(let i = 1;i <= n;i++){
  for(let j = 1;j <= m;j++){
  if(text1[i-1] == text2[j-1]){
  dp[i][j] = dp[i-1][j-1] + 1;
  }else{
  dp[i][j] = Math.max(dp[i][j-1],dp[i-1][j]);
  }
  }
  }
  return dp[n][m];
  };

176.原型、原型链，ES6是怎么实现继承的
class Point {
constructor(x) {
this.x = 1;
this.p = 2;
}
print() {
return this.x;
}
}
Point.prototype.z = '4'
class ColorPoint extends Point {
constructor(x) {
this.color = color; // ReferenceError
super(x, y);
this.x = x; // 正确
}
m() {
super.print();
}
}
ES6继承是通过class丶extends 关键字来实现继承 Point是父类，ColorPoint 是子类 通过 class 新建子类 extends继承父类的方式实现继承，方式比ES5简单的多。

177.基本的自适应布局有哪些？手写几个三栏自适应

<div class="outer outer2">
   <div class="left">2-left</div>
   <div class="middle">2-middle</div>
   <div class="right">2-right</div>
</div>

.outer2 {
display: flex;
}
.outer2 .left {
flex: 0 0 100px;
}
.outer2 .middle {
flex: auto;
}
.outer2 .right {
flex: 0 0 200px;
}

.outer3 .left{
float: left;
width: 100px;
}
.outer3 .right {
float: right;
width: 200px;
}
.outer3 .middle {
margin: 0 200px 0 100px;
}
178.防抖 - debounce 当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，如果设定的时间到来之前，又一次触发了事件，就重新开始延时。
const debounce = (fn, delay) => {
let timer = null;
return (...args) => {
clearTimeout(timer);
timer = setTimeout(() => {
fn.apply(this, args);
}, delay);
};
};

节流 - throttle 当持续触发事件时，保证一定时间段内只调用一次事件处理函数
const throttle = (fn, delay = 500) => {
let flag = true;
return (...args) => {
if (!flag) return;
flag = false;
setTimeout(() => {
fn.apply(this, args);
flag = true;
}, delay);
};
};
jsonp
function jsonp({url, params, cb}) {
return new Promise((resolve, reject) => {
window[cb] = function (data) {  // 声明全局变量
resolve(data)
document.body.removeChild(script)
}
params = {...params, cb}
let arrs = []
for(let key in params) {
arrs.push(`${key}=${params[key]}`)
}
let script = document.createElement('script')
script.src = `${url}?${arrs.join('&')}`
document.body.appendChild(script)
})
}
//手写apply
Function.prototype.apply = function(content = window) {
content.fn = this;
let result;
// 判断是否有第二个参数
if(arguments[1]) {
result = content.fn(...arguments[1]);
} else {
result = content.fn();
}
delete content.fn;
return result;
}
//手写bind
Function.prototype.bind = function(content) {
if(typeof this != 'function') {
throw Error('not a function');
}
let _this = this;
let args = [...arguments].slice(1);
return function F() {
// 判断是否被当做构造函数使用
if(this instanceof F) {
return _this.apply(this, args.concat([...arguments]))
}
return _this.apply(content, args.concat([...arguments]))
}
}
//手写call
Function.prototype.call2 = function(content = window) {
// 判断是否是underfine和null
// if(typeof content === 'undefined' || typeof content === null){
//     content = window
// }
content.fn = this;
let args = [...arguments].slice(1);
let result = content.fn(...args);
delete content.fn;
return result;
}
实现一个new操作符的具体实现步骤实现new：
/**

* 创建一个new操作符
* @param {*} Con 构造函数
* @param  {...any} args 构造函数中传的参数
  */
  function createNew(Con, ...args) {
  let obj = {} // 创建一个对象，因为new操作符会返回一个对象
  Object.setPrototypeOf(obj, Con.prototype) // 将对象与构造函数原型链接起来
  // obj.__proto__ = Con.prototype // 等价于上面的写法
  let result = Con.apply(obj, args) // 将构造函数中的this指向这个对象，并传递参数
  return result instanceof Object ? result : obj
  }
  手写instanceof
  instanceof 用来检测一个对象在其原型链中是否存在一个构造函数的 prototype 属性
  function instanceOf(left,right) {
  let proto = left.__proto__;
  let prototype = right.prototype
  while(true) {
  if(proto === null) return false
  if(proto === prototype) return true
  proto = proto.__proto__;
  }
  }

177.绑定多个点击事件的优化
（事件委托）

178.浏览器不同内核之间的区别？如何渲染dom树？css树呢？css选择的原理？以及原因？
https://juejin.im/post/6844903983052488717#heading-10
构建CSSOM规则树
浏览器解析CSS文件并生成CSS规则树，每个CSS文件都被分析成一个StyleSheet对象，每个对象都包含CSS规则。CSS规则对象包含对应于CSS语法的选择器和声明对象以及其他对象。
它们都是将每个 CSS 文件解析为样式表对象，每个对象包含 CSS 规则，CSS 规则对象包含选择器和声明对象，以及其他一些符合 CSS 语法的对象，
Webkit 使用了词法分析和语法分析这部分代码是自动生成的，而 Webkit 中实现的 CallBack 函数就是在 CSSParser 中。
通过调用 CSSStyleSheet 的 parseString 函数，将上述 CSS 解析过程启动，解析完一遍后，把 Rule 都存储在对应的 CSSStyleSheet 对象中；
由于目前规则依然是不易于处理的，还需要将之转换成 CSSRuleSet。也就是将所有的纯样式规则存储在对应的集合当中，这种集合的抽象就是 CSSRuleSet；
3.CSSRuleSet 提供了一个 addRulesFromSheet 方法，能将 CSSStyleSheet 中的 rule 转换为 CSSRuleSet 中的 rule ；
4.基于这些个 CSSRuleSet 来决定每个页面中的元素的样式；
可能很多同学都知道排版引擎解析 CSS 选择器时是从右往左解析，这是为什么呢？
1.HTML 经过解析生成 DOM Tree（这个我们比较熟悉）；而在 CSS 解析完毕后，需要将解析的结果与 DOM Tree 的内容一起进行分析建立一棵 Render Tree，最终用来进行绘图。Render Tree 中的元素（WebKit 中称为「renderers」，Firefox 下为「frames」）与 DOM 元素相对应，但非一一对应：一个 DOM 元素可能会对应多个 renderer，如文本折行后，不同的「行」会成为 render tree 种不同的 renderer。也有的 DOM 元素被 Render Tree 完全无视，比如 display:none 的元素。
2.在建立 Render Tree 时（WebKit 中的「Attachment」过程），浏览器就要为每个 DOM Tree 中的元素根据 CSS 的解析结果（Style Rules）来确定生成怎样的 renderer。对于每个 DOM 元素，必须在所有 Style Rules 中找到符合的 selector 并将对应的规则进行合并。选择器的「解析」实际是在这里执行的，在遍历 DOM Tree 时，从 Style Rules 中去寻找对应的 selector。
3.因为所有样式规则可能数量很大，而且绝大多数不会匹配到当前的 DOM 元素（因为数量很大所以一般会建立规则索引树），所以有一个快速的方法来判断「这个 selector 不匹配当前元素」就是极其重要的。
4.如果正向解析，例如「div div p em」，我们首先就要检查当前元素到 html 的整条路径，找到最上层的 div，再往下找，如果遇到不匹配就必须回到最上层那个 div，往下再去匹配选择器中的第一个 div，回溯若干次才能确定匹配与否，效率很低。

179.jpg和png的区别？
JPG在图片压缩方面有巨大优势，但采用有损压缩，图片质量有损失。一般截屏用PNG格式不但比JPG质量高 且 文件还更小；
防锯齿PNG非常有优势。

不使用defineProperty，还可以使用什么实现双向绑定 手写双向绑定
https://juejin.im/post/6844903601416978439#heading-12
const input = document.getElementById('input');
const p = document.getElementById('p');
const obj = {};

const newObj = new Proxy(obj, {
get: function(target, key, receiver) {
console.log(`getting ${key}!`);
return Reflect.get(target, key, receiver);
},
set: function(target, key, value, receiver) {
console.log(target, key, value, receiver);
if (key === 'text') {
input.value = value;
p.innerHTML = value;
}
return Reflect.set(target, key, value, receiver);
},
});

input.addEventListener('keyup', function(e) {
newObj.text = e.target.value;
});

180.XSS和CSRF概念和如何防范,refer不靠谱，验证码用户体验差，除了隐藏token外还有什么实现

181.DFS和BFS
深度优先搜索的步骤分为 1.递归下去 2.回溯上来。顾名思义，深度优先，则是以深度为准则，先一条路走到底，直到达到目标。这里称之为递归下去。否则既没有达到目标又无路可走了，那么则退回到上一步的状态，走其他路。这便是回溯上来
广度优先搜索（BFS）
广度优先搜索在进一步遍历图中顶点之前，先访问当前顶点的所有邻接结点。
深度优先搜索在搜索过程中访问某个顶点后，需要递归地访问此顶点的所有未访问过的相邻顶点。

182.CORS和preflight
不会触发CORS预检的请求称为简单请求，满足以下所有条件的才会被视为简单请求
简单请求
使用GET、POST、HEAD其中一种方法
只使用了如下的安全首部字段，不得人为设置其他首部字段
Accept
Accept-Language
Content-Language
Content-Type 仅限以下三种
text/plain
multipart/form-data
application/x-www-form-urlencoded
HTML头部header field字段：DPR、Download、Save-Data、Viewport-Width、WIdth
请求中的任意XMLHttpRequestUpload 对象均没有注册任何事件监听器；XMLHttpRequestUpload 对象可以使用 XMLHttpRequest.upload 属性访问
请求中没有使用 ReadableStream 对象
preflight
需预检的请求要求必须首先使用 OPTIONS 方法发起一个预检请求到服务器，以获知服务器是否允许该实际请求。"预检请求“的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响
下面的请求会触发预检请求，其实非简单请求之外的就会触发预检，就不用记那么多了
使用了PUT、DELETE、CONNECT、OPTIONS、TRACE、PATCH方法
人为设置了非规定内的其他首部字段，参考上面简单请求的安全字段集合，还要特别注意Content-Type的类型XMLHttpRequestUpload 对象注册了任何事件监听器请求中使用了ReadableStream对象

183.Promise
Promise为我们解决了什么问题?
在传统的异步编程中，如果异步之间存在依赖关系，我们就需要通过层层嵌套回调来满足这种依赖，如果嵌套层数过多，可读性和可维护性都变得很差，产生所谓“回调地狱”，而Promise将回调嵌套改为链式调用，增加可读性和可维护性。
Promise的调用流程：
Promise的构造方法接收一个executor()
在new Promise()时就立刻执行这个executor回调
executor()内部的异步任务被放入宏/微任务队列，等待执行
then()被执行，收集成功/失败回调，放入成功/失败队列
executor()的异步任务被执行，触发resolve/reject，从成功/失败队列中取出回调依次执行
这是个「观察者模式」，这种 收集依赖 -> 触发通知 -> 取出依赖执行 的方式，被广泛运用于观察者模式的实现，在Promise里，执行顺序是then收集依赖 -> 异步触发resolve -> resolve执行依赖。

184.事件循环(输出顺序) event loop
JS引擎在执行JS脚本的时候是单线程的，脚本会顺序放入执行栈中依次解析并执行，如果遇到setTimeout这种延时任务，会交给Web APIs线程处理，web api 等到延时结束后把回调函数推进宏任务队列，遇到promise这种异步任务时，会把它推进微任务队列。宏微任务也有其执行规则，每次执行完一个宏任务，就要把微任务队列中的所有微任务取出来执行，微任务执行完毕，开始检查渲染。这就是JavaScript的异步执行机制，也叫事件循环。
每个宏任务之后，引擎会立即执行微任务队列中的所有任务，然后再执行其他的宏任务，或渲染，或进行其他任何操作。
宏任务包括：
I/O
setTimeout / setInterval / setImemediate
requestAnimationFrame
微任务包括：
promise / .then /.catch /.finally
mutationObserver
process.nextTick

185.this是动态变化的，谁调用它，就指向哪里，this有四种绑定规则：
默认绑定（严格/非严格模式）严格是undefined；非严格独立函数调用时this指向window。
隐式绑定，当函数引用有上下文对象时，隐式绑定会把函数中的this绑定到这个上下文对象。
显示绑定，通过call()或者apply()方法，第一个参数是一个对象。
new绑定，如果有new关键字，this指向new出来的那个对象。

186.什么是闭包?
简单讲，闭包就是指有权访问另一个函数作用域中的变量的函数。
如何产生一个闭包？
创建闭包最常见方式，就是在一个函数内部创建另一个函数并返回。
闭包的作用域链包含着它自己的作用域，以及包含它的函数的作用域和全局作用域。
闭包的注意事项：
通常，函数的作用域及其所有变量都会在函数执行结束后被销毁。但是，在创建了一个闭包以后，这个函数的作用域就会一直保存到闭包不存在为止。
闭包的应用：
设计私有的方法和变量。
函数柯里化
187. 对 async/await 的理解
首先，async函数返回的是⼀个Promise对象；
然后，async 函数内部 return 返回的值。会成为 then 方法回调函数的参数; 如果 async 函数内部抛出异常，则会导致返回的 Promise 对象状态变为 reject 状态。抛出的错误而会被 catch 方法回调函数接收到。
async 函数返回的 Promise 对象，必须等到内部所有的 await 命令的 Promise 对象执行完，才会发生状态改变.
正常情况下，await 命令后面跟着的是 Promise ，如果不是的话，也会被转换成一个 立即 resolve 的 Promise.

188. flex
     首先，采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。
     其次，容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。
     最后，项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。
     容器属性，指定容器内元素的排列、对齐方式
     flex-direction 主轴方向
     flex-wrap 单行，多行
     flex-flow direction+wrap
     justify-content 对齐方式，中间空多少

align-items 交叉轴的定义
align-content 多根轴线的对齐方式
order 项目排列顺序
flex-grow放大比例默认0
flex-shrink 缩小比例默认1
flex-basis 项目本身占多大
flex(该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto))
align-self

189. 对缓存的了解
     分为强缓存和协商缓存：
190. 浏览器在加载资源时，先根据这个资源的一些http header(expries和cache-control)判断它是否命中强缓存，强缓存如果命中，浏览器直接从自己的缓存中读取资源，不会发请求到服务器。比如某个css文件，如果浏览器在加载它所在的网页时，这个css文件的缓存配置命中了强缓存，浏览器就直接从缓存中加载这个css，连请求都不会发送到网页所在服务器；
     no-cache
     在发布缓存副本之前，强制要求缓存把请求提交给原始服务器进行验证(协商缓存验证)。
     no-store
     缓存不应存储有关客户端请求或服务器响应的任何内容，即不使用任何缓存。
191. 当强缓存没有命中的时候，浏览器一定会发送一个请求到服务器，通过服务器端依据资源的另外一些http header(last-modified和etag)验证这个资源是否命中协商缓存，如果协商缓存命中，服务器会将这个请求返回，但是不会返回这个资源的数据，而是告诉客户端可以直接从缓存中加载这个资源，于是浏览器就又会从自己的缓存中去加载这个资源；
192. 强缓存与协商缓存的共同点是：如果命中，都是从客户端缓存中加载资源，而不是从服务器加载资源数据；区别是：强缓存不发请求到服务器，协商缓存会发请求到服务器。
193. 当协商缓存也没有命中的时候，浏览器直接从服务器加载资源数据。

190.浏览器

1. 浏览器本地存储
   浏览器的本地存储主要分为Cookie、localStorage、sessionStorage 和 IndexedDB。
   cookie
   Cookie 本质上就是浏览器里面存储的一个很小的文本文件，向同一个域名下发送请求，都会携带相同的 Cookie，服务器拿到 Cookie 进行解析，便能拿到客户端的状态。
   cookie 就是用来做状态存储的,但是也有缺陷：容量缺陷，性能缺陷，安全缺陷
   localStorage
   localStorage有一点跟Cookie一样，在同一个域名下，会存储相同的一段localStorage。
   相对Cookie的区别：容量。localStorage 的容量上限为5M，只存在客户端，默认不参与与服务端的通信，接口封装，有get set 函数
   应用场景：
   利用localStorage的较大容量和持久特性，可以利用localStorage存储一些内容稳定的资源，比如官网的logo，存储Base64格式的图片资源，因此利用localStorage
   sessionStorage
   与localStorage基本相同。
   但sessionStorage和localStorage有一个本质的区别，那就是前者只是会话级别的存储，并不是持久化存储。会话结束，也就是页面关闭，这部分sessionStorage就不复存在了。
   应用场景：
   可以用它对表单信息进行维护，将表单信息存储在里面，可以保证页面即使刷新也不会让之前的表单信息丢失。
   可以用它存储本次浏览记录。如果关闭页面后不需要这些记录，用sessionStorage就再合适不过了。事实上微博就采取了这样的存储方式。
   indexedDB
   IndexedDB是运行在浏览器中的非关系型数据库, 本质上是数据库，理论上容量是没有上限的。
   重要特性：
   支持事务，存储二进制数据，键值对存储，异步操作，受同源策略限制，即无法访问跨域的数据库
2. 对前端路由的了解
   何为前端路由？
   简单的说，就是在保证只有一个 HTML 页面，且与用户交互时不刷新和跳转页面的同时，为 SPA 中的每个视图展示形式匹配一个特殊的 url。在刷新、前进、后退和SEO时均通过这个特殊的 url 来实现。
   为实现这一目标，我们需要做到以下 2 点：
   改变 url 且不让浏览器向服务器发送请求。
   可以监听到 url 的变化
   hash 模式
   这里的 hash 就是指 url 后的 # 号以及后面的字符。比如说 "www.baidu.com/#hashhash" ，其中 "#hashhash" 就是我们期望的 hash 值。
   由于 hash 值的变化不会导致浏览器向服务器发送请求，而且 hash 的改变会触发 hashchange 事件，浏览器的前进后退也能对其进行控制，所以在 H5 的 history 模式出现之前，基本都是使用 hash 模式来实现前端路由。
   history 模式
   在 HTML5 之前，浏览器就已经有了 history 对象。但在早期的 history 中只能用于多页面的跳转：
   history.go(-1);       // 后退一页
   history.go(2);        // 前进两页
   history.forward();     // 前进一页
   history.back();      // 后退一页
   在 HTML5 的规范中，history 新增了以下几个 API：
   history.pushState();         // 添加新的状态到历史状态栈
   history.replaceState();      // 用新的状态代替当前状态
   history.state                // 返回当前状态对象
3. 前端安全（xss、csrf）
   xss 攻击
   分为 DOM xss、反射型xss、存储型 xss
   恶意攻击者往Web页面里插入恶意Script代码，当用户浏览该页之时，其他用户看到这个包括恶意脚本的页面并执行，获取用户的cookie等敏感信息，从而达到恶意攻击用户的目的。
   预防措施：防止下发界面显示html标签，把</>等符号转义 。
   csrf 攻击
   其实就是一个钓鱼网站，要理解为什么会收到攻击，应该采取什么策略进行防御。
   CSRF通过伪装来自受信任用户的请求来利用受信任的网站。用户C首先登录受信任网站A，并在本地生成Cookie。在不登出A的情况下，访问危险网站B。B要求访问站点A，浏览器会自动带上用户C的cookie，并且A站点也分辨不清好坏，B就可以冒充受信任用户C来进行操作。
   预防措施：请求中加入随机数，让钓鱼网站无法正常伪造请求。

193.堆排序
堆排序（Heapsort）是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。堆排序可以说是一种利用堆的概念来排序的选择排序。分为两种方法：
大顶堆：每个节点的值都大于或等于其子节点的值，在堆排序算法中用于升序排列；
小顶堆：每个节点的值都小于或等于其子节点的值，在堆排序算法中用于降序排列；

194.rsa
第一步：生成密钥对，即公钥和私钥。
1：随机找两个质数 P 和 Q ,P 与 Q 越大，越安全。
2：计算 n 的欧拉函数 φ(n)。
3：随机选择一个整数 e，条件是1< e < m，且 e 与 m 互质。
4：有一个整数 d，可以使得 e*d 除以 m 的余数为 1。
将n和e封装成公钥，n和d封装成私钥。
第二步：加密生成密文 。
解密生成明文。

195.css保持宽高比
height: auto
iframe加载完后，将高度变为指定的px
https://www.it-swarm.dev/zh/html/%E4%BD%BFiframe%E9%80%82%E5%90%88%E5%AE%B9%E5%99%A8%E5%89%A9%E4%BD%99%E9%AB%98%E5%BA%A6%E7%9A%84100%EF%BC%85/958073557/

196.二叉树翻转180度
/**

* Definition for a binary tree node.
* function TreeNode(val) {
* ````````````````
  this.val = val;
  ````````````````
* ```````````````````````````````
  this.left = this.right = null;
  ```````````````````````````````
* }*/
  /**
* @param {TreeNode} root
* @return {TreeNode}
  */
  var invertTree = function(root) {
  if(!root) return null
  if(root) {
  var left = root.left
  root.left = root.right
  root.right = left
  }
  invertTree(root.left)
  invertTree(root.right)
  return root
  };

197.闭包的内部实现
闭包是一个可以从另一个函数的作用域访问变量的函数。这是通过在函数内创建函数来实现的。 当然，外部函数无法访问内部范围。
规则1:闭包以外层函数为单位，只有被内部函数(不管嵌套多深)使用的变量才会纳入闭包里面。
规则2:内部函数持有的闭包，不仅仅包含内部函数的所需的变量：
规则3:内存函数的上下文链，只会关联存在闭包的函数上下文：
规则4:内部函数没用引用外层函数变量，但如果其他内部函数引用了外层变量。这个内部函数依旧持有上下文中的闭包
规则5:不同内部函数持有相同的外部函数闭包,也就是闭包中的变量对所有内部函数都是共享，不管这些函数何时执行，同步或者异步
规则6:文法上下文才可以访问
规则7:值对象里的函数无法绑定闭包
规则8:常规函数闭包无法绑定this,匿名函数只能通过闭包绑定this
规则9:闭包是编译已知，平行函数之间的闭包是隔离的，就算这个这两个方法运行在同一个stack上的上下两层。

198.DOM是什么？有哪些操作
DOM概念本身很简单，请先完全跟着我的思路来：普通文档（*.txt）和HTML/XML文档（*.html/*.xml）的区别仅仅是因为后者是有组织的结构化文件；浏览器将结构化的文档以树的数据结构读入浏览器内存，并将每个树的子节点定义为一个NODE（想象这颗树，从根节点到叶子节点都被建模为一个NODE对象）；这每个节点（NODE）都有自己的属性（名称、类型、内容...）；NODE之间有层级关系（parents、child、sibling...）；以上已经完成文档的建模工作（将文档内容以树形结构写入内存），此时再编写一些方法来操作节点（属性和位置信息），即为NODE API。
抽象一下：DOM是一种将HTML/XML文档组织成对象模型的建模过程；DOM建模重点在于如何解析HTML/XML文档和开放符合DOM接口规范的节点操作API接口。再抽象一下：解析文档，建模成对象模型，开放API接口。最后：DOM：Document Object Model 文档对象模型

199.==和===区别== 允许类型转换，=== 不允许类型转换
typeof null === Object , 以及undefined和null区别
undefined表示一个变量没有被声明，或者被声明了但没有被赋值（未初始化），一个没有传入实参的形参变量的值为undefined，如果一个函数什么都不返回，则该函数默认返回undefined。null则表示“什么都没有”，即“空值”。
Javascript将未赋值的变量默认值设为undefined；Javascript从来不会将变量设为null。它是用来让程序员表明某个用var声明的变量时没有值的；
undefined不是一个有效的JSON，而null是；
null 和 undefined 的值相等，但类型不等：undefined的类型(typeof)是undefined；null的类型(typeof)是object。
null和undefined之间的主要区别在于它们被转换为原始类型的方式。

200.判断数组的方法，哪个更好？
Array.isArray(arr) 是 Object.prototype.toString.call(arg) === '[object Array]';的语法糖。instanceof 跨frame时不能共享原型链。
删除已有的html节点(考察removeChild)
var child=document.getElementById("p1");
child.parentNode.removeChild(child);
字符串反转const reverseString = str => str.split("").reverse().join("")
跨域和AJAX(大概6-7种)CORS、JSONP、Fetch、postMessage、Node、Webpack proxyTable、...
:before和::before的区别(伪类、伪元素)
单冒号(:)用于CSS3伪类，双冒号(::)用于CSS3伪元素。
伪类nth-child和nth-of-type区别

201.requestAnimationFrame和setTimeout
requestAnimationFrame是浏览器用于定时循环操作的一个接口，类似于setTimeout，主要用途是按帧对网页进行重绘。

202.IP伪造
1.REMOTE_ADDR是远端IP，默认来自tcp连接客户端的Ip。可以说，它最准确，确定是，只会得到直接连服务器客户端IP。如果对方通过代理服务器上网，就发现。获取到的是代理服务器IP了。
如：a→b(proxy)→c ,如果c 通过REMOTE_ADDR ，只能获取到b的IP,获取不到a的IP了。
这个值是无法修改的。
2.HTTP_X_FORWARDED_FOR，HTTP_CLIENT_IP 为了能在大型网络中，获取到最原始用户IP，或者代理IP地址。对HTTp协议进行扩展。定义了实体头。
HTTP_X_FORWARDED_FOR = clientip,proxy1,proxy2其中的值通过一个 逗号+空格 把多个IP地址区分开, 最左边(client1)是最原始客户端的IP地址, 代理服务器每成功收到一个请求，就把请求来源IP地址添加到右边。
HTTP_CLIENT_IP 在高级匿名代理中，这个代表了代理服务器IP。
其实这些变量，来自http请求的：X-Forwarded-For字段，以及client-ip字段。 正常代理服务器，当然会按rfc规范来传入这些值。
但是，攻击者也可以直接构造该x-forword-for值来“伪造源IP”,并且可以传入任意格式IP.
这样结果会带来2大问题，其一，如果你设置某个页面，做IP限制。 对方可以容易修改IP不断请求该页面。 其二，这类数据你如果直接使用，将带来SQL注册，跨站攻击等漏洞。

203.JS事件模型
属于W3C标准模型，现代浏览器(除IE6-8之外的浏览器)都支持该模型。在该事件模型中，一次事件共有三个过程:
事件捕获阶段(capturing phase)。事件从document一直向下传播到目标元素, 依次检查经过的节点是否绑定了事件监听函数，如果有则执行。
事件处理阶段(target phase)。事件到达目标元素, 触发目标元素的监听函数。
事件冒泡阶段(bubbling phase)。事件从目标元素冒泡到document, 依次检查经过的节点是否绑定了事件监听函数，如果有则执行。

204.nodejs内存泄露
事件监听
全局变量
打印内存快照
对比内存快照找出泄漏位置
gulp和webpack的区别？(模块与流，CommonChunks抽出公共模块)
gulp严格上讲，模块化不是他强调的东西，他旨在规范前端开发流程。
webpack更是明显强调模块化开发，而那些文件压缩合并、预处理等功能，不过是他附带的功能。

205.组件化思想
组件就是将一段UI样式和其对应的功能作为独立的整体去看待，无论这个整体放在哪里去使用，它都具有一样的功能和样式，从而实现复用，这种整体化的细想就是组件化

206.node创建子进程，进程间的通信 进程通信
而Node有4种创建进程的方式：spawn()，exec()，execFile()和fork()
通过stdin/stdout传递json：最直接的方式，适用于能够拿到“子”进程handle的场景，适用于关联进程之间通信，无法跨机器
Node原生IPC支持：最native（地道？）的方式，比上一种“正规”一些，具有同样的局限性
通过sockets：最通用的方式，有良好的跨环境能力，但存在网络的性能损耗
借助message queue：最强大的方式，既然要通信，场景还复杂，不妨扩展出一层消息中间件，漂亮地解决各种通信问题

207.负载均衡
负载均衡是高可用架构的一个关键组件，主要用来提高性能和可用性，通过负载均衡将流量分发到多个服务器，同时多服务器能够消除这部分的单点故障。

208.JavaScript 如何得到HTTP的请求头信息和返回的头信息
Javascript中跟response header有关的就两个方法：
getResponseHeader 从响应信息中获取指定的http头 语法
strValue = oXMLHttpRequest.getResponseHeader(bstrHeader);
getAllResponseHeaders 获取响应的所有http头 语法
strValue = oXMLHttpRequest.getAllResponseHeaders();
需要注意的是，通常，在IE下不能完整的获取header报头数据，只能取到如下header数据：
X-Powered-By:
X-UA-Compatible:
Keep-Alive:
Transfer-Encoding:
Content-Type:
比如你要获取时间戳，在IE下必须做些特殊处理，需要在后端设置一下，关闭缓存：
header( 'Cache-Control: no-store');
var req = new XMLHttpRequest();
req.open('GET', document.location, false);
req.send(null);
var header = req.getAllResponseHeaders().toLowerCase();
console.log(Headers);

209.什么是前端路由
接收前端请求url
比如前端的地址栏是 www.xxx.com/show;
这个地址没准请求的是 www.xxx.com/aaa
那么对于后端来说接收的是 www.xxx.com/aaa 而不是 www.xxx.com/show
无论前端还是后端，我们看到的URL都是马甲，需要通过路由触发/执行真实需要执行的逻辑路径
你看到的不一定是真实的。也为以后的业务变更提供了回旋的余地
应用场景在哪里
前端路由应用场景就是所谓的单页应用。
在业务允许浏览器允许的情况下使用前端路由可以让页面体验较好。但是在例如很多业务情景下就不适用了;
例如展示广告，几乎不需要在页面上有其他逻辑，例如严谨的下单流程，后端路由可以严格控制前端不可进入页面;
还有后端路由可以应用于API层面提供接口等等许多的场景都是可以的。
灵活选择前后端路由会让你的业务体验相当不错，或者更深层次的你用到了同构，前后端共用一套路由，在直接由回车进入页面时将这套路由在服务端渲染输出，但是页面点击跳转等动作时又是前端路由…

210.下拉刷新怎么实现?
下拉–>提示松开刷新–>松开后–>开始刷新–>刷新成功后还原
js部分：主要就是为一个节点绑定事件，可以是整个body，按照实际来看
目前主要涉及三个事件， touchstart touchmove touchend
这里获取touch点坐标是用 pageX , pageY 当然不兼容的话先不考虑
因为是下滑才刷新，所以稍微控制一下way，其实也就是通过这个控制是获取 pageX 还是 pageY
滑一滑可以直接看到dist的变化，其实就把它看做px了吧

211.异步加载和延迟加载
异步加载的方案: 动态插入script标签
通过ajax去获取js代码，然后通过eval执行
script标签上添加defer或者async属性
创建并插入iframe，让它异步执行js
延迟加载：有些 js 代码并不是页面初始化的时候就立刻需要的，而稍后的某些情况才需要的。

212.线程与进程的区别
javaScript单线程执行机制
首先解释下，单线程和多线程。
什么是单线程？单线程就是一个进程中只有一个线程。程序顺序执行，前面的执行完，才会执行后面的程序。
什么是多线程？多线程就是一个进程中只有多个线程。在进程内部进行线程间的切换，由于每个线程执行的时间片很短，所以在感觉上是并行的。

进程和线程的主要差别在于它们是不同的操作系统资源管理方式。进程有独立的地址空间，一个进程崩溃后，在保护模式下不会对其它进程产生影响，而线程只是一个进程中的不同执行路径。线程有自己的堆栈和局部变量，但线程之间没有单独的地址空间，一个线程死掉就等于整个进程死掉，所以多进程的程序要比多线程的程序健壮，但在进程切换时，耗费资源较大，效率要差一些。但对于一些要求同时进行并且又要共享某些变量的并发操作，只能用线程，不能用进程。

线程的划分尺度小于进程，使得多线程程序的并发性高。
另外，进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率。
线程在执行过程中与进程还是有区别的。每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。
从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看做多个独立的应用，来实现进程的调度和管理以及资源分配。这就是进程和线程的重要区别。

进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动,进程是系统进行资源分配和调度的一个独立单位.
线程是进程的一个实体,是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位.线程自己基本上不拥有系统资源,只拥有一点在运行中必不可少的资源(如程序计数器,一组寄存器和栈),但是它可与同属一个进程的其他的线程共享进程所拥有的全部资源.
一个线程可以创建和撤销另一个线程;同一个进程中的多个线程之间可以并发执行.

线程和进程基本概念
进程：操作系统分配的占有CPU资源的最小单位。拥有独立的地址空间。
线程：安排CPU执行的最小单位。同一个进程下的所有线程，共享进程的地址空间。
简单讲，计算机就像工厂，进程是个大车间，计算机内部有很多个这样的大车间。线程是工人，每一个车间里的工人至少有一个。

线程和进程的关系、通性
关系：进程中包含着至少一个线程。在进程创建之初，就会包含一个线程，这个线程会根据需要，调用系统库函数去创建其他线程。但需要注意的是，这些线程之间是没有层级关系的，他们之间协同完成工作。在整个进程完成工作之后，其中的线程会被销毁，释放资源。
通性：都包含三个状态，就绪、阻塞、运行。通俗的讲，阻塞就是资源未到位，等待资源中。就绪，就是资源到位了，但是CPU未到位，还在运行其他。
既然，线程和进程是存在通性的，那么为什么操作系统还要设置线程这个单位，那就说说线程的几点好处：

1. 在一个程序中，多个线程可以同步或者互斥并行完成工作，简化了编程模型；
2. 线程较进程来讲，更轻；
3. 线程虽然微观并行。但是，在一个进程内部，一个线程阻塞后，会执行这个进程内部的其他线程，而不是整体阻塞。从某种意义上，提高了CPU的利用率。

一个程序至少有一个进程,一个进程至少有一个线程.
线程的划分尺度小于进程，使得多线程程序的并发性高。
另外，进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率。
线程在执行过程中与进程还是有区别的。每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。
从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看做多个独立的应用，来实现进程的调度和管理以及资源分配。这就是进程和线程的重要区别。

为什么感觉上javaScript是多线程？而且还支持AJAX异步呢？AJAX是真正的异步吗？
先说明，从哪里可以得出javaScript是单线程。
比如你页面一上来就alert（“hello world~”）；只要你不关闭这个对话框，后续的js代码就不会再执行。
因为，单线程就是这样一步一步的顺次执行，前面不执行完，后面不会执行。也就是说，在具体的某一时刻，只有一段代码在执行。
可是，JavaScript明明可以处理各种触发事件，感觉上是异步多线程啊。其实，它的原理是这样的，JavaScript单线程的执行浏览器的一个事件队列，要执行的函数和触发事件的回调函数都被放在这个队列中。
比如，我点击率一下按钮，之后又将浏览器缩小了，那么这两个事件的回调函数就会顺次地被放在当前执行的“函数”之后，再一一执行。
那么，既然JavaScript是单线程，那么如何维护这个函数队列呢，他分身无术啊。这时候，就需要知道，浏览器可不是单线程。虽然，每一个window只有一个js引擎，但是浏览器是事件驱动的、异步的、多线程的。

浏览器内部有一个事件轮询（event loop），是一个大的内部消息循环，会轮询大的消息队列，并执行。也就是js要处理的事件队列，是浏览器维护的。
浏览器至少有四个线程（不同浏览器会有差异）： js引擎线程、界面渲染线程、浏览器事件触发线程、http请求线程。
其实，到这里就说的很明白了。但是，又想到了延时函数（setTimeout）的例子，感觉上，因为没有阻塞执行，会感觉是异步，其实并不是。只是，js在执行到延时函数时，会触发浏览器的定时器，到设置时间，浏览器再将这个函数放入执行的函数队列，再由JavaScript引擎执行。都是在浏览器空闲了才会执行。

关于AJAX的异步，是真正的异步。
同样的道理，在调用AJAX的时候，浏览器会开辟一个新的线程，去处理这个请求，得到响应后，如果这个请求有回调，会将这个回调再放入事件队列中。再由JavaScript引擎执行。
关于JavaScript的阻塞
浏览器虽然是多线程，但是由于JavaScript具有阻塞特性，无论外链还是内嵌脚本，在浏览器执行解释js脚本的时候，浏览器是不会去做别的事情的，比如渲染页面，而是直到js下载并执行完毕。
这样，js脚本的下载、解释执行，会反该页面的继续绘制，给用户带来不良的体验。所以，要对其优化，有如下几点：
a、将 `<script>`内嵌和外链，在可以的情况下，放在<body>底部。注：对于css，浏览器是并行下载
b、在页面onload后，加载js
c、html5 `<script>`标签的defer属性，在页面加载完成后下载
d、使用创建 `<script>`标签的方式，在页面加载完成后添加进去。
注：解决阻塞就是一句话，先让页面渲染完，再加载js。

213.怎么重构页面？
编写 CSS、让页面结构更合理化，提升用户体验，实现良好的页面效果和提升性能。
对网站重构的理解
网站重构：在不改变外部行为的前提下，简化结构、添加可读性，而在网站前端保持一致的行为。也就是说是在不改变UI的情况下，对网站进行优化，在扩展的同时保持一致的UI。
使网站前端兼容于现代浏览器(针对于不合规范的CSS、如对IE6有效的)
对于移动平台的优化
针对于SEO进行优化
深层次的网站重构应该考虑的方面
减少代码间的耦合
让代码保持弹性
严格按规范编写代码
设计可扩展的API
代替旧有的框架、语言(如VB)
增强用户体验
通常来说对于速度的优化也包含在重构中
压缩JS、CSS、image等前端资源(通常是由服务器来解决)
程序的性能优化(如数据读写)
采用CDN来加速资源加载
对于JS DOM的优化
HTTP服务器的文件缓存

214.ES6模块机制
ES6 模块跟 CommonJS 模块的不同，主要有以下两个方面：
ES6 模块输出的是值的引用，输出接口动态绑定，而 CommonJS 输出的是值的拷贝
ES6 模块编译时执行，而 CommonJS 模块总是在运行时加载

215.node异步非阻塞 I/O 底层实现原理
从用户体验角度讲，异步IO可以消除UI阻塞，快速响应资源
Node中的异步网络Io就是利用了epoll来实现， 简单来说， 就是利用一个线程来管理众多的IO请求， 通过事件机制实现消息通讯。
avaScript是单线程的，但Node本身其实是多线程的，除了用户代码无法并行执行外，所有的I/O请求是可以并行执行的。
事件循环是Node异步I/O实现的核心，Node通过事件驱动的方式处理请求，使得其无须为每个请求创建额外的线程，省掉了创建和销毁线程的开销。同时也因为线程数较少，不受线程上下文切换的影响，维持了Node的高性能。
Node异步IO、非阻塞的特性，使它非常适用于IO密集、高并发的应用场景

216.XSS 与 CSRF 安全问题，提到了 SSRF 安全
服务端请求伪造（Server Side Request Forgery, SSRF）指的是攻击者在未能取得服务器所有权限时，利用服务器漏洞以服务器的身份发送一条构造好的请求给服务器所在内网。SSRF攻击通常针对外部网络无法直接访问的内部系统。

217.如何实现(2).plus(5) 输出7
Number.prototype.plus = function(n) {
return this.valueOf() + n;
}
Number.prototype.minus = function(n) {
return this.valueOf() - n;
}
console.log( (5).plus(3).minus(6) ) //2

218.函数声明和函数定义的区别
JavaScript 解释器中存在一种变量声明被提升的机制，也就是说函数声明会被提升到作用域的最前面，即使写代码的时候是写在最后面，也还是会被提升至最前面。
而用函数表达式创建的函数是在运行时进行赋值，且要等到表达式赋值完成后才能调用
9.用过iframe吗？iframe一般用来干嘛？
iframe跨域，

219.setTimeout与window.requestAnimationFrame相比有什么区别
setTimeout 与 requestAnimationFrame 的区别：
引擎层面：setTimeout 属于 JS 引擎，存在事件轮询，存在事件队列。
requestAnimationFrame 属于 GUI 引擎，发生在渲染过程的中重绘重排部分，与电脑分辨路保持一致。
性能层面：当页面被隐藏或最小化时，定时器 setTimeout 仍在后台执行动画任务。
当页面处于未激活的状态下，该页面的屏幕刷新任务会被系统暂停，requestAnimationFrame 也会停止。
应用层面：利用 setTimeout，这种定时机制去做动画，模拟固定时间刷新页面。
requestAnimationFrame 由浏览器专门为动画提供的 API，在运行时浏览器会自动优化方法的调用，在特定性环境下可以有效节省了CPU 开销

220.现在有一个下拉加载更多的列表，当加载的次数非常多时，页面的dom结构会变得非常复杂，会不会带来性能问题？怎么优化？
列表随着时间会越来越长，需要控制展示的节点数量。
列表长度随着WebSocket的通信而增加，数据更新频度过快，需要有缓冲池。
增加一个列表锁定的提示，可以手动解开列表的锁定。
移动聚焦的时候会随即展示日志详情，因为移动速度过快，所以需要增加防抖。

221.说说防抖和节流
函数节流是：在固定的时间内触发事件，每隔n秒触发一次
函数防抖是：当你频繁触发后，n秒内只执行一次
debounce
search搜索联想，用户在不断输入值时，用防抖来节约请求资源。
频繁操作点赞和取消点赞，因此需要获取最后一次操作结果并发送给服务器
throttle
鼠标不断点击触发，mousedown(单位时间内只触发一次),mousemove，滚动加载更多，resize
window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次

222.UA是什么？有什么用？（这题没答出来，确实没见过，也不太记得是不是UA了，反正很陌生）
navigator.userAgent

223.情景题：设计一个限时抢购系统，要求并发量10w，服务器只能达到1w，从前到后给一个整体的设计思路
秒杀商品页面静态化和缓存化，减少后端请求，将商品描述、参数、详情，全部写到一个静态页面，不用进行程序的逻辑处理，不需访问数据库,不用部署动态的服务器和数据库服务器；这边从产品层面可以让按钮点击一次之后就变成灰色，避免重复点击；针对API调用，可以设置在规定的时间内，一个IP只能请求一次，其他时间返回缓存。
动态生成随机下单页面的URL，为了避免用户直接访问下单URL,需要将URL动态化，用随机数作为参数，只能秒杀开始的时候才生成。

224.Array.isArray(Array.prototype)的结果？true

225.es6箭头函数和普通函数的区别（箭头函数this指向继承自外围作用域）

226.js引擎怎么实现Class关键字 手写Class
function _defineProperties(target,arr){
for(let i=0;i<arr.length;i++){
Object.defineProperty(target,arr[i].key,{
...arr[i],
configurable : true,
enumerable : true,
writable:true
})
}
}

function _createClass(Constructor, protoProps, staticProps) {
if (protoProps) _defineProperties(Constructor.prototype, protoProps);
if (staticProps) _defineProperties(Constructor, staticProps);
return Constructor;
}
手写extend
function _inherits(subClass, superClass) {
// 确保superClass为function
if (typeof superClass !== "function" && superClass !== null) {
throw new TypeError("Super expression must either be null or a function, not " + typeof superClass);
}
// subClass.prototype的[[prototype]]关联到superClass superClass.prototype
// 给subClass添加constructor这个属性
subClass.prototype = Object.create(superClass && superClass.prototype, {
constructor: {
value: subClass,
enumerable: false,
writable: true,
configurable: true
}
});
// 设置subclass的内置[[prototype]]与superClass相关联
if (superClass) Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass;
}

手写super
function _possibleConstructorReturn(self, call) {
if (!self) {
throw new ReferenceError("this hasn't been initialised - super() hasn't been called");
}
//显示绑定b的内置[[prototype]]到this，即在b中执行b原型链上关联的属性。
return call && (typeof call === "object" || typeof call === "function") ? call : self;
}

227.js实现map函数手写map
Array.prototype.selfMap = function () {
const ary = this
const result = new Array(ary.length);
const [ fn, thisArg ] = [].slice.call(arguments)
if (typeof fn !== 'function') {
throw new TypeError(fn + 'is not a function')
}
for (let i = 0; i < ary.length; i++) {
// fix稀疏数组的情况
if (i in ary) {
result[i] = fn.call(thisArg, ary[i], i, ary);
}
}
return result
}

手写filter
Array.prototype.selfFilter = function () {
const ary = this
const result = []
const [ fn , thisArg ] = [].slice.call(arguments)

````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````
if (typeof fn !== 'function') {
    throw new TypeError(fn + 'is not a function')
}
const result = [];
for (let i = 0; i < ary.length; i++) {
    if (i in ary && fn.call(thisArg, ary[i], i, ary)) {
        result.push(ary[i])
    }
}
return result
````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````

}

手写reduce
Array.prototype.selfReduce = function(fn, initialValue) {
var arr = Array.prototype.slice.call(this);
var res, startIndex;
res = initialValue ? initialValue : arr[0];
startIndex = initialValue ? 0 : 1;
for(var i = startIndex; i < arr.length; i++) {
res = fn.call(null, res, arr[i], i, this);
}
return res;
}

手写forEach
Array.prototype.selfForeach = function(callback, context) {
// 不能是null调用方法
if (this === null) {
throw new TypeError(
"Array.prototype.reduce" + "called on null or undefined"
);
}
// 第一个参数必须要为function
if (typeof callback !== "function") {
throw new TypeError(callback + " is not a function");
}
// 获取数组
let arr = Array.prototype.slice.call(this);
let _len = arr.length;
for (let i = 0; i < _len; i++) {
callback.call(context, arr[i], i, arr);
}
return arr;
};

228.eval是做什么用的，有什么安全问题

1. eval只是一个普通的函数，只不过他有一个快速通道通向编译器，可以将string变成可执行的代码。有类似功能的还有Function ,setInterval   和 setTimeout。
2. eval不容易调试。用chromeDev等调试工具无法打断点调试，所以麻烦的东西也是不推荐使用的…
3. 说到性能问题，在旧的浏览器中如果你使用了eval，性能会下降10倍。在现代浏览器中有两种编译模式：fast path和slow path。fast path是编译那些稳定和可预测（stable and predictable）的代码。而明显的，eval不可预测，所以将会使用slow path ，所以会慢。还有一个是，在使用类似于Closure Compiler等压缩（混淆）代码时，使用eval会报错。（又慢又报错，我还推荐吗？）
4. 关于安全性，我们经常听到eval是魔鬼，他会引起XSS攻击，实际上，如果我们对信息源有足够的把握时，eval并不会引起很大的安全问题。而且不光是eval，其他方式也可能引起安全问题。比如：   莫名其妙给你注入一个<script src="">标签，或者一段来历不明的JSONP请求，再或者就是Ajax请求中的eval代码…   所以啊，只要你的信息源不安全，你的代码就不安全。不单单是因为eval引起的。你用eval的时候会在意XSS的问题，你越在意就越出问题，出的多了，eval就成噩梦了。
5. 效率问题是程序逻辑问题。对于一些有执行字符串代码需求的程序中，不用eval而用其他方式模拟反而会带来更大的开销。

229.v-model是用来做什么的
v-model是用来双向绑定，绑定input，textarea，select等元素上的数据，但是其本质仅仅是语法糖。是监听用户的输入事件用来更新数据， v-model 默认会利用名为 value 的 prop 和名为 input 的事件实现，但是对于不同的表单元素 value 属性会用于不同的目的

230.实现一个compose(arr)({index:0}}) var arr=[fn1,fn2,fn3]; function fn1(index,next){a.index++;next()}... 返回value和next,next()表示调用下一个函数
https://segmentfault.com/a/1190000008394749
var compose = function(...args) {
var len = args.length
var count = len - 1
var result
return function f1(...args1) {
result = args[count].apply(this, args1)
if (count <= 0) {
count = len - 1
return result
} else {
count--
return f1.call(null, result)
}
}
}

231.为什么存在跨域这个问题？为什么要有同源策略？同源策略是什么？如果没有会有什么问题？
同源策略是为了避免向第三方网站发送 post 请求、向第三方网站请求可能会造成信息泄露
CSRF 是为了防止非自己网站的请求向服务器请求数据

232.讲讲闭包？闭包存在的问题
http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html

233.new关键字和不new有什么区别？
第一种是构造函数式，即通过new运算符调用构造函数Function来创建函数
第二种不是实例化，只是调用函数把返回值赋给变量。
这句中，new做了以下几件事情。

1. 创建一个新的对象，这个对象的类型是object；
2. 查找class的prototype上的所有方法、属性，复制一份给创建的Object
3. 将构造函数classA内部的this指向创建的Object
4. 创建的Object的__proto__指向class的prototype
5. 执行构造函数class
6. 返回新创建的对象给变量b

234.断点续传
利用 Blob.prototype.slice 方法，和数组的 slice 方法相似，调用的 slice 方法可以返回原文件的某个切片
这样我们就可以根据预先设置好的切片最大数量将文件切分为一个个切片，然后借助 http 的可并发性，同时上传多个切片，这样从原本传一个大文件，变成了同时传多个小的文件切片，可以大大减少上传时间
另外由于是并发，传输到服务端的顺序可能会发生变化，所以我们还需要给每个切片记录顺序
前端使用 localStorage 记录已上传的切片 hash
服务端保存已上传的切片 hash，前端每次上传前向服务端获取已上传的切片
一个算法题，怎么找出连续子数组的最大和（如果和为负数，则重新开始，如果和为正数，则继续加，然后比较大小，选出最大和即可。）

11、怎么实现拖拽？
HTML 的 drag & drop 使用了 DOM event model 以及从mouse events 继承而来的 drag events 。一个典型的drag操作是这样开始的：用户用鼠标选中一个可拖动的（draggable）元素，移动鼠标到一个可放置的（droppable）元素，然后释放鼠标。 在操作期间，会触发一些事件类型，有一些事件类型可能会被多次触发（比如drag 和 dragover 事件类型）

说说你对组件化思想的理解
组件化是一种编程思想，是一种拆分代码的方式
组件拆分最重要的是有统一的拆分方式和拆分原则。基础组件、业务组件、区块组件和页面组件。
渐进式和离散式。
渐进式：数据逐层传递，组件各自负责自己的业务逻辑
离散式：数据统一处理，集中处理业务逻辑

谈谈你对js单线程的理解
javascript是单线程的，所有任务都需要排队，这些任务分为同步任务和异步任务，单线程上有一个主线程任务。同步任务必须再主线程上排队进行，而异步任务（类似于点击事件）必须在主线程上的任务全部进行完成后形成一个任务队列（将所有的触发事件放在一个任务队列中），这任务队列的任务也是需要排队的，当主线程任务完成后他们将通过触发事件按顺序加入到主线程进行任务。可以改变程序正常执行顺序的操作就可以看成是异步操作。
ajax 异步解释：ajax异步就是当任务队列存在ajax请求时，当任务走到ajax时，ajax传递参数给服务器，而不需要等数据传到后再执行下面的任务，而是让请求数据的时间继续进行下一个任务，从而表现为异步。其中浏览器为ajax新开一个线程请求，当数据请求到后将回调函数（success()）继续放入任务队列中。

进程与线程
进程是操作系统分配资源和调度任务的基本单位,线程是建立在进程上的一次程序运行单位，一个进程上可以有多个线程。

JavaScript是单线程，指的是主线程是单线程。H5中的Web Worker标准虽然是多线程，但Web Worker和JS主线程不是平级的，主线程可以控制Web Worker，并且Web Worker不能操作DOM，不能访问document、window、parent。

JavaScript中其它线程：
浏览器事件触发线程(用来控制事件循环,存放setTimeout、浏览器事件、ajax的回调函数)
定时触发器线程(setTimeout定时器所在线程)
异步HTTP请求线程(ajax请求线程)
同步异步
同步和异步关注的是消息通知机制
同步就是发出调用后，没有得到结果之前，该调用不返回，一旦调用返回，就得到返回值了。 简而言之就是调用者主动等待这个调用的结果。
而异步则相反，调用者在发出调用后这个调用就直接返回了，所以没有返回结果。换句话说当一个异步过程调用发出后，调用者不会立刻得到结果，而是调用发出后，被调用者通过状态、通知或回调函数处理这个调用。

vue里有一个nexttick了解过吗，它是做什么用的
VUE为了更好的性能，不是每修改一次DOM就更新一次DOM。而是异步更新DOM。
所以，VUE的渲染时机是在每个任务队列执行完成之间。就是每次的任务队列中的修改数据或者修改DOM的操作的宏任务或者微任务中。
任务队列1执行完成-> 更新DOM -> 任务队列2执行完成 -> 更新DOM....
所有同步任务都在主线程上执行，形成一个执行栈。
主线程之外，会存在一个任务队列，只要异步任务有了结果，就在任务队列中放置一个事件。
当执行栈中的所有同步任务执行完后，就会读取任务队列。那些对应的异步任务，会结束等待状态，进入执行栈。
主线程不断重复第三步
这里主线程的执行过程就是一个tick，而所有的异步结果都是通过任务队列来调度。
Event Loop 分为宏任务和微任务，无论是执行宏任务还是微任务，完成后都会进入到一下tick，并在两个tick之间进行UI渲染。
由于Vue DOM更新是异步执行的，即修改数据时，视图不会立即更新，而是会监听数据变化，并缓存在同一事件循环中，等同一数据循环中的所有数据变化完成之后，再统一进行视图更新。为了确保得到更新后的DOM，所以设置了 Vue.nextTick()方法。
使用Vue.nextTick()是为了可以获取更新后的DOM 。 触发时机：在同一事件循环中的数据变化后，DOM完成更新，立即执行Vue.nextTick()的回调。
原因：Vue异步执行DOM更新，只要观察到数据变化，Vue将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变，如果同一个watcher被多次触发，只会被推入到队列中一次。

vue数据响应式机制是如何实现
Vue 的响应式，核心机制是 观察者模式。
其实在 Vue 中初始化渲染时，视图上绑定的数据就会实例化一个 Watcher，依赖收集就是是通过属性的 getter 函数完成的，其中 Observer 与 Dep 是一对一的关系， Dep 与 Watcher 是多对多的关系，Dep 则是 Observer 和 Watcher 之间的纽带。依赖收集完成后，当属性变化会执行被 Observer 对象的 dep.notify() 方法，这个方法会遍历订阅者（Watcher）列表向其发送消息， Watcher 会执行 run 方法去更新视图，
数据是被观察的一方，发生改变时，通知所有的观察者，这样观察者可以做出响应，
比如，重新渲染然后更新视图。现在我们重点分析下initData，这里主要做了两件事，一是将_data上面的数据代理到vm上，二是通过执行 observe(data, true / asRootData /)将所有data变成可观察的，即对data定义的每个属性进行getter/setter操作，这里就是Vue实现响应式的基础；
首先将Observer实例绑定到data的ob属性上面去，防止重复绑定；
若data为数组，先实现对应的变异方法（这里变异方法是指Vue重写了数组的7种原生方法，这里不做赘述，后续再说明），再将数组的每个成员进行observe，使之成响应式数据；
否则执行walk()方法，遍历data所有的数据，进行getter/setter绑定，这里的核心方法就是 defineReative(obj, keys[i], obj[keys[i]])
其中getter方法：
先为每个data声明一个 Dep 实例对象，被用于getter时执行dep.depend()进行收集相关的依赖;
根据Dep.target来判断是否收集依赖，还是普通取值。Dep.target是在什么时候，如何收集的后面再说明，先简单了解它的作用，
依赖收集简单点理解就是收集只在实际页面中用到的data数据，然后打上标记，这里就是标记为Dep.target。

在setter方法中:
获取新的值并且进行observe，保证数据响应式；
通过dep对象通知所有观察者去更新数据，从而达到响应式效果。
在Observer类中，我们可以看到在getter时，dep会收集相关依赖，即收集依赖的watcher，然后在setter操作时候通过dep去通知watcher,此时watcher就执行变化，我们用一张图描述这三者之间的关系：
Watcher是一个观察者对象。依赖收集以后Watcher对象会被保存在Dep的subs中，数据变动的时候Dep会通知Watcher实例，然后由Watcher实例回调cb进行视图的更新。
被Observer的data在触发 getter 时，Dep 就会收集依赖的 Watcher ，其实 Dep 就像刚才说的是一个书店，可以接受多个订阅者的订阅，当有新书时即在data变动时，就会通过 Dep 给 Watcher 发通知进行更新。
vue中的data为什么要设计成一个函数
这样每一个实例的data属性都是独立的，不会相互影响了,Object是引用数据类型,如果不用function 返回,每个组件的data 都是内存的同一个地址,一个数据改变了其他也改变了;

代码题：用es5的方式实现es6中的const
手写const
var __const = function __const(data, value) {
window.data = value // 把要定义的data挂载到window下，并赋值value
Object.defineProperty(window, data, { // 利用Object.defineProperty的能力劫持当前对象，并修改其属性描述符
enumerable: false,
configurable: false,
get: function () {
return value
},
set: function (data) {
if (data !== value) { // 当要对当前属性进行赋值时，则抛出错误！
throw new TypeError('Assignment to constant variable.')
} else {
return value
}
}
})
}

8. axios了解吗？jsonp原理是什么
   axios是一个基于Promise的http请求库
   从浏览器中创建 XMLHttpRequests
   从 node.js 创建 http 请求
   支持 Promise API
   拦截请求和响应 （就是有interceptor）
   转换请求数据和响应数据
   取消请求
   自动转换 JSON 数据
   客户端支持防御 XSRF
9. setTimeout是怎么实现的?放入到队列中吗？代码执行到这块是放入，还是时间到了才放入。
   时间到了才放入
10. 讲讲http和https
    我提到做过web负载均衡的改造，让我具体说一下实践和操作过程；

垃圾回收
在 V8 中会把堆分为新生代和老生代两个区域，新生代中存放的是生存时间短的对象，老生代中存放的生存时间久的对象。
新生区通常只支持 1～8M 的容量，而老生区支持的容量就大很多了。对于这两块区域，V8 分别使用两个不同的垃圾回收器，以便更高效地实施垃圾回收。
副垃圾回收器，主要负责新生代的垃圾回收。
主垃圾回收器，主要负责老生代的垃圾回收。
#垃圾回收器的工作流程
现在你知道了 V8 把堆分成两个区域——新生代和老生代，并分别使用两个不同的垃圾回收器。其实不论什么类型的垃圾回收器，它们都有一套共同的执行流程。

第一步是标记空间中活动对象和非活动对象。所谓活动对象就是还在使用的对象，非活动对象就是可以进行垃圾回收的对象。
第二步是回收非活动对象所占据的内存。其实就是在所有的标记完成之后，统一清理内存中所有被标记为可回收的对象。
第三步是做内存整理。一般来说，频繁回收对象后，内存中就会存在大量不连续空间，我们把这些不连续的内存空间称为内存碎片。当内存中出现了大量的内存碎片之后，如果需要分配较大连续内存的时候，就有可能出现内存不足的情况。所以最后一步需要整理这些内存碎片，但这步其实是可选的，因为有的垃圾回收器不会产生内存碎片，比如接下来我们要介绍的副垃圾回收器。

那么接下来，我们就按照这个流程来分析新生代垃圾回收器（副垃圾回收器）和老生代垃圾回收器（主垃圾回收器）是如何处理垃圾回收的。

#副垃圾回收器
副垃圾回收器主要负责新生区的垃圾回收。而通常情况下，大多数小的对象都会被分配到新生区，所以说这个区域虽然不大，但是垃圾回收还是比较频繁的。
新生代中用Scavenge 算法来处理。所谓 Scavenge 算法，是把新生代空间对半划分为两个区域，一半是对象区域，一半是空闲区域，如下图所示
新加入的对象都会存放到对象区域，当对象区域快被写满时，就需要执行一次垃圾清理操作。
在垃圾回收过程中，首先要对对象区域中的垃圾做标记；标记完成之后，就进入垃圾清理阶段，副垃圾回收器会把这些存活的对象复制到空闲区域中，同时它还会把这些对象有序地排列起来，所以这个复制过程，也就相当于完成了内存整理操作，复制后空闲区域就没有内存碎片了。
完成复制后，对象区域与空闲区域进行角色翻转，也就是原来的对象区域变成空闲区域，原来的空闲区域变成了对象区域。这样就完成了垃圾对象的回收操作，同时这种角色翻转的操作还能让新生代中的这两块区域无限重复使用下去。
由于新生代中采用的 Scavenge 算法，所以每次执行清理操作时，都需要将存活的对象从对象区域复制到空闲区域。但复制操作需要时间成本，如果新生区空间设置得太大了，那么每次清理的时间就会过久，所以为了执行效率，一般新生区的空间会被设置得比较小。
也正是因为新生区的空间不大，所以很容易被存活的对象装满整个区域。为了解决这个问题，JavaScript 引擎采用了对象晋升策略，也就是经过两次垃圾回收依然还存活的对象，会被移动到老生区中。

#主垃圾回收器
主垃圾回收器主要负责老生区中的垃圾回收。除了新生区中晋升的对象，一些大的对象会直接被分配到老生区。因此老生区中的对象有两个特点，一个是对象占用空间大，另一个是对象存活时间长。
由于老生区的对象比较大，若要在老生区中使用 Scavenge 算法进行垃圾回收，复制这些大的对象将会花费比较多的时间，从而导致回收执行效率不高，同时还会浪费一半的空间。因而，主垃圾回收器是采用标记 - 清除（Mark-Sweep）的算法进行垃圾回收的。下面我们来看看该算法是如何工作的。
首先是标记过程阶段。标记阶段就是从一组根元素开始，递归遍历这组根元素，在这个遍历过程中，能到达的元素称为活动对象，没有到达的元素就可以判断为垃圾数据。

比如最开始的那段代码，当 showName 函数执行退出之后，这段代码的调用栈和堆空间如下图所示：

从上图你可以大致看到垃圾数据的标记过程，当 showName 函数执行结束之后，ESP 向下移动，指向了 foo 函数的执行上下文，这时候如果遍历调用栈，是不会找到引用 1003 地址的变量，也就意味着 1003 这块数据为垃圾数据，被标记为红色。由于 1050 这块数据被变量 b 引用了，所以这块数据会被标记为活动对象。这就是大致的标记过程。

接下来就是垃圾的清除过程。它和副垃圾回收器的垃圾清除过程完全不同，你可以理解这个过程是清除掉红色标记数据的过程，可参考下图大致理解下其清除过程：

上面的标记过程和清除过程就是标记 - 清除算法，不过对一块内存多次执行标记 - 清除算法后，会产生大量不连续的内存碎片。而碎片过多会导致大对象无法分配到足够的连续内存，于是又产生了另外一种算法——标记 - 整理（Mark-Compact），这个标记过程仍然与标记 - 清除算法里的是一样的，但后续步骤不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存。你可以参考下图

#全停顿
现在你知道了 V8 是使用副垃圾回收器和主垃圾回收器处理垃圾回收的，不过由于 JavaScript 是运行在主线程之上的，一旦执行垃圾回收算法，都需要将正在执行的 JavaScript 脚本暂停下来，待垃圾回收完毕后再恢复脚本执行。我们把这种行为叫做全停顿（Stop-The-World）。

比如堆中的数据有 1.5GB，V8 实现一次完整的垃圾回收需要 1 秒以上的时间，这也是由于垃圾回收而引起 JavaScript 线程暂停执行的时间，若是这样的时间花销，那么应用的性能和响应能力都会直线下降。主垃圾回收器执行一次完整的垃圾回收流程如下图所示：

在 V8 新生代的垃圾回收中，因其空间较小，且存活对象较少，所以全停顿的影响不大，但老生代就不一样了。如果在执行垃圾回收的过程中，占用主线程时间过久，就像上面图片展示的那样，花费了 200 毫秒，在这 200 毫秒内，主线程是不能做其他事情的。比如页面正在执行一个 JavaScript 动画，因为垃圾回收器在工作，就会导致这个动画在这 200 毫秒内无法执行的，这将会造成页面的卡顿现象。

为了降低老生代的垃圾回收而造成的卡顿，V8 将标记过程分为一个个的子标记过程，同时让垃圾回收标记和 JavaScript 应用逻辑交替进行，直到标记阶段完成，我们把这个算法称为增量标记（Incremental Marking）算法。如下图所示：

使用增量标记算法，可以把一个完整的垃圾回收任务拆分为很多小的任务，这些小的任务执行时间比较短，可以穿插在其他的 JavaScript 任务中间执行，这样当执行上述动画效果时，就不会让用户因为垃圾回收任务而感受到页面的卡顿了。

6.组件间通信（父子，多层）
方法一、props/$emit父组件通过props向下传递数据给子组件。注：组件中的数据共有三种形式：data、props、computed子组件通过events给父组件发送消息，实际上就是子组件把自己的数据发送到父组件。
方法二、$emit/$on
这种方法通过一个空的Vue实例作为中央事件总线（事件中心），用它来触发事件和监听事件,巧妙而轻量地实现了任何组件间的通信，包括父子、兄弟、跨级。
方法三、vuex
Vuex实现了一个单向数据流，在全局拥有一个State存放数据，当组件要更改State中的数据时，必须通过Mutation进行，Mutation同时提供了订阅者模式供外部插件调用获取State数据的更新。而当所有异步操作(常见于调用后端接口异步获取更新数据)或批量的同步操作需要走Action，但Action也是无法直接修改State的，还是需要通过Mutation来修改State的数据。最后，根据State的变化，渲染到视图上。

具体做法应该在vuex里数据改变的时候把数据拷贝一份保存到localStorage里面，刷新之后，如果localStorage里有保存的数据，取出来再替换store里的state。
方法四、$attrs/$listeners
$attrs：包含了父作用域中不被 prop 所识别 (且获取) 的特性绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件。通常配合 inheritAttrs 选项一起使用。
$listeners：包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件
$attrs与$listeners 是两个对象，$attrs 里存放的是父组件中绑定的非 Props 属性，$listeners里存放的是父组件中绑定的非原生事件。

provide/inject
祖先组件中通过provider来提供变量，然后在子孙组件中通过inject来注入变量。
provide / inject API 主要解决了跨级组件间的通信问题，不过它的使用场景，主要是子组件获取上级组件的状态，跨级组件间建立了一种主动提供与依赖注入的关系。

方法六、$parent / $children与 ref
ref：如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例
$parent / $children：访问父 / 子实例
这两种方法的弊端是，无法在跨级或兄弟间通信。

父子通信：
父向子传递数据是通过 props，子向父是通过 events（$emit）；通过父链 / 子链也可以通信（$parent / $children）；ref 也可以访问组件实例；provide / inject API；$attrs/$listeners
兄弟通信：
Bus；Vuex
跨级通信：
Bus；Vuex；provide / inject API、$attrs/$listeners

7.DOM与BOM的区别
BOM 即浏览器对象模型，BOM没有相关标准，BOM的最核心对象是window对象。
DOM即文档对象模型，DOM是W3C标准，DOM的最根本对象是document（window.document），这个对象实际上是window对象的属性，这个对象的独特之处是这个是唯一一个既属于BOM又属于DOM的对象。

10.原生js添加事件的方式，对应的取消事件的方式
addEventListener同时存在捕获与冒泡时，捕获的优先级是高于冒泡的
使用stopPropagation可以阻止事件的传播。不能使用return false
removeEventListener

13.script不放在最后会怎样（阻塞渲染），即使放在前面也有避免的方法（async，defer），有没有更为完美的方法（不知道）
CSS 不会阻塞 DOM 的解析，但会阻塞 DOM 渲染。
JS 阻塞 DOM 解析，但浏览器会"偷看"DOM，预先下载相关资源。
浏览器遇到 <script>且没有defer或async属性的 标签时，会触发页面渲染，因而如果前面CSS资源尚未加载完毕时，浏览器会等待它加载完毕在执行脚本。

14.jQuery的实现原理
jQuery = function( selector, context ) {
return new jQuery.fn.init( selector, context, rootjQuery );
}
jQuery.fn = jQuery.prototype;
jQuery.fn.init.prototype = jQuery.fn;

递归实现阶乘
fun(int n){
if (n <= 1){
return 1;
}else{
return fun(n-1) * n;
}
}
实现树遍历
function dfsPreorderByRcs(tree) {
const output = [];
const visitLoop = (node) => {
if (node) {
// 先搜索出根节点的值，push进结果列表
output.push(node.data);
// 访问左子节点树，左子节点开始作为根节点进行下一轮递归
visitLoop(node.left);
// 同上述，递归遍历右子节点
visitLoop(node.right);
}
}
visitLoop(tree);
return output;
}

怎么衡量一个排序算法的稳定性
排序前后两个相等的数相对位置不变，则算法稳定。

快排的时间复杂度怎么计算的
nlogn
//手写快排
const quickSort = (array) => {
const sort = (arr, left = 0, right = arr.length - 1) => {
if (left >= right) {//如果左边的索引大于等于右边的索引说明整理完毕
return
}
let i = left
let j = right
const baseVal = arr[j] // 取无序数组最后一个数为基准值
while (i < j) {//把所有比基准值小的数放在左边大的数放在右边
while (i < j && arr[i] <= baseVal) { //找到一个比基准值大的数交换
i++
}
arr[j] = arr[i] // 将较大的值放在右边如果没有比基准值大的数就是将自己赋值给自己（i 等于 j）
while (j > i && arr[j] >= baseVal) { //找到一个比基准值小的数交换
j--
}
arr[i] = arr[j] // 将较小的值放在左边如果没有找到比基准值小的数就是将自己赋值给自己（i 等于 j）
}
arr[j] = baseVal // 将基准值放至中央位置完成一次循环（这时候 j 等于 i ）
sort(arr, left, j-1) // 将左边的无序数组重复上面的操作
sort(arr, j+1, right) // 将右边的无序数组重复上面的操作
}
const newArr = array.concat() // 为了保证这个函数是纯函数拷贝一次数组
sort(newArr)
return newArr
}

写一个大数加法
function add(a, b) {
if (a.length < b.length) {
a = '0' + a;
}
if (b.length < a.length) {
b = '0' + b;
}

``````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````
    // 标志位 满十进一
    var addOne = 0;
    var res = [];
    for (var i = a.length; i >= 0; i--) {
        //像小学加法那样，从最后一位开始相加
        var c1 = a.charAt(i) - 0;
        var c2 = b.charAt(i) - 0;
        var sum = c1 + c2 + addOne;
        //满十进一
        if (sum > 9) {
            addOne = 1;
            res.unshift(sum - 10);
        } else {
            addOne = 0;
            res.unshift(sum);
        }
    }
    // 如果是1，就进一
    if (addOne) {
        res.unshift(addOne);
    }
    // 假如这种情况，'01'+'02' = '03'
    if (res[0] === 0) {
        res.splice(0, 1);
    }
    //数组变字符串
    return res.join('');
``````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````

}

function sumStrings(a,b){
var res='', c=0;
a = a.split('');
b = b.split('');
console.log(a);
console.log(b);
while (a.length || b.length || c){
c += ~~a.pop() + ~~b.pop();
res = c % 10 + res;
//console.log(res);
c = c>9;
}
return res.replace(/^0+/,'');
}
大数乘法
var multiply = function(num1, num2) {
if(num1==0 || num2==0) return "0"
const res=[];// 结果集
for(let i=0;i<num1.length;i++){
let tmp1=num1[num1.length-1-i]; // num1尾元素
for(let j=0;j<num2.length;j++){
let tmp2 = num2[num2.length-1-j]; // num2尾元素
let pos = res[i+j] ? res[i+j]+tmp1*tmp2 : tmp1*tmp2;// 目标值 ==》三元表达式，判断结果集索引位置是否有值
res[i+j]=pos%10; // 赋值给当前索引位置
// 目标值是否大于10 ==》是否进位 这样简化res去除不必要的"0"
pos >=10 && (res[i+j+1]=res[i+j+1] ? res[i+j+1]+Math.floor(pos/10) : Math.floor(pos/10));
}
}
return res.reverse().join("");
};
function mul(num1, num2) {
if(num1 === '0' || num2 === '0') return '0'
let flag = 0,
result = '0',
tempResult = '',
temp = 0
for(let i=num2.length-1; i>=0; i--) {
flag = 0
tempResult = ''
temp = 0
for(let j=num1.length-1; j>=0; j--) {
temp = parseInt(num2[i])*parseInt(num1[j]) + flag
tempResult = (temp%10) + tempResult
flag = parseInt(temp/10)
}
tempResult = (flag>0?flag:'') + tempResult
result = add(result, tempResult+'0'.repeat(num2.length-1-i))
}
return result
}

prototype和__proto__的关系是什么？
1.对象有属性__proto__,指向该对象的构造函数的原型对象。
2.方法除了有属性__proto__,还有属性prototype，prototype指向该方法的原型对象。

请谈谈 Object.getPrototypeOf()、 Object.setPrototypeOf()、Object.create() ！
获取对象原型，设置原型，复制对象继承
请实现ECMAScript 5中的Object.getPrototypeOf() 函数！
手写getPrototypeOf
if(!Object.getPrototypeOf){
if('__proto__' in {}){
Object.getPrototypeOf=function(object){
return object.__proto__;
};
}else{
Object.getPrototypeOf=function(object){
var constructor=object.constructor;
if(object!=constructor.prototype){
return constructor.prototype;
}else if('superclass' in constructor){
return constructor.superclass.prototype;
}
console.warn("cannot find Prototype");
return Object.prototype;
};
}
}
手写setPrototypeOf
Object.prototype.setPrototypeOf = function(obj, proto) {
if(obj.__proto__) {
obj.__proto__ = proto;
return obj;
} else {
// 如果你想返回 prototype of Object.create(null):
var Fn = function() {
for (var key in obj) {
Object.defineProperty(this, key, {
value: obj[key],
});
}
};
Fn.prototype = proto;
return new Fn();
}
}
手写object.create
Object.myCreate = function(proto){
if (typeof proto !== 'object' && typeof proto !== 'function') {
throw new TypeError('Object prototype may only be an Object or null');
}
function F() {}
F.prototype = proto;
return new F();
}
手写instanceof
var myInstanceof = function(left, right){
if(left ===null || right === null)    return false;
left = Object.getPrototypeOf(left);
right = right.prototype;
while(true){
if(left === right)   return true;
left = Object.getPrototypeOf(left);
}
}
手写new
function myNew() {  // 接收参数：第一个为构造函数，后面为需要传入构造函数的参数
let obj = new Object(),
construct = [].shift.call(arguments);
Object.getPrototypeOf(obj) = construct.prototype;
let ret = construct.apply(obj, arguments);  // 注意：函数返回值为null,null被忽略
return (ret && typeof ret === 'object') ? ret : obj;
};

JavaScript 中，有一个函数，执行对象查找时，永远不会去查找原型，这个函数是哪个？
hasOwnProperty

Object.defineProperty()是干什么用的？
Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

请谈谈变量提升！
javaScript 变量提升JavaScript 中，函数及变量的声明都将被提升到函数的最顶部。 JavaScript 中，变量可以在使用后声明，也就是变量可以先使用再声明。

28、 什么是闭包（closure），为什么要用它？
简单的理解是函数的嵌套形成闭包，闭包包括函数本身已经它的外部作用域使用闭包可以形成独立的空间，延长变量的生命周期，报存中间状态值

29、javascript 代码中的"use strict";是什么意思 ? 使用它区别是什么？
意思是使用严格模式，使用严格模式，一些不规范的语法将不再支持

30、如何判断一个对象是否属于某个类？ Instanceof constructor
31、new 操作符具体干了什么呢?new做了什么

1. 创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
2. 属性和方法被加入到 this 引用的对象中。
3. 新创建的对象由 this 所引用，并且最后隐式的返回 this 。

32、用原生 JavaScript 的实现过什么功能吗？
主要考察原生 js 的实践经验

34、对 JSON 的了解？
轻量级数据交互格式，可以形成复杂的嵌套格式，解析非常方便

36、模块化开发怎么做？
理解模块化开发模式：浏览器端 requirejs，seajs；服务器端 nodejs；ES6 模块化；fis、webpack 等前端整体模块化解决方案；grunt、gulp 等前端工作流的使用

37、AMD（Modules/Asynchronous-Definition）、CMD（Common Module Definition） 规范区别？
理解这两种规范的差异，主要通过 requirejs 与 seajs 的对比，理解模块的定义与引用方式的差异以及这两种规范的设计原则

38、requireJS 的核心原理是什么？（如何动态加载的？如何避免多次加载的？ 如何 缓存的？）
核心是 js 的加载模块，通过正则匹配模块以及模块的依赖关系，保证文件加载的先后顺序， 根据文件的路径对加载过的文件做了缓存

39、让你自己设计实现一个 requireJS，你会怎么做？
核心是实现 js 的加载模块，维护 js 的依赖关系，控制好文件加载的先后顺序

40、谈一谈你对 ECMAScript6 的了解？
ES6 新的语法糖，类，模块化等新特性

41、ECMAScript6 怎么写 class 么，为什么会出现 class 这种东西?
class Point {
constructor(x, y) {
this.x = x;
this.y = y;
}
toString() {
return '('+this.x+', '+this.y+')';
}
}

42、异步加载的方式有哪些？
方案一：<script>标签的 async="async"属性（详细参见：script 标签的 async 属性） 方案二：<script>标签的 defer="defer"属性
方案三：动态创建<script>标签
方案四：AJAX eval（使用 AJAX 得到脚本内容，然后通过 eval_r(xmlhttp.responseText) 来运行脚本）
方案五：iframe 方式
43、documen.write 和 innerHTML 的区别?
document.write 是重写整个 document, 写入内容是字符串的 html innerHTML 是 HTMLElement 的属性，是一个元素的内部 html 内容

45、call() 和 .apply() 的含义和区别？
apply 的参数是数组形式，call 的参数是单个的值，除此之外在使用上没有差别，重点
理解这两个函数调用的 this 改变

46、数组和对象有哪些原生方法，列举一下？
Array.concat( ) 连接数组
Array.join( ) 将数组元素连接起来以构建一个字符串
Array.length 数组的大小
Array.pop( ) 删除并返回数组的最后一个元素
Array.push( ) 给数组添加元素
Array.reverse( ) 颠倒数组中元素的顺序
Array.shift( ) 将元素移出数组
Array.slice( ) 返回数组的一部分
Array.sort( ) 对数组元素进行排序
Array.splice( ) 插入、删除或替换数组的元素
Array.toLocaleString( ) 把数组转换成局部字符串
Array.toString( ) 将数组转换成一个字符串
Array.unshift( ) 在数组头部插入一个元素
Object.hasOwnProperty( ) 检查属性是否被继承
Object.isPrototypeOf( ) 一个对象是否是另一个对象的原型
Object.propertyIsEnumerable( ) 是否可以通过
for/in 循环看到属性
Object.toLocaleString( ) 返回对象的本地字符串表示
Object.toString( ) 定义一个对象的字符串表示
Object.valueOf( ) 指定对象的原始值
47、JS 怎么实现一个类。怎么实例化这个类
严格来讲 js 中并没有类的概念，不过 js 中的函数可以作为构造函数来使用，通过 new 来实例化，其实函数本身也是一个对象。

48、JavaScript 中的作用域与变量声明提升？
理解 JavaScript 的预解析机制，js 的运行主要分两个阶段：js 的预解析和运行，预解析阶段所有的变量声明和函数定义都会提前，但是变量的赋值不会提前

49、如何编写高性能的 Javascript？
使用 DocumentFragment 优化多次 append 通过模板元素 clone ，替代 createElement
使用一次 innerHTML 赋值代替构建 dom 元素
使用 firstChild 和 nextSibling 代替 childNodes 遍历 dom 元素使用 Array 做为 StringBuffer ，代替字符串拼接的操作
将循环控制量保存到局部变量
顺序无关的遍历时，用 while 替代 for 将条件分支，按可能性顺序从高到低排列
在同一条件子的多（ >2 ）条件分支时，使用 switch 优于 if 使用三目运算符替代条件分支
需要不断执行的时候，优先考虑使用 setInterval

50、那些操作会造成内存泄漏？
闭包，循环

51、javascript 对象的几种创建方式？

1. 工厂模式
2. 构造函数模式
3. 原型模式
4. 混合构造函数和原型模式
5. 动态原型模式
6. 寄生构造函数模式
7. 稳妥构造函数模式

52、javascript 继承的 6 种方法？

1. 原型链继承
2. 借用构造函数继承
3. 组合继承(原型+借用构造)
4. 原型式继承
5. 寄生式继承
6. 寄生组合式继承

53、eval 是做什么的？

1. 它的功能是把对应的字符串解析成 JS 代码并运行
2. 应该避免使用 eval，不安全，非常耗性能（2 次，一次解析成 js 语句，一次执行）

54、JavaScript 原型，原型链 ? 有什么特点？

1. 原型对象也是普通的对象，是对象一个自带隐式的 proto 属性，原型也有可能有自己的原型，如果一个原型对象的原型不为 null 的话，我们就称之为原型链
2. 原型链是由一些用来继承和共享属性的对象组成的（有限的）对象链

55、事件、IE 与火狐的事件机制有什么区别？ 如何阻止冒泡？

1. 我们在网页中的某个操作（有的操作对应多个事件）。例如：当我们点击一个按钮就会产生一个事件。是可以被 JavaScript 侦测到的行为
2. 事件处理机制：IE 是事件冒泡、firefox 同时支持两种事件模型，也就是：捕获型事件和冒泡型事件
3. ev.stopPropagation();
   注意旧 ie 的方法：ev.cancelBubble = true;

56、简述一下 Sass、Less，且说明区别？
他们是动态的样式语言，是 CSS 预处理器,CSS 上的一种抽象层。他们是一种特殊的语法/语言而编译成 CSS。
变量符不一样，less 是@，而 Sass 是$; Sass 支持条件语句，可以使用 if{}else{},for{}循环等等。而 Less 不支持;
Sass 是基于 Ruby 的，是在服务端处理的，而 Less 是需要引入 less.js 来处理 Less 代码输
出 Css 到浏览器

57、关于 javascript 中 apply()和 call()方法的区别？
相同点:两个方法产生的作用是完全一样的不同点:方法传递的参数不同Object.call(this,obj1,obj2,obj3) Object.apply(this,arguments)
apply()接收两个参数，一个是函数运行的作用域(this)，另一个是参数数组。call()方法第一个参数与 apply()方法相同，但传递给函数的参数必须列举出来。

58、简述一下 JS 中的闭包？
闭包用的多的两个作用：读取函数内部的变量值；让这些变量值始终保存着(在内存中)。 同时需要注意的是：闭包慎用，不滥用，不乱用，由于函数内部的变量都被保存在内存中， 会导致内存消耗大。

59、说说你对 this 的理解？
在 JavaScript 中，this 通常指向的是我们正在执行的函数本身，或者是，指向该函数所属的对象。
全局的 this → 指向的是 Window
函数中的 this → 指向的是函数所在的对象对象中的 this → 指向其本身

60、分别阐述 split(),slice(),splice(),join()？
join()用于把数组中的所有元素拼接起来放入一个字符串。所带的参数为分割字符串的分隔 符，默认是以逗号分开。归属于 Array
split()即把字符串分离开，以数组方式存储。归属于 Stringstring
slice() 方法可从已有的数组中返回选定的元素。该方法并不会修改数组，而是返回一个子数组。如果想删除数组中的一段元素，应该使用方法 Array.splice()
splice() 方法向/从数组中添加/删除项目，然后返回被删除的项目。返回的是含有被删除的元素的数组。

61、事件委托是什么？
让利用事件冒泡的原理，让自己的所触发的事件，让他的父元素代替执行！

62、如何阻止事件冒泡和默认事件？
阻止浏览器的默认行为window.event?window.event.returnValue=false:e.preventDefault(); 停止事件冒泡window.event?window.event.cancelBubble=true:e.stopPropagation();
原生JavaScript中，return false;只阻止默认行为，不阻止冒泡，jQuery中的return false;
既阻止默认行为，又阻止冒泡

63、添加 删除 替换 插入到某个接点的方法？
obj.appendChidl() obj.removeChild() obj.replaceChild() obj.innersetBefore()

64、你用过 require.js 吗？它有什么特性？
（1）实现 js 文件的异步加载，避免网页失去响应；
（2）管理模块之间的依赖性，便于代码的编写和维护。

65、谈一下 JS 中的递归函数，并且用递归简单实现阶乘？
递归即是程序在执行过程中不断调用自身的编程技巧，当然也必须要有一个明确的结束条 件，不然就会陷入死循环。

66、请用正则表达式写一个简单的邮箱验证。
/^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;

67、简述一下你对 web 性能优化的方案？

1. 尽量减少 HTTP 请求
2. 使用浏览器缓存
3. 使用压缩组件
4. 图片、JS 的预载入
5. 将脚本放在底部
6. 将样式文件放在页面顶部
7. 使用外部的 JS 和 CSS
8. 精简代码

68、在 JS 中有哪些会被隐式转换为 false
Undefined、null、关键字 false、NaN、零、空字符串

69 、定时器 setInterval 有一个有名函数 fn1，setInterval（fn1,500）与
setInterval（fn1(),500）有什么区别？
第一个是重复执行每 500 毫秒执行一次，后面一个只执行一次。

70、外部 JS 文件出现中文字符，会出现什么问题，怎么解决？ 会出现乱码，加 charset=”GB2312”;

71、谈谈浏览器的内核，并且说一下什么是内核？
Trident (['traɪd(ə)nt])--IE ， Gecko (['gekəʊ])--Firefox, Presto (['prestəʊ])--opera,webkit—谷歌和 Safari
浏览器内核又可以分成两部分：渲染引擎和 JS 引擎。它负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入 CSS 等），以及计算网页的显示方式，然后会输出至显示器或打印机。JS 引擎则是解析 Javascript 语言，执行 javascript 语言来实现网页的动态效果。

72、JavaScript 原型，原型链 ? 有什么特点？

* 原型对象也是普通的对象，是对象一个自带隐式的 proto 属性，原型也有可能有自己的原型，如果一个原型对象的原型不为 null 的话，我们就称之为原型链。
* 原型链是由一些用来继承和共享属性的对象组成的（有限的）对象链。
* JavaScript 的数据对象有那些属性值？
  writable：这个属性的值是否可以改。
  configurable：这个属性的配置是否可以删除，修改。
  enumerable：这个属性是否能在for…in循环中遍历出来或在Object.keys中列举出来。value：属性值。
* 当我们需要一个属性的时，Javascript 引擎会先看当前对象中是否有这个属性， 如果没有的话，就会查找他的 Prototype 对象是否有这个属性。

74、事件、IE 与火狐的事件机制有什么区别？ 如何阻止冒泡？

1. 我们在网页中的某个操作（有的操作对应多个事件）。例如：当我们点击一个按钮就会 产生一个事件。是可以被 JavaScript 侦测到的行为。
2. 事件处理机制：IE 是事件冒泡、火狐是 事件捕获；
3. ev.stopPropagation();

75、什么是闭包（closure），为什么要用？
执行 say667()后,say667()闭包内部变量会存在,而闭包内部函数的内部变量不会存在.使得 Javascript 的垃圾回收机制 GC 不会收回 say667()所占用的资源，因为 say667()的内部函数的执行需要依赖 say667()中的变量。这是对闭包作用的非常直白的描述.

76、如何判断一个对象是否属于某个类？
使用 instanceof （待完善）
if(a instanceof Person){
alert('yes');
}

78、JSON 的了解
JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。它是基于 JavaScript 的一个子集。数据格式简单, 易于读写, 占用带宽小
{'age':'12', 'name':'back'}

79、js 延迟加载的方式有哪些
defer 和 async、动态创建 DOM 方式（用得最多）、按需异步载入 js

81、异步加载的方式
(1) defer，只支持 IE
(2) async：
(3) 创建 script，插入到 DOM 中，加载完毕后 callBack
documen.write 和 innerHTML 的区别document.write 只能重绘整个页面
innerHTML 可以重绘页面的一部分

82、告诉我答案是多少？
(function(x){
delete x; alert(x);
})(1+5);
函数参数无法 delete 删除，delete 只能删除通过 for in 访问的属性。当然，删除失败也不会报错，所以代码运行会弹出“1”。

83、JS 中的 call()和 apply()方法的区别？
例子中用 add 来替换 sub，add.call(sub,3,1) == add(3,1) ，所以运行结果为：alert(4); 注意：js 中的函数其实是对象，函数名是对 Function 对象的引用。
function add(a,b){ alert(a+b);
}
function sub(a,b){ alert(a-b);
}
add.call(sub,3,1);

九、移动 APP 开发

1. 移动端最小触控区域是多大？
   移动端的点击事件的有延迟，时间是多久，为什么会有？ 怎么解决这个延时？（click 有
   300ms 延迟,为了实现 safari 的双击事件的设计，浏览器要知道你是不是要双击操作。）
2. 对 Node 的优点和缺点提出了自己的看法：
   *（优点）因为 Node 是基于事件驱动和无阻塞的，所以非常适合处理并发请求，
   因此构建在 Node 上的代理服务器相比其他技术实现（如 Ruby）的服务器表现要好得多。此外，与 Node 代理服务器交互的客户端代码是由 javascript 语言编写的，
   因此客户端和服务器端都用同一种语言编写，这是非常美妙的事情。
   *（缺点）Node 是一个相对新的开源项目，所以不太稳定，它总是一直在变， 而且缺少足够多的第三方库支持。看起来，就像是 Ruby/Rails 当年的样子。
3. 需求：实现一个页面操作不会整页刷新的网站，并且能在浏览器前进、后退时正确响应。给出你的技术实现方案？
   至少给出自己的思路（url-hash,可以使用已有的一些框架 history.js 等）
4. Node.js 的适用场景？
   1)、实时应用：如在线聊天，实时通知推送等等（如 http://socket.io）
   2)、分布式应用：通过高效的并行 I/O 使用已有的数据
   3)、工具类应用：海量的工具，小到前端压缩部署（如 grunt），大到桌面图形界面应用程序
   4)、游戏类应用：游戏领域对实时和并发有很高的要求（如网易的 pomelo 框架）
   5)、利用稳定接口提升 Web 渲染能力
   6)、前后端编程语言环境统一：前端开发人员可以非常快速地切入到服务器端的开发（如著 名的纯 Javascript 全栈式 MEAN 架构）
5. 对 Node 的优点和缺点提出了自己的看法？
   优点：
6. 因为 Node 是基于事件驱动和无阻塞的，所以非常适合处理并发请求，因此构建在 Node 上的代理服务器相比其他技术实现（如 Ruby）的服务器表现要好得多。
7. 与 Node 代理服务器交互的客户端代码是由 javascript 语言编写的，因此客户端和服务器端都用同一种语言编写，这是非常美妙的事情。
   缺点：
8. Node 是一个相对新的开源项目，所以不太稳定，它总是一直在变。
9. 缺少足够多的第三方库支持。看起来，就像是 Ruby/Rails 当年的样子（第三方库现在已经很丰富了，所以这个缺点可以说不存在了）。
10. 对 BFC 规范的理解？
    Formatting Context：指页面中的一个渲染区域，并且拥有一套渲染规则，他决定了其子元素如何定位，以及与其他元素的相互关系和作用。
11. WEB 应用从服务器主动推送 Data 到客户端有那些方式？
    html5 websoket WebSocket 通过 Flash
    XHR 长时间连接
    XHR Multipart Streaming 不可见的 Iframe

<script>标签的长时间连接(可跨域)

7. 那些操作会造成内存泄漏？
内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。
垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用 数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。
setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）


22、如何优化网页加载速度？
1.减少 css，js 文件数量及大小(减少重复性代码，代码重复利用)，压缩 CSS 和 Js 代码
2.图片的大小
3.把 css 样式表放置顶部，把 js 放置页面底部4.减少 http 请求数
5.使用外部 Js 和 CSS

26、对前端界面工程师这个职位是怎么样理解的？它的前景会怎么样？
前端是最贴近用户的程序员，比后端、数据库、产品经理、运营、安全都近。1、实现界面交互
2. 提升用户体验
3. 有了 Node.js，前端可以实现服务端的一些事情
前端是最贴近用户的程序员，前端的能力就是能让产品从 90 分进化到 100 分，甚至更好， 参与项目，快速高质量完成实现效果图，精确到 1px；
与团队成员，UI 设计，产品经理的沟通； 做好的页面结构，页面重构和用户体验； 处理 hack，兼容、写出优美的代码格式；


AJAX请求总共有多少种CALLBACK?
  onSuccess
　　onFailure
　　onUninitialized
　　onLoading
　　onLoaded
　　onInteractive
　　onComplete
　　onException

Ajax的优缺点及工作原理？
AJAX的优点
1. 提高了性能和速度
减少客户端和服务器之间的流量传输，同时减少了双方响应的时间，响应更快，因此提高了性能和速度。
2. 交互性能好
使用ajax，可以开发更快，更具交互性的Web应用程序。
3. 异步调用
AJAX对Web服务器进行异步调用。这意味着客户端浏览器在开始渲染之前避免等待所有数据到达。
4. 节省带宽
基于Ajax的应用程序使用较少的服务器带宽，因为无需重新加载完整的页面。
5. 使用XMLHttpRequest
AJAX的缺点
1. 增加了设计和开发时间
2. 比构建经典Web应用程序更复杂
3. AJAX应用程序中的安全性较低，因为所有文件都是在客户端下载的。
4. 可能出现网络延迟问题
5. 禁用JavaScript的浏览器无法使用该应用程序。
6. 由于安全限制，只能使用它来访问服务于初始页面的主机的信息。如果需要显示来自其他服务器的信息，则无法在AJAX中显示。

如何解决ajax无法后退的问题？
1.每次手动点击左侧的菜单，我将Ajax地址的查询内容(?后面的)附在demo HTML页面地址后面，使用history.pushState塞到浏览器历史中。
2.浏览器的前进与后退，会触发window.onpopstate事件，通过绑定popstate事件，就可以根据当前URL地址中的查询内容让对应的菜单执行Ajax载入，实现Ajax的前进与后退效果。
3.页面首次载入的时候，如果没有查询地址、或查询地址不匹配，则使用第一个菜单的Ajax地址的查询内容，并使用history.replaceState更改当前的浏览器历史，然后触发Ajax操作

类数组有哪些？
arguments NodeList HTMLCollection

关于DOM
JavaScript中Element与Node的区别，children与childNodes的区别！
Element继承了Node类，也就是说Element是Node多种类型中的一种，即当NodeType为1时Node即为ElementNode，另外Element扩展了Node，Element拥有id、class、children等属性。
　children是Element的属性，childNodes是Node的属性
Node的children属性为为undefined。

兼容浏览器的获取指定元素（elem）的样式属性（name）的方法！
function getStyle(elem, name){
  //如果属性存在于style[]中，直接取
  if(elem.style[name]){
    return elem.style[name];
  }
  //否则 尝试IE的方法
  else if(elem.currentStyle){
    return elem.currentStyle[name];
  }
  //尝试W3C的方式
  else if(document.defaultView && document.defaultView.getComputedStyle){
    //W3C中为textAlign样式，转为text-align
    name = name.replace(/([A-Z])/g, "-$1");
    name = name.toLowerCase();

    var s = document.defaultView.getComputedStyle(elem, "");
    return s && s.getPropertyValue(name);
  } else {
    return null;
  }

}

如何获取光标的水平位置？
function getCursortPosition (ctrl)
{
    //获取光标位置函数
    var CaretPos = 0;
    // IE Support
    if (document.selection)
    {
        ctrl.focus (); // 获取焦点
        var Sel = document.selection.createRange (); // 创建选定区域
        Sel.moveStart('character', -ctrl.value.length); // 移动开始点到最左边位置
        CaretPos = Sel.text.length;                      // 获取当前选定区的文本内容长度
    }
    // Firefox support (非ie)
    else if (ctrl.selectionStart || ctrl.selectionStart == '0')
    {
        CaretPos =ctrl.selectionStart; // 获取选定区的开始点
    }
    return CaretPos;
}
//ctrl 是dom节点，pos 是要定位到的位置
function setCaretPosition(ctrl, pos)
{
    //设置光标位置函数
    if(ctrl.setSelectionRange)   //非ie
    {
        ctrl.focus();  // 获取焦点
        ctrl.setSelectionRange(pos,pos);  // 设置选定区的开始和结束点
    }
    else if (ctrl.createTextRange)
    {
        var range = ctrl.createTextRange();  // 创建选定区
        range.collapse(true);                // 设置为折叠,即光标起点和结束点重叠在一起
        range.moveEnd('character', pos);     // 移动结束点
        range.moveStart('character', pos);   // 移动开始点
        range.select();                      // 选定当前区域
    }
}


浏览器里面的事件都会按照一定的规则去传递，这个规则是什么？
事件捕获、事件响应、事件冒泡。
事件流的三个阶段，哪些事件不能冒泡？如何阻止冒泡？
目前不支持冒泡的事件有哪些呢？
blur、focus、load、unload 、以及自定义的事件。
原因是在于：这些事件仅发生于自身上，而它的任何父节点上的事件都不会产生，所有不会冒泡。

请指出document load和document ready的区别？
ready 事件的触发，表示文档结构已经加载完成（不包含图片等非文字媒体文件）。
onload 事件的触发，表示页面包含图片等文件在内的所有元素都加载完成。

能否写一个通用的事件侦听器函数？
var EventUtil = {
    //根据情况分别使用dom2 || IE || dom0方式 来添加事件
    addHandler: function(element,type,handler) {
        if(element.addEventListener) {
            element.addEventListener(type,handler,false);
        } else if(element.attachEvent) {
            element.attachEvent("on" + type,handler);
        } else {
            element["on" + type] = handler;
        }
    },

    //根据情况分别获取DOM或者IE中的事件对象，事件目标，阻止事件的默认行为
    getEvent: function(event) {
        return event ? event: window.event;
    },
    getTarget: function(event) {
        return event.target || event.srcElement;
    },
    preventDefault: function(event) {
        if(event.preventDefault) {
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    }

    //根据情况分别使用dom2 || IE || dom0方式 来删除事件
    removeHandler: function(element,type,handler){
        if(element.removeHandler) {
            element.removeEventListener(type,handler,false);
        } else if(element.detachEvent) {
            element.detachEvent("on" + type,handler);
        } else {
            element["on" + type] = null;
        }
    }

    //根据情况分别取消DOM或者IE中事件冒泡
    stopPropagation: function(event) {
        if (event.stopPropagation) {
            event.stopPropagation();
        } else {
            event.cancelBubble = true;
        }
    }
}

var btn = document.getElementById("myBtn"),
    handler = function () {
        alert("Clicked");
    };

EventUtil.addHandler(btn,"click",handler);
EventUtil.removeHandler(btn,"click",handler);
表单的change事件和input事件有什么区别？
input输入框的onchange事件，要在 input 失去焦点的时候才会触发；
oninput 事件在用户输入时触发，它是在元素值发生变化时立即触发；

amd和cmd的区别，怎么了解到这些区别的，是否是去看了规范
对于依赖的模块，AMD 是提前执行，CMD 是延迟执行。不过 RequireJS 从 2.0 开始，也改成可以延迟执行（根据写法不同，处理方式不同）。CMD 推崇 as lazy as possible.
CMD 推崇依赖就近，AMD 推崇依赖前置
AMD 的 API 默认是一个当多个用，CMD 的 API 严格区分，推崇职责单一

请谈谈你对ES6 Module的理解！如何使用ES6 Module？
CommonJS 和 ES6 Module对比有哪些区别？
ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。
关于ES6


谈谈async/await！
async用于申明一个function是异步的，而await 用于等待一个异步方法执行完成。

说一说回调地狱是什么，有什么问题。异常捕获怎么做。
使用promise解决回调地狱，捕获异常

说一说promise。一个promise有多个then，如果第一个then出错，后面的还会执行吗，如何捕获异常。
如果第一个then出错了，我还想要后面的继续执行，应该怎么做。

手写promise方法
https://zhuanlan.zhihu.com/p/143699690
因为引用计数产生的内存泄漏，在ES6中的解决办法是什么？
使用 WeakMap。

Buffer模块是干什么的
Buffer作为存在于全局对象上，无需引入模块即可使用，你绝对不可以忽略它。 可以理解Buffer是在内存中开辟的一片区域，用于存放二进制数据。Buffer所开辟的是堆外内存。
流
怎么理解流呢？流是数据的集合（与数据、字符串类似），但是流的数据不能一次性获取到，数据也不会全部load到内存中，因此流非常适合大数据处理以及断断续续返回chunk的外部源。流的生产者与消费者之间的速度通常是不一致的，因此需要buffer来暂存一些数据。buffer大小通过highWaterMark参数指定，默认情况下是16Kb。
存储需要占用大量内存的数据
Buffer 对象占用的内存空间是不计算在 Node.js 进程内存空间限制上的，所以可以用来存储大对象，但是对象的大小还是有限制的。一般情况下32位系统大约是1G，64位系统大约是2G。

http模块如何将异步处理方式实现成同步处理方式
async/await

vue的事件监听；
v-on

vuex的action和mutation的异步操作和同步操作问题；
 actions 和 mutations 并不是为了解决竞态问题，而是为了能用 devtools 追踪状态变化。事实上在 vuex 里面 actions 只是一个架构性的概念，并不是必须的，说到底只是一个函数，你在里面想干嘛都可以，只要最后触发 mutation 就行。异步竞态怎么处理那是用户自己的事情。vuex 真正限制你的只有 mutation 必须是同步的这一点。同步的意义在于这样每一个 mutation 执行完成后都可以对应到一个新的状态，这样 devtools 就可以打个 snapshot 存下来，然后就可以随便 time-travel 了。如果你开着 devtool 调用一个异步的 action，你可以清楚地看到它所调用的 mutation 是何时被记录下来的，并且可以立刻查看它们对应的状态。

哪些操作会造成内存泄漏？
1. 闭包引起的内存泄漏；
2. 意外的全局变量引起的内存泄漏；
3. 没有清理的DOM元素引起的内存泄漏；
4. 被遗忘的定时器或者回调函数；
5. 子元素存在引用引起的内存泄漏；
开发过程中遇到的内存泄露情况，如何解决的？

想实现一个对页面某个节点的拖曳？如何做？（使用原生JS）。
给需要拖拽的节点绑定mousedown, mousemove, mouseup事件
mousedown事件触发后，开始拖拽
mousemove时，需要通过event.clientX和clientY获取拖拽位置，并实时更新位置
mouseup时，拖拽结束
需要注意浏览器边界的情况
var box = document.getElementById("box");
box.onmousedown = function(e){
var e = e || window.event;
var diffX = e.clientX - box.offsetLeft;
var diffY = e.clientY - box.offsetTop;
document.onmousemove = function(e){
var e = e || window.event;
box.style.left = e.clientX - diffX + "px";
box.style.top = e.clientY - diffY + "px";
};
document.onmouseup = function(){
document.onmousemove = null;
document.onmousedown = null;
};
};
判断一个字符串中出现次数最多的字符，统计这个次数。

编写一个方法 求一个字符串的字节长度！
function GetBytes(str){

        var len = str.length;

        var bytes = len;

        for(var i=0; i<len; i++){

            if (str.charCodeAt(i) > 255) bytes++;

        }

        return bytes;

    }

写一个function，清除字符串前后的空格。
/(^\s+)|(\s+$)/g
怎样用js实现千位分隔符？
/(\d)(?=(\d{3})+$)/g

Javascript中实现类似PHP的print_r函数！
function print_r(arr){
  var output;
  if(arr.constructor == Array || arr.constructor == Object){
    output="Array\n";
    output=output+"(\n";
    for(var p in arr){
      if(arr[p].constructor == Array || arr[p].constructor == Object){
        output=output+"    ["+p+"] => "+print_r(arr[p]);
      } else {
        output=output+"    ["+p+"] => "+arr[p]+"\n";
      }
    }
    output=output+")\n";
  }
  return output;
}

delegate如何实现
var delegate = function(criteria, listener) {
  return function(e) {
    var el = e.target;
    do {
      if (!criteria(el)) continue;
      e.delegateTarget = el;
      listener.apply(this, arguments);
      return;
    } while( (el = el.parentNode) );
  };
};

前端异常监测如何实现
老的浏览器不支持hashchange这些事件怎么办（不会。。面试官说可以用轮询）

三种隐藏
而使用 visibility 属性时，子元素如果设置为 visibility:visible; 并没有受父元素的影响，可以继续显示出来。
使用 visibility 和 display 属性，自身的事件不会触发，而使用 opacity 属性，自身绑定的事件还是会触发的
visibility 和 display 属性是不会影响其他元素触发事件的，而 opacity 属性 如果遮挡住其他元素，其他的元素就不会触发事件了。
dispaly 属性会产生回流，而 opacity 和 visibility 属性不会产生回流。
dispaly 和 visibility 属性会产生重绘，而 opacity 属性不一定会产生重绘。
https://segmentfault.com/a/1190000015116392

HTML/HTML5 API
浏览器存储
localStorage 是注册在几级域名底下的？
同一浏览器的相同域名和端口的不同页面间可以共享相同的 localStorage；但是同一浏览器的相同域名和端口的不同页面间无法共享sessionStorage的信息。这里需要注意的是，页面仅指顶级窗口，如果一个页面包含多个iframe且他们属于同源页面，那么他们之间是可以共享sessionStorage的。在实际开发过程中，遇到的最多的问题就是localStorage的同源策略问题。
websocket
websocket聊天室如果发送失败了，你怎么解决这个问题？如何做到发送图片？ 有了文字、图片等不同的数据类型之后，你如何实现数据的存储，如何设计，前端如何获取？

Canvas和SVG的比较！
Canvas是使用JavaScript程序绘图(动态生成)，SVG是使用XML文档描述来绘图。     从这点来看：SVG更适合用来做动态交互，而且SVG绘图很容易编辑，只需要增加或移除相应的元素就可以了。       同时SVG是基于矢量的，所有它能够很好的处理图形大小的改变。Canvas是基于位图的图像，它不能够改变大小，只能缩放显示；所以说Canvas更适合用来实现类似于Flash能做的事情(当然现在Canvas与Flash相比还有一些不够完善的地方)。

HTML5的离线存储怎么使用？能否解释一下工作原理？
如上面提到的HTML5的离线存储是基于一个新建的.appcache文件的，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。
离线浏览--用户可在离线时使用它们。
速度--已经缓存的资源加载得更快。
减少服务器负载--浏览器将只从服务器下载更改过的资源。
浏览器是怎么对HTML5的离线储存资源进行管理和加载的呢？
在线的情况下，浏览器发现html头部有manifest属性，它会请求manifest文件，如果是第一次访问app，那么浏览器就会根据manifest文件的内容下载相应的资源并且进行离线存储。如果已经访问过app并且资源已经离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的manifest文件与旧的manifest文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，那么就会重新下载文件中的资源并进行离线存储。离线的情况下，浏览器就直接使用离线存储的资源。

页面可见性（Page Visibility） API可以有哪些用途？
通过监听网页的可见性，可以预判网页的卸载，还可以用来节省资源，减缓电能的消耗。比如，一旦用户不看网页，对服务器的轮询网页动画正在播放的音频或视频
如何在页面上实现一个圆形的可点击区域？
三种做法。
纯html: map+area;
纯CSS: border-radius: 50%;
纯js: 利用点在圆内点在圆外

meta标签作用！
application name  当前页所属Web应用系统的名称
keywords  描述网站内容的关键词,以逗号隔开，用于SEO搜索
description 当前页的说明
author  当前页的作者名
copyright 版权信息
renderer  renderer是为双核浏览器准备的，用于指定双核浏览器默认以何种方式渲染页面
viewreport  它提供有关视口初始大小的提示，仅供移动设备使用
iframe 有哪些缺点？

HTML5的form如何关闭自动完成功能？
设置Form的autocomplete为"on"或者"off"来开启或者关闭自动完成功能
简述一下src与href的区别？
href 时指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，用于超链接。 src 时指向外部的资源位置，指向的内容将会嵌入到文档中当前标签所在位置；在请求 src 资源时会将其指向的资源下载并应用到文档内，例如js 脚本，img 图片和frame 等元素。
介绍一下盒模型！

盒模型的组成包括:content, padding, border, margin组成。
有两种盒模型:标准盒模型和IE盒模型。两种盒模型的主要区别是:标准盒模型的宽高是值内容宽高(content),而IE盒模型的宽高是指content+padding+border。
设置盒模型的方式是：设置box-sizing
    box-sizing:content-box  标准盒模型
    box-sizing:border-box IE盒模型

文档流是元素在Web页面上的一种呈现方式。所有的HTML元素都是块盒子（Block Boxes，块级元素）或行内框（Inline Boxes，行内元素）。当浏览器开始渲染HTML文档时，它从窗口的顶端开始，经过整个文档内容的过程中，分配元素需要的空间。除非文档的尺寸被CSS规则限定，否则浏览器垂直扩展文档来容纳全部的内容。每个新的块级元素渲染为新行。行内元素则按照顺序被水平渲染直到当前行遇到边界，然后换到下一行垂直渲染。

有没有了解过css3的position:sticky;
单词sticky的中文意思是“粘性的”，position:sticky表现也符合这个粘性的表现。基本上，可以看出是position:relative和position:fixed的结合体——当元素在屏幕内，表现为relative，就要滚出显示器屏幕的时候，表现为fixed

有没有遇到过这样的问题： 一个有border的div，里面有一个图片，发现图片和下面的border有一定的空隙（baseline）。
css高度坍塌：两个盒子，一个下边据20px，一个上边据50px，最后为两个盒子之间的距离为多少？如何解决高度坍塌？
给父元素添加overflow：hidden；
弊端：这样会使超出的部分内容隐藏。
优点：写起来简单。

2.添加一个空的标签（例如：div）。
目的：是为了消除子元素的浮动。
代码：div{clear：both； height：0； width：0；overflow：hidden；}
.fix{zoom:1;}
.fix:after{display:block; content:'clear'; clear:both; line-height:0; visibility:hidden;}
3.万能清除法（推荐使用）
最近的父标签 ：：after{content：“，”；display：block；
clear：both；width：0；height:0;}
怎么让Chrome支持小于12px 的文字？
.chrome_adjust{
    font-size: 10px; //针对可以识别12px以下字体大小的浏览器
    -webkit-transform: scale(0.8);
    -webkit-transform-origin-X: left; //定义文字相对于X轴的位置
    -o-transform: scale(1); //针对能识别-webkit的opera browser设置
    display: inline-block;
}

说一说CSS3中的动画，animation中可以取哪些值？
animation动画，其可以不需要事件触发就可进行，其属性如下：
animation-name 需要绑定到选择器的keyframe名称
animation-duration 动画持续时间
animation-timing-function 动画速度曲线，以什么样的速度进行变化，与transition的animation-timing-function一样
animation-delay 动画延迟时间
animation-iteration-count 播放次数，其取值可以是数字，或者infinite(无限次播放)
animation-direction 是否应该轮流反向播放动画，默认取值normal，也可以取值alternate(反响播放动画)
animation-play-state 规定动画是否正在运行或暂停
animation-fill-mode 固定动画时间之外状态

说一说CSS3中的变形，transform都有哪些选项？
 transform: rotate | scale | skew | translate |matrix;

媒体查询的原理是什么？
浏览器探测用户的视口宽度从而调用不同的样式树和DOM结构树进行一个合成渲染。

移动端解决屏幕旋转问题！
window.orientation;
https://www.jianshu.com/p/ebae956a2adb

异步加载JS文件的方式有哪些？
将<script>标签放到<body>底部
defer属性
async属性
动态创建<script>标签
利用ajax请求js的代码并用eval执行
用iframe引入js
6:requirejs
7:import
8:define
1. defer脚本的执行会在window.onload之前，其他没有添加defer属性的script标签之后。
2. async会让脚本在下载完可用时立即执行，而defer脚本则会在dom加载完毕后执行，
3. async不能确保加载执行的顺序，多个 defer 脚本，会按照它们在页面出现的顺序加载
请说出三种减低页面加载时间的方法！
1. 减少http请求（合并文件、合并图片）

2. 优化图片文件，减小其尺寸，特别是缩略图，一定要按尺寸生成缩略图然后调用，不要在网页中用resize方法实现，虽然这样看到的图片外形小了，但是其加载的数据量一点也没减少。曾经见过有人在网页中加载的缩略图，其真实尺寸有10M之巨…普通图像、icon也要尽可能压缩后，可以采用web图像保存、减少颜色数等等方法实现。

3. 图像格式的选择（GIF：提供的颜色较少，可用在一些对颜色要求不高的地方）

4.  压缩Javascript、CSS代码：一般js、css文件中存在大量的空格、换行、注释，这些利于阅读，如果能够压缩掉，将会很有利于网络传输。这方面的工具也有很多，可以在百度里搜索一下关键字“css代码压缩”，或者“js代码压缩”将会发现有很多网站都提供这样的功能，当然了你也可以自己写程序来做这个工作，如果你会的话。就拿我们这个网站来说吧。刚开始上传这个网站的时候，我的很多Css代码都没有压缩，后面发现了这个问题，我就上网找了相关的网站的压缩代码的功能，最后就把很多CSS文件都压缩了。这个压缩比率还是比较高的，一般都有百分五十左右。这个代码压缩对于网页的加载还是很有用的。

5.  服务器启用gzip压缩功能：将要传输的文件压缩后传输到客户端再解压，在网络传输 数据量会大幅减小。在服务器上的Apache、Nginx可直接启用，也可用代码直接设置传输文件头，增加gzip的设置，也可从 负载均衡设备直接设置。不过需要留意的是，这个设置会略微增加服务器的负担。服务器性能不是很好的网站，要慎重考虑。

6.标明高度和宽度（如果浏览器没有找到这两个参数，它需要一边下载图片一边计算大小，如果图片很多，浏览器需要不断地调整页面。这不但影响速度，也影响浏览体验。 当浏览器知道了高度和宽度参数后，即使图片暂时无法显示，页面上也会腾出图片的空位，然后继续加载后面的内容。从而加载时间快了，浏览体验也更好了。）

7. 网址后面加上“/”:对服务器而言，不加斜杠服务器会多一次判断的过程，加斜杠就会直接返回网站设置的存放在网站根目录下的默认页面。

首屏、白屏时间如何计算！
在html文档的head中所有的静态资源以及内嵌脚本/样式之前记录一个时间点，在head最底部记录另一个时间点，两者的差值作为白屏时间
window.performance
DNS查询耗时 = domainLookupEnd - domainLookupStart
TCP链接耗时 = connectEnd - connectStart
request请求耗时 = responseEnd - responseStart
解析dom树耗时 = domComplete - domInteractive
白屏时间 = domloading - fetchStart
domready可操作时间 = domContentLoadedEventEnd - fetchStart
onload总下载时间 = loadEventEnd - fetchStart

域名收敛是什么？
域名收敛：就是将静态资源放在一个域名下。减少DNS解析的开销。

域名发散：是将静态资源放在多个子域名下，就可以多线程下载，提高并行度，使客户端加载静态资源更加迅速。
前端开发中，如何优化图像？图像格式的区别？

文件、脚本合并是如何优化的呢？

浏览器是如何渲染页面的？
https://juejin.im/entry/6844903503609987080
处理 HTML 标记并构建 DOM 树。
处理 CSS 标记并构建 CSSOM 树。
将 DOM 与 CSSOM 合并成一个渲染树。
根据渲染树来布局，以计算每个节点的几何信息。
将各个节点绘制到屏幕上。
这五个步骤并不一定一次性顺序完成
渲染步骤包括样式、布局、绘制，在某些情况下还包括合成。在解析步骤中创建的CSSOM树和DOM树组合成一个Render树，然后用于计算每个可见元素的布局，然后将其绘制到屏幕上。在某些情况下，可以将内容提升到它们自己的层并进行合成，通过在GPU而不是CPU上绘制屏幕的一部分来提高性能，从而释放主线程。
渲染步骤包括样式、布局、绘制，在某些情况下还包括合成。在解析步骤中创建的CSSOM树和DOM树组合成一个Render树，然后用于计算每个可见元素的布局，然后将其绘制到屏幕上。在某些情况下，可以将内容提升到它们自己的层并进行合成，通过在GPU而不是CPU上绘制屏幕的一部分来提高性能，从而释放主线程。
第四步是在渲染树上运行布局以计算每个节点的几何体。布局是确定呈现树中所有节点的宽度、高度和位置，以及确定页面上每个对象的大小和位置的过程。回流是对页面的任何部分或整个文档的任何后续大小和位置的确定。

构建渲染树后，开始布局。渲染树标识显示哪些节点（即使不可见）及其计算样式，但不标识每个节点的尺寸或位置。为了确定每个对象的确切大小和位置，浏览器从渲染树的根开始遍历它。
最后一步是将各个节点绘制到屏幕上，第一次出现的节点称为first meaningful paint。在绘制或光栅化阶段，浏览器将在布局阶段计算的每个框转换为屏幕上的实际像素。

浏览器缓存

浏览器如何知道一个文件资源是否需要缓存？
Cache-Control
前端缓存如何实现？etag和Last-Modified的优先级哪个比较高以及为什么？cache-control和expire优先级哪个比较高以及为什么？
如果两者同时存在，以 Cache-Control 为准。
如果同时设了 ETag 和 Last-Modified，那么必须同时满足条件才会 304，不存在谁更优先就使用谁一说。

accept是什么，怎么用？
Accept代表发送端（客户端）希望接受的数据类型。
比如：Accept：text/xml;
代表客户端希望接受的数据类型是xml类型
XSS脚本劫持，如何截获？

如何绕过过滤
首先尝试大小写
特殊字符
各种事件
特殊编码
当过滤某个标签可以进行编码尝试绕过 假如eval函数应用
xss盲打就是将可能出现xss的地方都插上xss平台上的语句(用来获取cookie后台等敏感信息)
这里我将xss语句都插进去
模拟管理员登录后台

性能优化CSS
内联首屏关键CSS（Critical CSS）
异步加载CSS
去除无用CSS
压缩CSS
1. 有选择地使用选择器
3. 优化重排与重绘
4. 不要使用@import
优化动画，启用GPU硬件加速。
node内存管理
内存泄漏识别
在 Node.js 环境里提供了 process.memoryUsage 方法用来查看当前进程内存使用情况，单位为字节
 垃圾回收会暂停Js的运行， 如果内存过大， 就会导致垃圾回收的时间变长， 从而导致Js暂停的时间过长。
v8是怎么做内存分代的。
内存在服务端本来就是一个寸土寸金的东西，在 V8 中限制 64 位的机器大约 1.4GB，32 位机器大约为 0.7GB。因此，对于一些大内存的操作需谨慎否则超出 V8 内存限制将会造成进程退出。
新生代
v8中的新生代主要存放的是生存周期较短的对象， 它具有两个空间semispace， 分别为From和To， 在分配内存的时候将内存分配给From空间， 当垃圾回收的时候， 会检查From空间存活的对象(广度优先算法)并复制到To空间， 然后清空From空间， 再互相交换From和To空间的位置， 使得To空间变为From空间。
老生代
老生代主要存放的是生存周期比较长的对象。内存按照 1MB 分页，并且都按照 1MB 对齐。新生代的内存页是连续的，而老生代的内存页是分散的，以链表的形式串联起来。 它的内部有4种类型。

新生代（Scavenge算法）
对于新生代的垃圾回收使用Scavenge算法。将堆内存分为两个相等的部分，只有一个处于使用中【称为From空间】，另一个处于闲置的状态【称为To空间】。分配对象时首先在From中分配，开始进行垃圾回收的时候，检查From中的存活的对象，将这些对象复制到To中，并且将From中的非存活的对象的空间释放。完成之后将From和To的角色对换。Scavenge算法只能只用空间的一半，但是因为新生代中的对象的特征就是存活时间短的小对象，所以需要复制的对象占少部分，在这个场景下，Scavenge的时间效率上表现更好（此处暂不进行具体算法层面的分析）。
新生代中的对象在满足一定条件后会被晋升成老对象...也就是移到老生代的堆内存中去。条件主要分为两个：一个是对象是否经历过Scavenge回收，另一个是To空间的内存的比例超过一定的限制。这个比例的设置需要根据具体的场景来设置，不过一般的设置的值是25%。
老生代（Mark-Sweep & Mark-Compact）
老生代的对象存活时间较长，不适合Scavenge算法。为此V8在老生代中主要采用Mark-Sweep和Mark-Compact相结合的方式来进行垃圾回收。
Mark-Sweep的概念很简单，就是遍历空间标记存活的对象，在之后清除的阶段进行清除。这样处理的问题是，垃圾回收处理后的内存的空间会变成不连续的，这会对后续的内存的分配造成浪费。Mark-Compact的作用就是在Mark-Sweep的基础上多了一个合并内存空间的过程。V8在实际处理的时候只有在空间不足的时候使用Mark-Compact。
因为老生代的堆空间比较大，在进行一次完成的老生代的垃圾回收造成的停顿较大，所以V8会将整个过程分为很多小步来完成（incremental marking），垃圾回收和Javascript的逻辑执行交替运行。此外V8还有有一些其他减少停顿时间的方法。

4. 进程是运行在虚拟内存上的吗
否
8. 客户端如何知道服务器传输过来的是最后一段内容
就俩办法，你自己的包里带长度，服务器端首先接收到长度，然后不停的按长度接收，接收满那些长度，则此次发送完毕，或者就是包尾结束符，不停的收，收到结束符则视作此次发送完毕。

9. 迅雷的断点续传了解吗
服务器通过返回的http报文头部字段告知客户端这只是请求数据的一部分，你需要再发起对剩余数据的请求如果浏览器发起的请求中没有带range字段，而服务器没有返回全部数据，而是返回部分数据和一个206的状态码，则浏览器不会发起对剩余字段的请求，此时下载失败。
如果浏览器发起的请求带range字段，服务器返回指定range范围的数据，则下载成功
①断点续传需要在下载过程中记录每条线程的下载进度；
　　②每次下载开始之前先读取数据库，查询是否有未完成的记录，有就继续下载，没有则创建新记录插入数据库；
　　③在每次向文件中写入数据之后，在数据库中更新下载进度；
　　④下载完成之后删除数据库中下载记录。
　　断点续传在HTTP请求上和一般的下载有所不同，客户端浏览器传给Web服务器的时候要多加一条信息——从哪里开始（HTTP请求变量）。要实现HTTP断点续传，Web服务器必须支持HTTP/1.1（相对于HTTP/1.0老版本）。
　　HTTP请求是有一个Header的，里面有个Range属性是定义下载区域的，它接收的值是一个区间范围，比如：Range:bytes=0-10000。这样我们就可以按照一定的规则，将一个大文件拆分为若干很小的部分，然后分批次的下载，每个小块下载完成之后，再合并到文件中；这样即使下载中断了，重新下载时，也可以通过文件的字节长度来判断下载的起始点，然后重启断点续传的过程，直到最后完成下载过程。

10. http如何控制请求的数量，过多请求服务端会应答不上来 请求过多
现代浏览器在与服务器建立了一个 TCP 连接后是否会在一个 HTTP 请求完成后断开？什么情况下会断开？
在 HTTP/1.0 中，一个服务器在发送完一个 HTTP 响应后，会断开 TCP 链接。但是这样每次请求都会重新建立和断开 TCP 连接，代价过大。所以虽然标准中没有设定，某些服务器对 Connection: keep-alive 的 Header 进行了支持。
意思是说，完成这个 HTTP 请求之后，不要断开 HTTP 请求使用的 TCP 连接。这样的好处是连接可以被重新使用，之后发送 HTTP 请求的时候不需要重新建立 TCP 连接，以及如果维持连接，那么 SSL 的开销也可以避免，
初始化连接和 SSL 开销消失了，说明使用的是同一个 TCP 连接。
持久连接：既然维持 TCP 连接好处这么多，HTTP/1.1 就把 Connection 头写进标准，并且默认开启持久连接，除非请求中写明 Connection: close，那么浏览器和服务器之间是会维持一段时间的 TCP 连接，不会一个请求结束就断掉。
所以第一个问题的答案是：默认情况下建立 TCP 连接不会断开，只有在请求报头中声明 Connection: close 才会在请求完成后关闭连接。
一个 TCP 连接可以对应几个 HTTP 请求？
了解了第一个问题之后，其实这个问题已经有了答案，如果维持连接，一个 TCP 连接是可以发送多个 HTTP 请求的。
# 第三个问题
一个 TCP 连接中 HTTP 请求发送可以一起发送么（比如一起发三个请求，再三个响应一起接收）？
HTTP/1.1 存在一个问题，单个 TCP 连接在同一时刻只能处理一个请求，意思是说：两个请求的生命周期不能重叠，任意两个 HTTP 请求从开始到结束的时间在同一个 TCP 连接里不能重叠。
在建立起一个 TCP 连接之后，假设客户端在这个连接连续向服务器发送了几个请求。按照标准，服务器应该按照收到请求的顺序返回结果，假设服务器在处理首个请求时花费了大量时间，那么后面所有的请求都需要等着首个请求结束才能响应。
为什么有的时候刷新页面不需要重新建立 SSL 连接？
在第一个问题的讨论中已经有答案了，TCP 连接有的时候会被浏览器和服务端维持一段时间，TCP 不需要重新建立，SSL 自然也会用之前的。
浏览器对同一 Host 建立 TCP 连接到数量有没有限制？
假设我们还处在 HTTP/1.1 时代，那个时候没有多路传输，当浏览器拿到一个有几十张图片的网页该怎么办呢？肯定不能只开一个 TCP 连接顺序下载，那样用户肯定等的很难受，但是如果每个图片都开一个 TCP 连接发 HTTP 请求，那电脑或者服务器都可能受不了，要是有 1000 张图片的话总不能开 1000 个TCP 连接吧，你的电脑同意 NAT 也不一定会同意。
所以答案是：有。Chrome 最多允许对同一个 Host 建立六个 TCP 连接。不同的浏览器有一些区别。

11. 拥塞控制是如何做的
什么是流量控制？流量控制的目的？
如果发送者发送数据过快，接收者来不及接收，那么就会有分组丢失。为了避免分组丢失，控制发送者的发送速度，使得接收者来得及接收，这就是流量控制。流量控制根本目的是防止分组丢失，它是构成TCP可靠性的一方面。

如何实现流量控制？
由滑动窗口协议（连续ARQ协议）实现。滑动窗口协议既保证了分组无差错、有序接收，也实现了流量控制。主要的方式就是接收方返回的 ACK 中会包含自己的接收窗口的大小，并且利用大小来控制发送方的数据发送。
流量控制引发的死锁？怎么避免死锁的发生？
当发送者收到了一个窗口为0的应答，发送者便停止发送，等待接收者的下一个应答。但是如果这个窗口不为0的应答在传输过程丢失，发送者一直等待下去，而接收者以为发送者已经收到该应答，等待接收新数据，这样双方就相互等待，从而产生死锁。
为了避免流量控制引发的死锁，TCP使用了持续计时器。每当发送者收到一个零窗口的应答后就启动该计时器。时间一到便主动发送报文询问接收者的窗口大小。若接收者仍然返回零窗口，则重置该计时器继续等待；若窗口不为0，则表示应答报文丢失了，此时重置发送窗口后开始发送，这样就避免了死锁的产生。
拥塞控制：拥塞控制是作用于网络的，它是防止过多的数据注入到网络中，避免出现网络负载过大的情况；常用的方法就是：（ 1 ）慢开始、拥塞避免（ 2 ）快重传、快恢复。

流量控制：流量控制是作用于接收者的，它是控制发送者的发送速度从而使接收者来得及接收，防止分组丢失的。

15. 如果有一个非常大的数组，要在这个数组当中找到某个值，有什么方法
find,findindex
babel转换代码的过程主要为三步：
解析 使用babylon这个解析器，它会根据输入的javascript代码字符串根据ESTree规范生成AST（抽象语法树）。
转换 根据一定的规则转换、修改AST。
生成 使用babel-generator将修改后的AST转换成普通代码。

vue项目实现按需加载的3种方式：vue异步组件、es提案的import()、webpack的require.ensure()
vue异步组件技术
vue-router配置路由，使用vue的异步组件技术，可以实现按需加载（懒加载）。 但是，这种情况下一个组件生成一个js文件。
es提案的import()
推荐使用这种方式(需要webpack > 2.4)
webpack官方文档：webpack中使用import() vue官方文档：路由懒加载使用import()
webpack提供的require.ensure()
vue-router配置路由，使用webpack的require.ensure技术，也可以实现按需加载。 这种情况下，多个路由指定相同的chunkName，会合并打包成一个js文件。

什么是http
http（超文本传输协议）是一个基于请求与响应模式的、无状态的、应用层的协议，常基于TCP的连接方式，HTTP1.1版本中给出一种持续连接的机制，绝大多数的Web开发，都是构建在HTTP协议之上的Web应用。
  http请求由三部分组成，分别是：请求行、消息报头、请求正文

1. 请求行以一个方法符号开头，以空格分开，后面跟着请求的URI和协议的版本，格式如下：Method Request-URI HTTP-Version CRLF
其中 Method表示请求方法；Request-URI是一个统一资源标识符；HTTP-Version表示请求的HTTP协议版本；CRLF表示回车和换行（除了作为结尾的CRLF外，不允许出现单独的CR或LF字符）。

请求方法（所有方法全为大写）有多种，各个方法的解释如下：
GET     请求获取Request-URI所标识的资源
POST    在Request-URI所标识的资源后附加新的数据
HEAD    请求获取由Request-URI所标识的资源的响应消息报头
PUT     请求服务器存储一个资源，并用Request-URI作为其标识
DELETE  请求服务器删除Request-URI所标识的资源
TRACE   请求服务器回送收到的请求信息，主要用于测试或诊断
CONNECT 保留将来使用
OPTIONS 请求查询服务器的性能，或者查询与资源相关的选项和需求
HTTP响应也是由三个部分组成，分别是：状态行、消息报头、响应正文
1. 状态行格式如下：
HTTP-Version Status-Code Reason-Phrase CRLF
其中，HTTP-Version表示服务器HTTP协议的版本；Status-Code表示服务器发回的响应状态代码；Reason-Phrase表示状态代码的文本描述。
状态代码有三位数字组成，第一个数字定义了响应的类别，且有五种可能取值：
1xx：指示信息--表示请求已接收，继续处理
2xx：成功--表示请求已被成功接收、理解、接受
3xx：重定向--要完成请求必须进行更进一步的操作
4xx：客户端错误--请求有语法错误或请求无法实现
5xx：服务器端错误--服务器未能实现合法的请求
常见状态代码、状态描述、说明：
200 OK      //客户端请求成功
400 Bad Request  //客户端请求有语法错误，不能被服务器所理解
401 Unauthorized //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
403 Forbidden  //服务器收到请求，但是拒绝提供服务
404 Not Found  //请求资源不存在，eg：输入了错误的URL
500 Internal Server Error //服务器发生不可预期的错误
503 Server Unavailable  //服务器当前不能处理客户端的请求，一段时间后可能恢复正常

3. 响应正文就是服务器返回的资源的内容
Cache-Control   用于指定缓存指令，缓存指令是单向的（响应中出现的缓存指令在请求中未必会出现），且是独立的（一个消息的缓存指令不会影响另一个消息处理的缓存机制），HTTP1.0使用的类似的报头域为Pragma。
请求时的缓存指令包括：no-cache（用于指示请求或响应消息不能缓存）、no-store、max-age、max-stale、min-fresh、only-if-cached;
响应时的缓存指令包括：public、private、no-cache、no-store、no-transform、must-revalidate、proxy-revalidate、max-age、s-maxage.
Cache-Control:Public 可以被任何缓存所缓存
Cache-Control:Private 内容只缓存到私有缓存中
Cache-Control:no-cache 所有内容都不会被缓存
Cache-Control:no-store 用于防止重要的信息被无意的发布。在请求消息中发送将使得请求和响应消息都不使用缓存。
Cache-Control:max-age 指示客户机可以接收生存期不大于指定时间（以秒为单位）的响应。
Cache-Control:min-fresh 指示客户机可以接收响应时间小于当前时间加上指定时间的响应。
Cache-Control:max-stale 指示客户机可以接收超出超时期间的响应消息。如果指定max-stale消息的值，那么客户机可以接收超出超时期指定值之内的响应消息。
Accept
Accept请求报头域用于指定客户端接受哪些类型的信息。eg：Accept：image/gif，表明客户端希望接受GIF图象格式的资源；Accept：text/html，表明客户端希望接受html文本。
Accept-Charset
Accept-Charset请求报头域用于指定客户端接受的字符集。eg：Accept-Charset:iso-8859-1,gb2312.如果在请求消息中没有设置这个域，缺省是任何字符集都可以接受。
Accept-Encoding
Accept-Encoding请求报头域类似于Accept，但是它是用于指定可接受的内容编码。eg：Accept-Encoding:gzip.deflate.如果请求消息中没有设置这个域服务器假定客户端对各种内容编码都可以接受。
Accept-Language
Accept-Language请求报头域类似于Accept，但是它是用于指定一种自然语言。eg：Accept-Language:zh-cn.如果请求消息中没有设置这个报头域，服务器假定客户端对各种语言都可以接受。
Authorization
Authorization请求报头域主要用于证明客户端有权查看某个资源。当浏览器访问一个页面时，如果收到服务器的响应代码为401（未授权），可以发送一个包含Authorization请求报头域的请求，要求服务器对其进行验证。
Host（发送请求时，该报头域是必需的）
Host请求报头域主要用于指定被请求资源的Internet主机和端口号，它通常从HTTP URL中提取出来的，
User-Agent：告诉HTTP服务器，客户端使用的操作系统和浏览器的名称和版本。
Referer：包含一个URL，用户从该URL代表的页面出发访问当前请求的页面。
响应报头允许服务器传递不能放在状态行中的附加响应信息，以及关于服务器的信息和对Request-URI所标识的资源进行下一步访问的信息。
常用的响应报头
Location
Location响应报头域用于重定向接受者到一个新的位置。Location响应报头域常用在更换域名的时候。
Server
Server响应报头域包含了服务器用来处理请求的软件信息。与User-Agent请求报头域是相对应的。下面是
Server响应报头域的一个例子：
Server：Apache-Coyote/1.1
WWW-Authenticate
WWW-Authenticate响应报头域必须被包含在401（未授权的）响应消息中，客户端收到401响应消息时候，并发送Authorization报头域请求服务器对其进行验证时，服务端响应报头就包含该报头域。

Session的实现方式：
1. 使用Cookie来实现
服务器给每个Session分配一个唯一的JSESSIONID，并通过Cookie发送给客户端。
当客户端发起新的请求的时候，将在Cookie头中携带这个JSESSIONID。这样服务器能够找到这个客户端对应的Session。
2. 使用URL回写来实现
URL回写是指服务器在发送给浏览器页面的所有链接中都携带JSESSIONID的参数，这样客户端点击任何一个链接都会把JSESSIONID带会服务器。如果直接在浏览器输入服务端资源的url来请求该资源，那么Session是匹配不到的。
Cookie和Session有以下明显的不同点：
1. Cookie将状态保存在客户端，Session将状态保存在服务器端；
2. Cookies是服务器在本地机器上存储的小段文本并随每一个请求发送至同一个服务器。Cookie最早在RFC2109中实现，后续RFC2965做了增强。网络服务器用HTTP头向客户端发送cookies，在客户终端，浏览器解析这些cookies并将它们保存为一个本地文件，它会自动将同一服务器的任何请求缚上这些cookies。Session并没有在HTTP的协议中定义；
3. Session是针对每一个用户的，变量的值保存在服务器上，用一个sessionID来区分是哪个用户session变量,这个值是通过用户的浏览器在访问的时候返回给服务器，当客户禁用cookie时，这个值也可能设置为由get来返回给服务器；
4. 就安全性来说：当你访问一个使用session 的站点，同时在自己机子上建立一个cookie，建议在服务器端的SESSION机制更安全些。因为它不会任意读取客户存储的信息。

断点续传的实现原理
HTTP协议的GET方法，支持只请求某个资源的某一部分；
206 Partial Content 部分内容响应；
Range 请求的资源范围；
Content-Range 响应的资源范围；
在连接断开重连时，客户端只请求该资源未下载的部分，而不是重新请求整个资源，来实现断点续传。
分块请求资源实例：
Eg1：Range: bytes=306302- ：请求这个资源从306302个字节到末尾的部分；
Eg2：Content-Range: bytes 306302-604047/604048：响应中指示携带的是该资源的第306302-604047的字节，该资源共604048个字节；
客户端通过并发的请求相同资源的不同片段，来实现对某个资源的并发分块下载。从而达到快速下载的目的。目前流行的FlashGet和迅雷基本都是这个原理。

11.2、多线程下载的原理
下载工具开启多个发出HTTP请求的线程；
每个http请求只请求资源文件的一部分：Content-Range: bytes 20000-40000/47000；
合并每个线程下载的文件。

1.长连接
Client方与Server方先建立通讯连接，连接建立后 不断开， 然后再进行报文发送和接收。
2.短连接
Client方与Server每进行一次报文收发交易时才进行通讯连接，交易完毕后立即断开连接。此种方式常用于一点对多点通讯，比如多个Client连接一个Server.

（1）TCP为了保证可靠传输，尽量减少额外开销（每次发包都要验证），因此采用了流式传输，面向流的传输，相对于面向消息的传输，可以减少发送包的数量，从而减少了额外开销。但是，对于数据传输频繁的程序来讲，使用TCP可能会容易粘包。当然，对接收端的程序来讲，如果机器负荷很重，也会在接收缓冲里粘包。这样，就需要接收端额外拆包，增加了工作量。因此，这个特别适合的是数据要求可靠传输，但是不需要太频繁传输的场合（两次操作间隔100ms，具体是由TCP等待发送间隔决定的，取决于内核中的socket的写法）
（2）UDP，由于面向的是消息传输，它把所有接收到的消息都挂接到缓冲区的接受队列中，因此，它对于数据的提取分离就更加方便，但是，它没有粘包机制，因此，当发送数据量较小的时候，就会发生数据包有效载荷较小的情况，也会增加多次发送的系统发送开销（系统调用，写硬件等）和接收开销。因此，应该最好设置一个比较合适的数据包的包长，来进行UDP数据的发送。（UDP最大载荷为1472，因此最好能每次传输接近这个数的数据量，这特别适合于视频，音频等大块数据的发送，同时，通过减少握手来保证流媒体的实时性）

粘包出现原因
简单得说，在流传输中出现，UDP不会出现粘包，因为它有消息边界(参考Windows网络编程)
1发送端需要等缓冲区满才发送出去，造成粘包
2接收方不及时接收缓冲区的包，造成多个包接收
具体点：
（1）发送方引起的粘包是由TCP协议本身造成的，TCP为提高传输效率，发送方往往要收集到足够多的数据后才发送一包数据。若连续几次发送的数据都很少，通常TCP会根据优化算法把这些数据合成一包后一次发送出去，这样接收方就收到了粘包数据。
（2）接收方引起的粘包是由于接收方用户进程不及时接收数据，从而导致粘包现象。这是因为接收方先把收到的数据放在系统接收缓冲区，用户进程从该缓冲区取数据，若下一包数据到达时前一包数据尚未被用户进程取走，则下一包数据放到系统接收缓冲区时就接到前一包数据之后，而用户进程根据预先设定的缓冲区大小从系统接收缓冲区取数据，这样就一次取到了多包数据。
粘包情况有两种，一种是粘在一起的包都是完整的数据包，另一种情况是粘在一起的包有不完整的包。
不是所有的粘包现象都需要处理，若传输的数据为不带结构的连续流数据（如文件传输），则不必把粘连的包分开（简称分包）。但在实际工程应用中，传输的数据一般为带结构的数据，这时就需要做分包处理。
在处理定长结构数据的粘包问题时，分包算法比较简单；在处理不定长结构数据的粘包问题时，分包算法就比较复杂。特别是粘在一起的包有不完整的包的粘包情况，由于一包数据内容被分在了两个连续的接收包中，处理起来难度较大。实际工程应用中应尽量避免出现粘包现象。

为了避免粘包现象，可采取以下几种措施：
（1）对于发送方引起的粘包现象，用户可通过编程设置来避免，TCP提供了强制数据立即传送的操作指令push，TCP软件收到该操作指令后，就立即将本段数据发送出去，而不必等待发送缓冲区满；
（2）对于接收方引起的粘包，则可通过优化程序设计、精简接收进程工作量、提高接收进程优先级等措施，使其及时接收数据，从而尽量避免出现粘包现象；
（3）由接收方控制，将一包数据按结构字段，人为控制分多次接收，然后合并，通过这种手段来避免粘包。

TCP协议和UDP协议特性区别总结： TCPUDP
1. TCP协议在传送数据段的时候要给段标号；UDP协议不
2. TCP协议可靠；UDP协议不可靠
3. TCP协议是面向连接；UDP协议采用无连接
4. TCP协议负载较高，采用虚电路；UDP采用无连接
5. TCP协议的发送方要确认接收方是否收到数据段（3次握手协议）
6. TCP协议采用窗口技术和流控制
7. TCP面向字节流，UDP面向用户数据报

Vuex 是什么？
官方解释：Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

vuex其实就是用一个全局的变量保存了Vue项目中的所有的公共数据，类似与在前端这块放了一个数据库，大家都可以在这里存数据，删数据，改数据，读数据，是不是有点熟悉：增，删，改，查；不过这个全局的变量给他定义了一个固定的名字就叫：store（仓库），是不是很形象，而这个仓库里面装数据的袋子就是state，加工数据的机器就叫做：mutations，操作机器的工人就叫做：actions，把数据装起来取走的卡车就叫做：getters;
store就是组件中的共享状态

封装axios
class Axios{
    constructor(){
        const _this = this;
        return new Proxy(function(){},{
            apply(fn,thisArg,agrs){

            },
            get(fn,key){
                return _this[key];
            },
            set(fn,key,val){
                _this[key] = val;
                return true;
            }
        })
    }
    get(){
        console.log('get request');
    }
}

export default new Axios();
get(...agrs){
    let options;
    if(agrs.length===1 && typeof agrs[0] ==='string'){//axois.get(url)
        options = {
            method:'get',
            url:agrs[0]
        }

    }else if(agrs.length === 1 && agrs[0] instanceof Object){//axios.get({url,params:{},headers:{}})
        options = {
            ...agrs[0],
            method:'get'
        }
    }else if(agrs.length === 2 && typeof agrs[0] ==='string'){//axios.get(url,{params:{},headers:{}})
        options={
            method:'get',
            url:agrs[0],
            ...agrs[1]
        }
    }else{
        assert(false,`arguments invalidate!`)
    }
    console.log('get options:',options);
}


var Axios = {
        get: function(url) {
            return new Promise((resolve, reject) => {
                var xhr = new XMLHttpRequest();
                xhr.open('GET', url, true);
                xhr.onreadystatechange = function() {
                    // readyState == 4说明请求已完成
                    if (xhr.readyState == 4 && xhr.status == 200) {
                        // 从服务器获得数据
                        resolve(xhr.responseText)
                    }
                };
                xhr.send();
            })
        },
    }

比较版本号
arr.sort((a, b) => {
    const listA = a.split('.');
    const listB = b.split('.');
    let k = undefined;

    for (let i in listA) {
        let itemA = Number(listA[i] || 0);
        let itemB = Number(listB[i] || 0);

        if (itemA === itemB) {
            continue;
        } else {
            k = Number(itemA) - Number(itemB);
            break;
        }
    }
    if (k === undefined) {
        k = listA.length - listB.length;
    }
    return k;
});
box-sizing：box-sizing
ES6中for…of与for…in的区别 forof forin
1. for…of循环可以代替数组实例的forEach方法，不同于forEach方法，它可以与break、continue和return配合使用。
2. for…in循环主要是为遍历对象而设计的，不适用于遍历数组。
3. JavaScript 原有的for…in循环，只能获得对象的键名，不能直接获取键值。ES6 提供for…of循环，允许遍历获得键值。
4. for…of循环调用遍历器接口，数组的遍历器接口只返回具有数字索引的属性。这一点跟for…in循环也不一样。
5. 对于普通的对象，for…of结构不能直接使用，会报错，必须部署了 Iterator 接口后才能使用。但是，这样情况下，for…in循环依然可以用来遍历键名。对于字符串来说，for…of循环还有一个特点，就是会正确识别 32 位 UTF-16 字符。for…of循环可用于字符串、DOM NodeList 对象、arguments对象。
6. 并不是所有类似数组的对象都具有 Iterator 接口，一个简便的解决方法，就是使用Array.from方法将其转为数组。
7. 对于普通的对象，for…in循环可以遍历键名，for…of循环会报错。
8. 一种解决方法是，使用Object.keys方法将对象的键名生成一个数组，然后遍历这个数组。
9. 另一个方法是使用 Generator 函数将对象重新包装一下。


反问：需要哪些东西充实自己：可以写几个页面做一下移动端，可以封装几个组件
洗牌算法
function shuffle(arr){
    var result = [],
        random;
    while(arr.length>0){
        random = Math.floor(Math.random() * arr.length);
        result.push(arr[random])
        arr.splice(random, 1)
    }
    return result;
}
function shuffle(arr){
    var length = arr.length,
        temp,
        random;
    while(0 != length){
        random = Math.floor(Math.random() * length)
        length--;
        // swap
        temp = arr[length];
        arr[length] = arr[random];
        arr[random] = temp;
    }
    return arr;
}
[1,2,3,4,5,6].sort(function(){
    return .5 - Math.random();
})
/**
 * Fisher–Yates shuffle
 */
Array.prototype.shuffle = function() {
    var input = this;

    for (var i = input.length-1; i >=0; i--) {

        var randomIndex = Math.floor(Math.random()*(i+1));
        var itemAtIndex = input[randomIndex];

        input[randomIndex] = input[i];
        input[i] = itemAtIndex;
    }
    return input;
}
二叉树后序遍历
var postorderTraversal = function(root) {
    // 调用栈
    const call = []
    const res = []
    if (root !== null) call.push(root)
    while (call.length) {
        const t = call.pop()
        if (t !== null) {
            call.push(t)
            call.push(null)
            if (t.right) call.push(t.right)
            if (t.left) call.push(t.left)
        } else {
            res.push(call.pop().val)
        }
    }

    return res
};

什么是BFC
BFC是 (Block Formatting context)的简称，即块格式化上下文。可以理解为它是运用一些渲染规则的块渲染区域，它是css世界中的结界。为何说是结界，因为在触发了 BFC 特性的容器下元素和容器外部元素完全隔离，子元素的布局不会影响外部元素，反之依然。
BFC 元素有如下一些特征：
BFC的块不会和浮动块重叠
计算BFC元素的高度时，会包括浮动元素
在一个BFC下的块 margin 会发生重叠，不在同一个则不会
BFC元素是一个独立的容器，使得里面的元素和外部元素隔离开，互补影响
触发BFC
通过以下设置可以触发一个块元素的BFC特性：
float 的值不为 none
overflow 的值为 auto, scroll和 hidde
display 的值为 table-cell, table-caption和 inline-block
position 设置为 absolute和 fixed

移动端1px
https://juejin.im/post/6844903877947424782
border:0.5px solid #E5E5E5
使用边框图片
border: 1px solid transparent;
border-image: url('./../../image/96.jpg') 2 repeat;
使用box-shadow实现
box-shadow: 0  -1px 1px -1px #e5e5e5,   //上边线
            1px  0  1px -1px #e5e5e5,   //右边线
            0  1px  1px -1px #e5e5e5,   //下边线
            -1px 0  1px -1px #e5e5e5;   //左边线
伪元素
.setOnePx{
  position: relative;
  &::after{
    position: absolute;
    content: '';
    background-color: #e5e5e5;
    display: block;
    width: 100%;
    height: 1px; /*no*/
    transform: scale(1, 0.5);
    top: 0;
    left: 0;
  }
}

.setBorderAll{
     position: relative;
       &:after{
           content:" ";
           position:absolute;
           top: 0;
           left: 0;
           width: 200%;
           height: 200%;
           transform: scale(0.5);
           transform-origin: left top;
           box-sizing: border-box;
           border: 1px solid #E5E5E5;
           border-radius: 4px;
      }
    }

介绍pm2
PM2是node进程管理工具，可以利用它来简化很多node应用管理的繁琐任务，如性能监控、自动重启、负载均衡等，而且使用非常简单
介绍路由的history
History 对象最初设计来表示窗口的浏览历史。但出于隐私方面的原因，History 对象不再允许脚本访问已经访问过的实际 URL。唯一保持使用的功能只有 back()、forward() 和 go() 方法。
window.history.pushState(stateObject,title,url )

将当前URL和history.state加入到history中，并用新的state和URL替换当前，不会造成页面刷新。
--参数解释
stateObject    //与要跳转到的URL对应的状态信息，没有特殊的情况下可以直接传{}
title       //现在大多数浏览器不支持或者忽略这个参数，我们在用的时候建议传一个空字符串
url            //这个参数提供了新历史纪录的地址,它不一定要是绝对地址，也可以是相对的，不可跨域
window.history.replaceState(stateObject,title,url)

用新的state和URL替换当前，不会造成页面刷新。
--参数解释
stateObject    //与要跳转到的URL对应的状态信息，没有特殊的情况下可以直接传{}
title       //现在大多数浏览器不支持或者忽略这个参数，我们在用的时候建议传一个空字符串
url            //这个参数提供了新历史纪录的地址,它不一定要是绝对地址，也可以是相对的，不可跨域
执行完之后，我们发现不能回退了，是不是就跟window.location.replace()实现同样的效果了

多个组件之间如何拆分各自的state，每块小的组件有自己的状态，它们之间还有一些公共的状态需要维护，如何思考这块
状态提升，找到容器组件和展示组件，保证唯一数据源和单向数据
对于组件的拆分还要做到高内聚低耦合

渲染进程是如何工作的
主线程 Main thread
工作线程 Worker thread
排版线程 Compositor thread
光栅线程 Raster thread
构建 DOM
当渲染进程接收到导航的确认信息，开始接受 HTML 数据时，主线程会解析文本字符串为 DOM。
渲染 html 为 DOM 的方法由 HTML Standard 定义。
加载次级的资源
网页中常常包含诸如图片，CSS，JS 等额外的资源，这些资源需要从网络上或者 cache 中获取。主进程可以在构建 DOM 的过程中会逐一请求它们，为了加速 preload scanner 会同时运行，如果在 html 中存在 <img> <link> 等标签，preload scanner 会把这些请求传递给 Browser process 中的 network thread 进行相关资源的下载。

3.JS 的下载与执行
当遇到 <script> 标签时，渲染进程会停止解析 HTML，而去加载，解析和执行 JS 代码，停止解析 html 的原因在于 JS 可能会改变 DOM 的结构（使用诸如 documwnt.write()等 API）。
不过开发者其实也有多种方式来告知浏览器应对如何应对某个资源，比如说如果在<script> 标签上添加了 async 或 defer 等属性，浏览器会异步的加载和执行 JS 代码，而不会阻塞渲染。更多的方法可参考 Resource Prioritization – Getting the Browser to Help You。
样式计算
仅仅渲染 DOM 还不足以获知页面的具体样式，主进程还会基于 CSS 选择器解析 CSS 获取每一个节点的最终的计算样式值。即使不提供任何 CSS，浏览器对每个元素也会有一个默认的样式。
获取布局
想要渲染一个完整的页面，除了获知每个节点的具体样式，还需要获知每一个节点在页面上的位置，布局其实是找到所有元素的几何关系的过程。其具体过程如下：
通过遍历 DOM 及相关元素的计算样式，主线程会构建出包含每个元素的坐标信息及盒子大小的布局树。布局树和 DOM 树类似，但是其中只包含页面可见的元素，如果一个元素设置了 display:none ，这个元素不会出现在布局树上，伪元素虽然在 DOM 树上不可见，但是在布局树上是可见的。
绘制各元素
即使知道了不同元素的位置及样式信息，我们还需要知道不同元素的绘制先后顺序才能正确绘制出整个页面。在绘制阶段，主线程会遍历布局树以创建绘制记录。绘制记录可以看做是记录各元素绘制先后顺序的笔记。
主线程依据布局树构建绘制记录
合成帧
熟悉 PS 等绘图软件的童鞋肯定对图层这一概念不陌生，现代 Chrome 其实利用了这一概念来组合不同的层。
复合是一种分割页面为不同的层，并单独栅格化，随后组合为帧的技术。不同层的组合由 compositor 线程（合成器线程）完成。
主线程会遍历布局树来创建层树（layer tree），添加了 will-change CSS 属性的元素，会被看做单独的一层。
事件的优化
一般我们屏幕的刷新速率为 60fps，但是某些事件的触发量会不止这个值，出于优化的目的，Chrome 会合并连续的事件 (如 wheel, mousewheel, mousemove, pointermove, touchmove )，并延迟到下一帧渲染时候执行 。
而如 keydown, keyup, mouseup, mousedown, touchstart, 和 touchend 等非连续性事件则会立即被触发。

表单可以跨域吗？
form表单是可以跨域的。

浏览器遵从同源策略，限制ajax跨域的原因在于ajax网络请求是可以携带cookie的（通过设置withCredentials为true），比如用户打开了浏览器，登录了weibo.com，然后又打开了百度首页，这时候百度首页内的js，向weibo.com用withCredentials为true的ajax方式提交一个post请求，是会携带浏览器和weibo.com之间的cookie的，所以浏览器就默认禁止了ajax跨域，服务端必须设置CORS才可以。

而form提交是不会携带cookie的，你也没办法设置一个hidden的表单项，然后通过js拿到其他domain的cookie，因为cookie是基于域的，无法访问其他域的cookie，所以浏览器认为form提交到某个域，是无法利用浏览器和这个域之间建立的cookie和cookie中的session的，故而，浏览器没有限制表单提交的跨域问题。

将输入的数组组装成一颗树状的数据结构
 function getTreeData(arr) {
    if (!arr || !(arr instanceof Array)) return '错误的数据类型'
    if (!arr.length) return '空数组'
    var len = arr.length
    var rootObj = {id: null, name: null, children: []}
    var nodeObj = {}
    for (var i = 0;i < len; i++) {
      if (!arr[i].parentId) {
        rootObj = {
          id: arr[i].id,
          name: arr[i].name,
          children: [],
        }
      } else {
        if (nodeObj.hasOwnProperty(arr[i].parentId)) {
          nodeObj[arr[i].parentId].children.push(arr[i])
        } else {
          nodeObj[arr[i].parentId] = {}
          nodeObj[arr[i].parentId].children = []
          nodeObj[arr[i].parentId].children.push(arr[i])
        }
      }
    }
    // 整理根节点过程
    function getChildren(node) {
      if(nodeObj[node.id] && nodeObj[node.id].children){
          node.children = nodeObj[node.id].children
          delete(nodeObj[node.id])
          var len = node.children.length
          if (len > 0) {
              for (var i = 0; i < len; i++) {
                 getChildren(node.children[i])
              }
     }
      } else if(!nodeObj[node.id]){
        console.log(node.id + '没有children')
    }
  }
    getChildren(rootObj)
   for(var p in nodeObj){
     if(nodeObj.hasOwnProperty){
       console.warn(p + ':没有该父节点')
     }
   }
    return rootObj
  }

Generator
Generator是es6提出的另一种异步编程解决方案，需要在函数名之前加一个*号，函数内部使用yield语句。Generaotr函数会返回一个遍历器，可以进行遍历操作执行每个中断点yield。
优点：没有了Promise的一堆then(),异步操作更像同步操作，代码更加清晰。
缺点：不能自动执行异步操作，需要写多个next()方法，需要配合使用Thunk函数和Co模块才能做到自动执行。

Promise
Promise是es6提出的异步编程的一种解决方案。
Promise 对象有三种状态：

pending: 初始状态，既不是成功，也不是失败状态。
fulfilled: 意味着操作成功完成。
rejected: 意味着操作失败。
promise的状态只能从pending变成fulfilled，和pending变成rejected，状态一旦改变，就不会再改变，且只有异步操作的结果才能改变promise的状态。
优点：解决了回调地狱的问题，将异步操作以同步操作的流程表达出来。
缺点：无法取消promise。如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。当处于Pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。当执行多个Promise时，一堆then看起来也很不友好。

手写parsedom
var items = [
  { name: 'item1' },
  { name: 'item2' }
];

// ***********
const s = {}
s.items = items
function ParseDom(str) {
  // 借助dom子节点使用dom方法
  const mid = document.createElement('div')
  mid.innerHTML = str
  const { children } = mid
  let res = ''
  // 遍历子节点
  ;[...children].forEach(c => {
    // 找属性节点
    const attrs = [...c.attributes]
    const targetAttr = attrs.find(x => x.name === 'ali-for');
    const nodename = c.nodeName.toLocaleLowerCase();
    // 属性全部写进去
    const attrsStr = attrs.reduce((r, c) => {
      if (c.name !== 'ali-for') {
        r += ` ${c.name}="${c.value}"`
      }
      return r
    }, '')
    if (!targetAttr) {
      // 没有循环渲染标记
      res += `<${nodename}${attrsStr}>${c.innerHTML}</${nodename}>`
      return
    }
    // 循环渲染
    const vfor = targetAttr.nodeValue
    const o = vfor.split(' in ')[1]
    const k = c.innerText.match(/\{\{(.*)\}\}/)[1].split('.')[1]
    ;s[o].forEach(x => {
      res += `<${nodename}${attrsStr}>${x[k]}</${nodename}>`
    })
  })
  return res
}
// ***********

var str = '<div ali-for="item in items">{{item.name}}</div>';
// 对应生成的dom
ParseDom(str);

Async/await 和 Promises 区别
在函数前有一个关键字async，await关键字只能在使用async定义的函数中使用。
任何一个async函数都会隐式返回一个promise，并且promise resolve 的值就是 return 返回的值 (例子中是”done”)
不能在函数开头使用await
promise是ES6，async/await是ES7
2 async/await相对于promise来讲，写法更加优雅
3 reject状态：
    1）promise错误可以通过catch来捕捉，建议尾部捕获错误，
    2）async/await既可以用.then又可以用try-catch捕捉

观察者和订阅-发布的区别，各⾃自⽤用在哪⾥里里
发布订阅模式相比观察者模式多了个事件通道，事件通道作为调度中心，管理事件的订阅和发布工作，彻底隔绝了订阅者和发布者的依赖关系。即订阅者在订阅事件的时候，只关注事件本身，而不关心谁会发布这个事件；发布者在发布事件的时候，只关注事件本身，而不关心谁订阅了这个事件。

观察者模式有两个重要的角色，即目标和观察者。在目标和观察者之间是没有事件通道的。一方面，观察者要想订阅目标事件，由于没有事件通道，因此必须将自己添加到目标(Subject) 中进行管理；另一方面，目标在触发事件的时候，也无法将通知操作(notify) 委托给事件通道，因此只能亲自去通知所有的观察者。
发布-订阅模式是面向调度中心编程的，而观察者模式则是面向目标和观察者编程的。前者用于解耦发布者和订阅者，后者用于耦合目标和观察者，

通过什什么做到并发请求
promise.all
import { Button } from 'antd'，打包的时候只打包button，
分模块加载，是怎么做到的
通过 babel-plugin-import 配置处理。
{
  "plugins": [
    ["import", {
      "libraryName": "antd",
      "libraryDirectory": "es",
      "style": "css"
    }]
  ]
}

Http报⽂文的请求会有⼏几个部分
HTTP请求/响应报文结构HTTP请求报文一个HTTP请求报文由四个部分组成：请求行、请求头部、空行、请求数据

cookie和token都存放在header⾥里里⾯面，为什什么只劫持前者
cookie
举例：服务员看你的身份证，给你一个编号，以后，进行任何操作，都出示编号后服务员去看查你是谁。
token
举例：直接给服务员看自己身份证
1攻击者通过xss拿到用户的cookie然后就可以伪造cookie了。
2或者通过csrf在同个浏览器下面通过浏览器会自动带上cookie的特性
在通过 用户网站-攻击者网站-攻击者请求用户网站的方式 浏览器会自动带上cookie
但是token
1 不会被浏览器带上 问题2 解决
2 token是放在jwt里面下发给客户端的 而且不一定存储在哪里 不能通过document.cookie直接拿到，通过jwt+ip的方式 可以防止 被劫持 即使被劫持 也是无效的jwt

key主要是解决哪一类的问题，为什么不建议用索引index（重绘）
key是React用来识别DOM元素的唯一东西。如果你将一个项目推入列表或删除中间的东西会发生什么？如果key与之前的相同，则React假定DOM元素表示与以前相同的组件，但这是不对的。
index作为key，其实就等于不加key
2. index作为key，只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出

topK
可以取数组的前 K 位构建一个小顶堆，这么堆顶就是前 K 位最小的值，然后从 K+1 遍历数组，如果小于堆顶，则将其交换，并重新构建堆，使堆顶最小，这么遍历结束后，堆就是最大的 K 位，堆顶是前 K 位的最小值。

/**
 * 小顶堆叶子节点排序
 * @param {number[]} arr - 堆
 * @param {number} i = 父节点
 * @param {length} i - 堆大小
 */
const heapify = (arr, i, length) => {
  const left = 2 * i + 1; // 左孩子节点
  const right = 2 * i + 2; // 右孩子节点
  let minimum = i; // 假设最小的节点为父结点

  // 确定三个节点的最小节点
  if (left < length && arr[left] < arr[minimum]) {
    minimum = left;
  }

  if (right < length && arr[right] < arr[minimum]) {
    minimum = right;
  }

  // 如果父节点不是最小节点
  if (minimum !== i) {
    // 最小节点和父节点交换
    const tmp = arr[minimum];
    arr[minimum] = arr[i];
    arr[i] = tmp;

    // 对调整的结点做同样的交换
    heapify(arr, minimum, length);
  }
}

/**
 * 构建小顶堆
 * 从 n/2 个节点开始，依次构建堆，直到第一个节点
 *
 * @param {number[]} arr
 */
const buildMinHeap = (arr) => {
  for (let i = Math.floor(arr.length / 2); i >= 0; i--) {
    heapify(arr, i, arr.length)
  }
  return arr;
}

/**·
 * 查找前 K 个最大的元素
 *
 * @param {number[]} arr - 要查询的数组
 * @param {number} k - 最大个数
 *
 * @return {number[]}
 */
const findKMax = (arr, k) => {
  // 取数组的前 K 位构建小顶堆
  const newArr = [...arr];
  const kMax = arr.slice(0, k)
  buildMinHeap(kMax);

  // 堆后面的进行遍历，如果比堆顶大，则交换并重新构建堆
  for (let i = k; i < newArr.length; i++) {
    if (newArr[i] > kMax[0]) {
      const tmp = kMax[0];
      kMax[0] = newArr[i];
      newArr[i] = tmp;

      buildMinHeap(kMax);
    }
  }

  return kMax;
}
const findKMax = (arr, k) => {
  return arr.sort((a, b) => b - a).slice(0, k);
}

柯里化，是函数式编程的一个重要概念。它既能减少代码冗余，也能增加可读性。另外，附带着还能用来装逼。
先给出柯里化的定义：在数学和计算机科学中，柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术。
function curry (fn, currArgs) {
    return function() {
        let args = [].slice.call(arguments);

        // 首次调用时，若未提供最后一个参数currArgs，则不用进行args的拼接
        if (currArgs !== undefined) {
            args = args.concat(currArgs);
        }

        // 递归调用
        if (args.length < fn.length) {
            return curry(fn, args);
        }

        // 递归出口
        return fn.apply(null, args);
    }
}

function currying(fn) {
    var slice = Array.prototype.slice,
    __args = slice.call(arguments, 1);
    return function () {
        var __inargs = slice.call(arguments);
        return fn.apply(null, __args.concat(__inargs));
    };
}


for in Object.keys Object.getOwnPropertyName区别
for in会输出自身以及原型链上可枚举的属性。
Object.keys是es5中新增的方法，用来获取对象自身可枚举的属性键。
Object.getOwnPropertyNames也是es5中新增的方法，用来获取对象自身的全部属性名。

介绍一下观察者模式：
关于定义：
定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象的状态得到通知并自动更新
应用场景：
1.当一个对象的改变需要同时改变其他对象，而不知道到底有多少对象待改变
2.当一个对象必须通知其他对象，而它又不希望是紧密耦合的
优点：
1.目标和观察者之间的松散耦合，目标不需要知道观察者有多少，属于哪个具体的类。同时为了保持系统层次的完整，它们可以属于不同抽象层
2.可以在任意时刻增加和删除观察者
缺点：
观察者之间并不知道相互的存在。所以如果依赖准则定义不当、目标的操作失误，都会引起一系列依赖的错误更新

Vue.js 的响应式系统：如何做到在 data 发生改变时，视图会自动更新？
模型（Model）通过 Observer、Dep、Watcher、Directive 等一系列对象的关联，和视图（DOM）建立起关系。归纳起来，Vue.js 在这里主要做了三件事：

通过 Observer 对 data 做监听，并且提供了订阅某个数据项变化的能力。
把 template 编译成一段 document fragment，然后解析其中的 Directive，得到每一个 Directive 所依赖的数据项和 update 方法。
通过 Watcher 把上述两部分结合起来，即把 Directive 中的数据依赖通过 Watcher 订阅在对应数据的 Observer 的 Dep 上。当数据变化时，就会触发 Observer 的 Dep 上的 notify 方法通知对应的 Watcher 的 update，进而触发 Directive 的 update 方法来更新 DOM 视图，最后达到模型和视图关联起来。

定时器为什么不精确
setTimeout是一个异步的宏任务，当执行setTimeout时是将回调函数在指定的时间之后放入到宏任务队列。但如果此时主线程有很多同步代码在等待执行，或者微任务队列以及当前宏任务队列之前还有很多任务在排队等待执行，那么要等他们执行完成之后setTimeout的回调函数才会被执行，因此并不能保证在setTimeout中指定的时间立刻执行回调函数

数字签名是什么？
数字签名就是附加在数据单元上的一些数据,或是对数据单元所作的密码变换。这种数据或变换允许数据单元的接收者用以确认数据单元的来源和数据单元的完整性并保护数据,防止被人(例如接收者)进行伪造。它是对电子形式的消息进行签名的一种方法,一个签名消息能在一个通信网络中传输。基于公钥密码体制和私钥密码体制都可以获得数字签名,主要是基于公钥密码体制的数字签名。
数字签名是个加密的过程，数字签名验证是个解密的过程。
数字签名的原理是？
数字签名技术是将原文通过特定HASH函数得到的摘要信息用发送者的私钥加密，与原文一起传送给接收者。接收者只有用发送者的公钥才能解密被加密的摘要信息，然后用HASH函数对收到的原文提炼出一个摘要信息，与解密得到的摘要进行对比。哪怕只是一个字符不相同，用HASH函数生成的摘要就一定不同。如果比对结果一致，则说明收到的信息是完整的，在传输过程中没有被修改，否则信息一定被修改过，因此数字签名能够验证信息的完整性。
数字签名的作用是？
一是能确定消息的不可抵赖性，因为他人假冒不了发送方的私钥签名。发送方是用自己的私钥对信息进行加密的，只有使用发送方的公钥才能解密。
二是数字签名能保障消息的完整性。一次数字签名采用一个特定的哈希函数，它对不同文件产生的数字摘要的值也是不相同的。

手写instanceof
function instanceof(left, right) {
    // 获得类型的原型
    let prototype = right.prototype
    // 获得对象的原型
    left = left.__proto__
    // 判断对象的类型是否等于类型的原型
    while (true) {
      if (left === null)
        return false
      if (prototype === left)
        return true
      left = left.__proto__
    }
}
执行上下文
当执行 JS 代码时，会产生三种执行上下文
全局执行上下文
函数执行上下文
eval 执行上下文
每个执行上下文中都有三个重要的属性
变量对象（VO），包含变量、函数声明和函数的形参，该属性只能在全局上下文中访问
作用域链（JS 采用词法作用域，也就是说变量的作用域是在定义时就决定了）
this
闭包
闭包的定义很简单：函数 A 返回了一个函数 B，并且函数 B 中使用了函数 A 的变量，函数 B 就被称为闭包。
在浏览器环境中，常见的 macro task 有 setTimeout、MessageChannel、postMessage、setImmediate。而常见的 micro task 有 MutationObsever 和 Promise.then
深拷贝
function structuralClone(obj) {
  return new Promise(resolve => {
    const {port1, port2} = new MessageChannel();
    port2.onmessage = ev => resolve(ev.data);
    port1.postMessage(obj);
  });
}

对于 CommonJS 和 ES6 中的模块化的两者区别是：
前者支持动态导入，也就是 require(${path}/xx.js)，后者目前不支持，但是已有提案
前者是同步导入，因为用于服务端，文件都在本地，同步导入即使卡住主线程影响也不大。而后者是异步导入，因为用于浏览器，需要下载文件，如果也采用同步导入会对渲染有很大影响
前者在导出时都是值拷贝，就算导出的值变了，导入的值也不会改变，所以如果想更新值，必须重新导入一次。但是后者采用实时绑定的方式，导入导出的值都指向同一个内存地址，所以导入值会跟随导出值变化
后者会编译成 require/exports 来执行的

//手写generator
function generator(cb) {
  return (function() {
    var object = {
      next: 0,
      stop: function() {}
    };

    return {
      next: function() {
        var ret = cb(object);
        if (ret === undefined) return { value: undefined, done: true };
        return {
          value: ret,
          done: false
        };
      }
    };
  })();
}
事件触发有三个阶段
window 往事件触发处传播，遇到注册的捕获事件会触发
传播到事件触发处时触发注册的事件
从事件触发处往 window 传播，遇到注册的冒泡事件会触发
事件触发一般来说会按照上面的顺序进行，但是也有特例，如果给一个目标节点同时注册冒泡和捕获事件，事件触发会按照注册的顺序执行。
CORS 需要浏览器和后端同时支持。IE 8 和 9 需要通过 XDomainRequest 来实现。
浏览器会自动进行 CORS 通信，实现 CORS 通信的关键是后端。只要后端实现了 CORS，就实现了跨域。
服务端设置 Access-Control-Allow-Origin 就可以开启 CORS。 该属性表示哪些域名可以访问资源，如果设置通配符则表示所有网站都可以访问资源。
postMessage
这种方式通常用于获取嵌入页面中的第三方页面数据。一个页面发送消息，另一个页面判断来源并接收消息

浏览器 DNS缓存与DNS prefetch （DNS预解析）
浏览器在获取网站域名的实际IP地址后会对其IP进行缓存，减少网络请求的损耗。每种浏览器都有一个固定的DNS缓存时间，其中Chrome的过期时间是1分钟，在这个期限内不会重新请求DNS。Chrome浏览器看本身的DNS缓存时间比较方便，在地址栏输入：
DNS(Domain Name System, 域名系统)，是域名和IP地址相互映射的一个分布式数据库。DNS 查询就是将域名转换成 IP 的过程，这个过程短的话 2ms 几乎无感，长则可能达到几秒钟。
当浏览器访问一个域名的时候，需要解析一次DNS，获得对应域名的ip地址。在解析过程中，按照浏览器缓存、系统缓存、路由器缓存、ISP(运营商)DNS缓存、根域名服务器、顶级域名服务器、主域名服务器的顺序，逐步读取缓存，直到拿到IP地址。
DNS Prefetch，即DNS预解析就是根据浏览器定义的规则，提前解析之后可能会用到的域名，使解析结果缓存到系统缓存中，缩短DNS解析时间，来提高网站的访问速度。
现代浏览器在 DNS Prefetch 上做了两项工作：
1. html 源码下载完成后，会解析页面的包含链接的标签，提前查询对应的域名。
2. 对于访问过的页面，浏览器会记录一份域名列表，当再次打开时，会在 html 下载的同时去解析 DNS。
DNS预解析分为以下两种：
自动解析
浏览器使用超链接的href属性来查找要预解析的主机名。当遇到a标签，浏览器会自动将href中的域名解析为IP地址，这个解析过程是与用户浏览网页并行处理的。但是为了确保安全性，在HTTPS页面中不会自动解析。
DNS Prefetch有效缩短了DNS的解析时间
如果浏览器最近将一个域名解析为IP地址，所属的操作系统将会缓存，下一次DNS解析时间可以低至0-1ms。 如果结果不在系统本地缓存，则需要读取路由器的缓存，则解析时间的最小值大约为15ms。如果路由器缓存也不存在，则需要读取ISP（运营商）DNS缓存，一般像taobao.com、baidu.com这些常见的域名，读取ISP（运营商）DNS缓存需要的时间在80-120ms，如果是不常见的域名，平均需要200-300ms。一般的网站在运营商这里都能查询的到，所以普遍来说DNS Prefetch可以给一个域名的DNS解析过程带来15-300ms的提升，尤其是一些大量引用很多其他域名资源的网站，提升效果就更加明显了
浏览器底层缓存进行了建模，当Chrome浏览器启动的时候，就会自动的快速解析浏览器最近一次启动时记录的前10个域名。所以经常访问的网址就没有DNS解析的延迟，打开速度更快。

echarts遇到了那些问题？
echarts基于canvas，窗口改变并不会自适应，就监控resize，

series里数据不停地加载数据，我的div变高，echarts转换div高度重新加载，263次之后，就没有折线了
关于canvas中折线超过12000不显示
我现在用多个canvas拼接解决了部分问题，非常期待你们新版的发布。我再去试试自定义画

如果全局设置了textStyle，Legend中的字体大小设置将失效。
似乎是全局的 textStyle 影响了 legend 的 textStyle，暂时可以先去掉全局的 textStyle 配置来规避这个问题
实时动态曲线的动画不平滑，有顿挫感
可以试试把 animationEasing 改成 'linear'，然后你更新数据的频率也要控制成跟动画时长一样

请问折线图的y轴能不能分成几段？每段可以显示自己的曲线
建议调整下几个grid的间距，把几个图形拼接到一起，然后隐藏掉不要的x轴就可以了

大数据量面积图的导出x轴坐标一共是1-100，由于缩放只显示了1-10，目前希望哪怕界面上缩放只显示了1-10的数据，导出时也可以把1-100全部导出
只能在导出前把 dataZoom 设置成 0 - 100，导出后再设回 0 - 10

appenddata增量渲染
点在多边形内，注意到如果从P作水平向左的射线的话，如果P在多边形内部，那么这条射线与多边形的交点必为奇数，如果P在多边形外部，则交点个数必为偶数（0也在内）。所以，我们可以顺序考虑多边形的每条边，求出交点的总个数。


双向绑定
Vue2
vue.js是采用的数据劫持结合发布者-订阅者模式的方式,通过object.defineProperty()来劫持各个属性的setter/getter
在数据变动时,发布消息给订阅者,触发相应的监听回调

具体步骤:
    1)需要observe(观察者)的数据对象进行遍历,包括子属性对象的属性,都加上setter和getter,这样的话,
    给这个对象的某个值赋值,就会触发setter,那么就能监听到数据的变化
    2)compile(解析)解析模版指令,将模版中的变量替换成数据,然后初始化渲染页面视图,并将每个指令对应的节点绑定更新函数,添加监听数据的订阅者,一旦数据有变动,收到通知,更新视图
    3)watcher(订阅者)是observer和compile之间通信的桥梁,主要做的事情是
        1>在实例化时往属性订阅器(dep)里面添加自己
        2>自身必须有一个update()方法
        3>待属性变动dep.notice()通知时,能够调用自身的update()方法,并触发compile中绑定的回调,
    4)mvvm作为数据绑定的入口,整合observer,compile和watcher来监听自己的model数据变化,通过compile来解析编译模版,
    最终利用watcher搭起observer和compile之间的通信桥梁,达到数据变化->更新视图:视图交互变化->数据model变更的双向绑定效果



Vue3
Object.defineProperty的缺陷:
1)无法检测到对象属性的新增或删除
    由于js的动态性，可以为对象追加新的属性或者删除其中某个属性，
    这点对经过Object.defineProperty方法建立的响应式对象来说，
    只能追踪对象已有数据是否被修改，无法追踪新增属性和删除属性，
    这就需要另外处理。
2)不能监听数组的变化（对数组基于下标的修改、对于 .length 修改的监测）
   vue在实现数组的响应式时，它使用了一些hack，
   把无法监听数组的情况通过重写数组的部分方法来实现响应式，
   这也只限制在数组的push/pop/shift/unshift/splice/sort/reverse七个方法，
   其他数组方法及数组的使用则无法检测到，
解决方法主要是使用proxy属性,这个proxy属性是ES6中新增的一个属性,
    proxy属性也是一个构造函数,他也可以通过new的方式创建这个函数,
    表示修改某些操作的默认行为,等同于在语言层面做出修改,所以属于一种元编程
    proxy可以理解为在目标对象之前架设一层拦截,外界对该对象的访问,都必须经过这层拦截,
    因此提出了一种机制,可以对外界的网文进行过滤和改写,proxy这个词是代理,
    用来表示由他代理某些操作,可以译为代理器

    Proxy，字面意思是代理，是ES6提供的一个新的API，用于修改某些操作的默认行为，
    可以理解为在目标对象之前做一层拦截，外部所有的访问都必须通过这层拦截，
    通过这层拦截可以做很多事情，比如对数据进行过滤、修改或者收集信息之类。
    借用proxy的巧用的一幅图，它很形象的表达了Proxy的作用。
proxy代理的特点:
    proxy直接代理的是整个对象而非对象属性,
    proxy的代理针对的是整个对象而不是像object.defineProperty针对某个属性,
    只需要做一层代理就可以监听同级结构下的所有属性变化,
    包括新增的属性和删除的属性
proxy代理身上定义的方法共有13种,其中我们最常用的就是set和get,但是他本身还有其他的13种方法

proxy的劣势:
    兼容性问题,虽然proxy相对越object.defineProperty有很有优势,但是并不是说proxy,就是完全的没有劣势,主要表现在以下的两个方面:
        1)proxy有兼容性问题,无完全的polyfill:
            proxy为ES6新出的API,浏览器对其的支持情况可在w3c规范中查到,通过查找我们可以知道,
            虽然大部分浏览器支持proxy特性,但是一些浏览器或者低版本不支持proxy,
            因此proxy有兼容性问题,那能否像ES6其他特性有polyfill解决方案呢?,
            这时我们通过查询babel文档,发现在使用babel对代码进行降级处理的时候,并没有合适的polyfill
        2)第二个问题就是性能问题,proxy的性能其实比promise还差,
        这就需要在性能和简单实用上进行权衡,例如vue3使用proxy后,
        其对对象及数组的拦截很容易实现数据的响应式,尤其是数组

        虽然proxy有性能和兼容性处理,但是proxy作为新标准将受到浏览器厂商重点持续的性能优化,
        性能这块会逐步得到改善

AST模型
AST就是抽象语法树，他是js代码另一种结构映射，可以将js拆解成AST，也可以把AST转成源代码。这中间的过程就是我们的用武之地。 利用 抽象语法树(AST) 可以对你的源代码进行修改、优化，甚至可以打造自己的编译工具。其实有点类似babel的功能。
AST也是一种数据结构，这种数据结构其实就是一个大的json对象，json我们都熟悉，他就像一颗枝繁叶茂的树。

vue2.x版本使用了Object.defineProperty来实现双向绑定，由于其功能的限制，只能绑定对象的某个属性，vue需要递归遍历对象的所有属性挨个进行绑定，功能上不是很完美。
vue3.0版本使用proxy进行双向绑定，proxy提供了可以定义对象基本操作的自定义行为的功能（如属性查找、赋值、枚举、函数调用），可以直接拦截整个对象，不需要再进行递归。本例中我们只使用到了proxy提供自定义set和get的能力。


Vue3
Composition API
逻辑复用
Typescript
Tree Shaking
1. 虚拟 DOM 重写，mounting和patching的速度提高100％
2. 更多的编译时的提示来减少运行时的开销
3. 组件快速路径+单个调用+子类型检测
跳过不必要的条件分支
JS引擎更容易优化
4. 优化插槽的生成
确保实例正确的跟踪依赖关系
避免不必要的父子组件重新渲染
5. 静态树提升
跳过修补整棵树，从而降低渲染成本
即使多次出现也能正常工作
8. 基于Proxy的观察者机制，全语言覆盖+更好的性能
目前vue使用的是Object.defineProperty 的 getter 和 setter
组件实例初始化的速度提高100％
使用Proxy节省以前一半的内存开销，加快速度，但是存在低浏览器版本的不兼容
为了继续IE11，Vue 3 将发布一个支持旧观察者机制和新 Proxy 版本的构建
压缩包体积更小
1.Performance [pəˈfɔːməns]
重写了虚拟Dom的实现（跳过静态节点，只处理动态节点）
update性能提高1.3~2倍
SSR速度提高了2~3倍
Tree shaking [ˈʃeɪkɪŋ]
可以将无用模块“剪辑”，仅打包需要的
Fragment ['frægmənt]
不再限于模板中的单个根节点
<Teleport> [ˈtelɪpɔːt]

以前称为<Portal> [ˈpɔːtl]，译作传送门
<Suspense> [səˈspens]

可在嵌套层级中等待嵌套的异步依赖项
TypeScript

更好的TypeScript支持
Custom Renderer API

自定义渲染器API
用户可以尝试WebGL自定义渲染器
Composition API

组合式API，替换原有的 Options API
根据逻辑相关性组织代码，提高可读性和可维护性
更好的重用逻辑代码（避免mixins混入时命名冲突的问题）
但是依然可以延用 Options [ˈɒpʃnz] API
1.插槽优化
　　在当前的 Vue 版本中，当父组件重新渲染时，其子组件也必须重新渲染。 使用 Vue 3 ，可以单独重新渲染父组件和子组件。

　　2.虚拟dom优化
　　随着虚拟 DOM 重写，我们可以期待更多的 编译时（compile-time）提示来减少 运行时（runtime）开销。重写将包括更有效的代码来创建虚拟节点。

　　3.静态树提升
　　使用静态树提升，这意味着 Vue 3 的编译器将能够检测到什么是静态组件，然后将其提升，从而降低了渲染成本。它将能够跳过未整个树结构打补丁的过程。

　　4.静态属性提升
　　此外，我们可以期待静态属性提升，其中 Vue 3 将跳过不会改变节点的打补丁过程。





首先客户端通过URL访问服务器建立SSL连接。
服务端收到客户端请求后，会将网站支持的证书信息（证书中包含公钥）传送一份给客户端。
客户端的服务器开始协商SSL连接的安全等级，也就是信息加密的等级。
客户端的浏览器根据双方同意的安全等级，建立会话密钥，然后利用网站的公钥将会话密钥加密，并传送给网站。
服务器利用自己的私钥解密出会话密钥。
服务器利用会话密钥加密与客户端之间的通信。
