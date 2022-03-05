## TCP/IP

- 由`FTP/SMTP/TCP/UDP/IP`等协议组成的协议族

- 分层 
  - 应用层
    - 决定了向用户提供应用服务时同心的活动
    - 如：`FTP、DNS、HTTP`
  - 传输层
    - 对上层应用层提供处于网络连接中的两台计算机之间的数据传输
    - 如：`TCP、UDP`
  - 网络层
    - 处理网络上流动的数据包。规定了通过怎样的路径到达对方的计算机，并把数据包传送给对方
    - 如：`IP`
  - 链路层
    - 用来处理连接网络的硬件设备
    - 如：操作系统、硬件设备驱动、NIC（网卡）、光纤等

## TCP三次握手

- 建立一个TCP连接时

  - 发送端首先发送一个带`SYN`标志的数据包给对方。

  - 接收到接收后，回传一个带有`SYN+ACK`标志的数据包以示传达确认信息。

  - 最后，发送端再回传一个带有`ACK`标志的数据包，表示握手结束。


## TCP四次挥手

- TCP终止连接时
  - 第一次挥手 客户端发送一个FIN报文（连续释放报文段），报文中指定一个序列号。此时客户端处于`FIN_WAIT1`状态。
  - 第二次挥手 服务端接收到FIN之后，会发送`ACK`报文（确认报文段），且将客户端的序列号+1作为`ACK`报文的序列号值，表明已经收到客户端的报文了，此时服务端处于`CLOSE_WAIT`状态。
  - 第三次挥手 如果服务端也想断开连接，和客户端第一次挥手一样发送`FIN`报文连续释放报文段），且指定一个序列号。此时服务端处于`LAST_ACK`状态。
  - 第四次挥手 客户端接收到FIN报文，一样发送一个ACK报文作为应答，且把服务端的序列号+1作为自己的`ACK`报文的序列号值，此时客户端处于TIME_WAIT状态。过一段时间（`2MSL`），确保服务端收到自己的`ACK`报文后进入`CLOSED`状态。服务端接收到ACK报文后就处于`CLOSED`状态了。

## HTTP/1.1方法

- `GET`
- `POST`
- `PUT`
- `DELETE`
- `HEAD`
- `OPTIONS`
- `TRACE`
- `CONNECT`

## HTTP状态码

- 1xx 信息性状态码 接收的请求正在处理
- 2xx 成功状态码 请求正常处理完毕
  - 200 请求成功
  - 204 请求成功 但没有返回内容
- 3xx 重定向状态码 需要进行附加操作以完成请求
  - 301 永久重定向 照浏览器标准不允许post变成get获取请求资源
  - 302 临时重定向 照浏览器标准不允许post变成get获取请求资源
  - 303 查看其他位置  与302类似 但明确表示应采用get获取请求资源
  - 304 服务端资源未改变 可直接使用客户端未过期的缓存
  - 305 临时重定向 遵照浏览器标准不允许post变成get
- 4xx 客户端错误状态码 服务器无法处理请求
  - 400 请求报文中存在语法错误
  - 401 该请求需要HTTP认证 （BASIC、DIGEST）的认证信息
  - 403 访问被拒绝 理由可以放在主体部分
  - 404 服务器无法找到请求的资源
- 5xx 服务器错误状态码 服务器处理请求出错
  - 500 服务器端在执行请求时发生了错误/web应用存在bug/临时的故障
  - 503 服务器超负载/停机维护

## DNS

- DNS查询方式

  - 递归查询

  - 迭代查询
- 域名缓存

  - 浏览器缓存
  - 操作系统缓存
- 查询过程

  - 首先搜索浏览器的DNS缓存，缓存中维护一张域名与IP地址的表
  - 没有命中，在操作系统中hosts文件搜索
  - 没有命中，则将域名发送至本地域名服务器，本地域名服务器将通过递归查询自己的DNS缓存。
  - 没有命中，则本地域名服务器向上级域名服务器进行迭代查询。
    - 本地域名服务器向根域名服务器发起请求，根域名服务器返回顶级域名服务器的地址给本地域名服务器。
    - 本地域名服务器拿到这个顶级域名服务器的地址后，向其发起请求，获取权限域名服务器的地址。
    - 本地域名服务器根据权限域名服务器的地址，向其发起请求，最终得到对应的IP地址。
  - 本地域名服务器得到IP地址返回给操作系统，同时将IP缓存起来
  - 操作系统将IP返回给浏览器，也将IP缓存起来。
  - 浏览器得到了对应IP地址，也将IP缓存起来。

## 缓存技术

- 分类
  - 数据缓存
  - CDN缓存
  - 代理服务器缓存
  - 浏览器缓存

- 浏览器缓存 
  - 在计算机中开辟一个内存区 同时也开辟一个硬盘区作为数据传输的缓冲区，用来暂时保存用户以前访问过的信息
  - 缓存位置 
    - Service Worker 
      - 运行在浏览器背后的独立线程
      - 传输协议必须为HTTPS
      - 自由控制缓存哪些文件、如何匹配缓存、如何读取缓存，并且缓存是持续性的
      - 步骤
        - 先注册Service Worker
        - 监听到 install 事件
        - 用户访问的时候就可以通过拦截请求的方式查询是否存在缓存，存在缓存的话就可以直接读取缓存文件，否则就去请求数据

    - Memory Cache 
      - 样式、脚本、图片等
      - 随着进程的释放而释放

    - Disk Cache 
    - Push Cache 推送缓存
      - HTTP/2 中
      - 上面三种缓存都没有命中时，它才会被使用
      - 会话结束就被释放

  - 分类
    - 强缓存
    - 协商缓存

  - 过程
    - 浏览器在加载资源时，根据资源的header判断是否命中强缓存
      - Cache-Control
        - 优先级大于Expires
        -  max-age 资源有效时间（秒）
        - no-cache 协商缓存
        - no-store 禁止缓存
        - public 所有用户都可缓存
        - private 终端浏览器缓存

      - Expires
        - 值是一个绝对时间，表示资源的失效时间
        - 缺点 服务器与客户端时间偏差较大导致缓存混乱

    - 如果命中，浏览器直接从缓存中读取资源，并不向服务器发起请求。返回200
    - 没有命中，浏览器会发送请求到服务器。
      - 当Cache-Control=no-cache时，服务器通过header判断是否命中协商缓存
        - Last-Modified/If-Modified-Since
          - Last-Modified表示该资源最后修改的时间
          - 再次请求资源时，请求头中会包含If-Modified-Since，该值为缓存之前返回的Last-Modified
          - 服务器判断是否命中协商缓存
          - 如果命中，服务器返回304并不反回资源也不反回Last-Modified。继续使用缓存。
          - 没有命中，重新发起请求
          - 缺点 短时间内资源发生了改变，Last-Modified并不会发生变化。

        - ETag/If-None-Match
          - ETag资源的唯一标识，资源改变时ETag会发生变化
          - 再次请求资源时，请求头中会包含ETag/If-None-Match，该值为缓存之前返回的ETag
          - 服务器判断是否命中协商缓存
          - 如果命中，服务器返回304并不反回资源，但会将ETag返回。继续使用缓存。
          - 没有命中，重新发起请求

      - 当Cache-Control=no-store时 禁止使用缓存，重新请求


## HTTP与HTTPS

- HTTPS层次 HTTP TLS/SSL TCP

- HTTP
  - 支持客户/服务器模式
  - 简单快速 请求方法和路径
  - 灵活 任意类型数据 Content-Type标记
  - 无连接 每次连接只处理一次 收到应答后就断开连接
  - 无状态 HTTP协议无法根据上次请求进行本次请求的处理
- HTTPS
  - HTTPS = HTTP + SSL/TLS
  - 过程
    - 客户端通过URL访问服务器建立SSL连接
    - 服务器接收到请求后，将网站支持的证书信息（包含公钥）传递给客户端
    - 客户端协商SSL连接的安全等级
    - 浏览器根据双方同意的安全等级，建立会话密钥，然后用公钥对会话密钥进行加密，传给服务器
    - 服务器利用自己的私钥解密
- 区别
  - HTTPS更加安全
  - 默认端口80/443
  - HTTPS需要加密和多次握手，性能不如HTTP
  - HTTPS需要SSL证书

## HTTP/1.0、HTTP/1.1、HTTP/2.0

- HTTP/1.0 每次请求都需要创建一个新的TCP连接，如果需要长连接需要设置 Connection：keep-alive
- HTTP/1.1 
  - 默认长连接 Connection: keep-alive 即在一个TCP连接上可以传送多个HTTP请求和响应
  - 不需要等待上一次请求结果返回，就可以发送下一个请求
  - 引入更多的缓存策略 If-Unmodified-Since, If-Match, If-None-Match
  - 引入Range 允许请求资源的某个部分
  - 引入host 实现一个台web服务器上可以在同一个IP地址和端口上使用不同的主机名创建多个虚拟web
  - 添加了 PUT、DELETE、OPTIONS等方法
- HTTP/2.0
  - 多路复用
    - 复用TCP连接，在一个连接内，客户端和浏览器可以同时发送多个请求和响应，且不用按照一一对应顺序，避免队头堵塞
  - 二进制分帧
    - 帧是HTTP/2.0最小的通讯单位
    - 采用二进制格式传输数据，而非文本格式，解析高效
    - 将请求分割为更小的帧，并且采用二进制编码
    - 同域名下所有通讯在单个连接上完成，连接可以承载任意数量的双向数据流
    - 每个数据都以消息的形式发送，而消息又由一个或多个帧组成。多个帧可以乱序发送，根据帧首部的流标识重新组装
  - 首部压缩
    - HTTP/2.0 使用首部表来跟踪和存储之前的发送的键值对，对于相同的数据，不再通过每次请求和响应发送
    - 首部表在HTTP/2.0连续存续期内始终存在，由客户端和服务器共同渐进的更新
  - 服务器推送
    - 允许服务器推送资源给服务端

## WebScoket与HTTP的区别

- 相同点
  - 都是基于TCP的应用层协议
  - 都使用Request/Response模型进行连接的建立
  - 在连接的建立过程中对错误的处理方式相同，在这个阶段WebSocket可能返回和HTTP相同的返回码
  - 都可以在网络中传输数据
- 不同点
  - WebSocket使用HTTP来建立连接，但是定义了一系列新的header域，这些域在HTTP中并不会使用
  - 连接不能通过中间人来转发，它必须是一个直接连接
  - 连接建立之后，通信双方都可以在任何时刻向另一方发送数据
  - 连接建立之后，数据的传输使用帧来传递，不再需要Request消息
  - 数据帧有序

## HTTP报文

- 组成 报文首部 空行（CR+LF） 报文主体

  - 请求报文首部
    - 请求行
    - 请求首部字段
    - 通用首部字段
    - 实体首部字段
    - 其他
  - 响应报文首部
    - 状态行
    - 响应首部字段
    - 通用首部字段
    - 实体首部字段
    - 其他

- 首部 RF2616

  - 通用首部字段

    | 字段              | 说明                       |
    | ----------------- | -------------------------- |
    | Cache-Control     | 控制缓存的行为             |
    | Connection        | 逐跳首部、连接的管理       |
    | Date              | 创建报文的日期时间         |
    | Pragma            | 报文指令                   |
    | Trailer           | 报文末端的首部一览         |
    | Transfer-Encoding | 指定报文主体的传输编码方式 |
    | Upgrade           | 升级为其他协议             |
    | Via               | 代理服务器的相关信息       |
    | Warning           | 错误通知                   |

  - 请求首部字段

    | 字段                | 说明                                          |
    | ------------------- | --------------------------------------------- |
    | Accept              | 用户代理可处理的媒体类型                      |
    | Accept-Charset      | 优先的字符集                                  |
    | Accept-Encoding     | 优先的内容编码                                |
    | Accept-Language     | 优先的语言                                    |
    | Authorization       | Web认证信息                                   |
    | Expect              | 期待服务器的特定行为                          |
    | From                | 用户的电子邮件地址                            |
    | Host                | 请求资源的所在服务器                          |
    | If-Match            | 比较实体标记（ETag）                          |
    | If-Modified-Since   | 比较资源的更新时间                            |
    | If-None-Match       | 比较实体标记 （与If-Match相反）               |
    | If-Range            | 资源未更新时发送实体Byte的范围请求            |
    | If-Unmodified-Since | 比较资源的更新时间（与If-Modified-Since相反） |
    | Max-Forwards        | 最大传输逐跳数                                |
    | Proxy-Authorization | 代理服务器要求客户端的认证信息                |
    | Range               | 实体的字节范围编码                            |
    | Referer             | 对请求URI的原始获取方                         |
    | TE                  | 传输编码的优先级                              |
    | User-Agent          | HTTP客户端程序的信息                          |

  - 响应首部字段

    | 字段                | 说明                         |
    | ------------------- | ---------------------------- |
    | Accept-Ranges       | 是否接受字节范围请求         |
    | Age                 | 推送资源创建经过时间         |
    | ETag                | 资源的匹配信息               |
    | Location            | 令客户端重定向至指定的URI    |
    | Proxy-Authorization | 代理服务器对客户端的认证信息 |
    | Retry-After         | 对再次发起请求的时机要求     |
    | Server              | HTTP服务器的安装信息         |
    | Vary                | 代理服务器缓存的管理信息     |
    | WWW-Authenticate    | 服务器对客户端的认证信息     |

  - 实体首部字段

    | 字段             | 说明                   |
    | ---------------- | ---------------------- |
    | Allow            | 资源可支持的HTTP方法   |
    | Content-Encoding | 实体主体使用的编码方式 |
    | Content-Language | 实体主体的自然语言     |
    | Content-Length   | 实体主体的大小（字节） |
    | Content-Location | 替代对应资源的URI      |
    | Content-MD5      | 实体主体的报文摘要     |
    | Content-Range    | 实体主体的位置范围     |
    | Content-Type     | 实体主体的媒体类型     |
    | Expires          | 实体主体过期的日期时间 |
    | Last-Modified    | 资源的最后修改日期时间 |

  - Cache-Control 控制缓存的行为

    - 例如 Cache-Control：private，max-age=0，no-chche

    - 缓存请求指令

      | 指令                | 参数   | 说明                         |
      | ------------------- | ------ | ---------------------------- |
      | no-cache            | 无     | 强制向资源服务器再次验证     |
      | no-store            | 无     | 不缓存请求或响应的任何内容   |
      | max-age = [秒]      | 必须   | 响应的最大Age值              |
      | max-stale（= [秒]） | 可省略 | 接受已过期的响应             |
      | min-fresh = [秒]    | 必须   | 期望在指定时间内的响应仍有效 |
      | no-transform        | 无     | 代理不可更改媒体类型         |
      | only-if-cached      | 无     | 从缓存中获取资源             |
      | cache-extension     | -      | 新指令标记（token）          |

    - 缓存响应指令

      | 指令             | 参数   | 说明                                           |
      | ---------------- | ------ | ---------------------------------------------- |
      | public           | 无     | 可向任意方提供响应的缓存                       |
      | private          | 可省略 | 仅特定用户返回响应                             |
      | no-cache         | 可省略 | 缓存前必须先确认其有效性                       |
      | no-store         | 无     | 不缓存请求或响应的任何内容                     |
      | no-transform     | 无     | 代理不可更改媒体类型                           |
      | must-revalidate  | 无     | 可缓存但必须再向服务器进行确认                 |
      | proxy-revalidate | 无     | 要求中间缓存服务器对缓存的响应有效性再进行确认 |
      | max-age = [秒]   | 必须   | 响应的最大Age值                                |
      | s-maxage = [秒]  | 必须   | 公共缓存服务器响应的最大Age值                  |
      | cache-extension  | -      | 新指令标记（Token）                            |

  - 请求响应首部

    - Accept

      - 例如  q表示权重
  
  
      ```
      Accpet：text/html,application/xhtml+xml,application/xml;q=0.9,*/*,q=0.9
      ```
      
      - 类型
        - 文本文件 text/html，text/plain，text/css，application/xhtml+xml，application/xml ...
        - 图片文件 image/jpeg，image/gif，image/png ...
        - 视频文件 video/mpeg，video/quicktime ...
        - 应用程序使用的二进制文件 application/octet-stream，application/zip ...
  
    - Accept-Charset
  
      - 通知服务器用户代理支持的字符集及相对优先顺序
  
    - Accept-Encoding
  
      - 告知服务器用户代理支持的内容编码及相对优先顺序
      - 参数
        - gzip 文件压缩程序 GNU zip LZ77算法及32为冗余检验
        - compress 文件压缩程序 LZW算法
        - deflate压缩算法
        - identity 不执行压缩
  
    - Accept-Language
  
      - 告知服务器用户代理能够处理的自然语言级及相对优先顺序
      - 参数
        - zh-cn，zh
        - en-us，en
  
    - Authorization
  
      - 告知服务器，用户代理的认证信息。
  
    - Expect
  
      - 告知服务器，期望出现的某种特定行为。
  
    - Host
  
      - 告知服务器，请求的资源所处的互联网主机名和端口号。（必须包含在请求内的首部字段）
  
    - If-Match
  
      - 实体标记（ETag）是与特定资源关联的确定值。资源更新后ETag也会随之更新
      - 当If-Match与ETag值匹配一致时，服务器才会接受请求否则返回412
      - 用*指定If-Match值，服务器将忽略ETag
  
    - If-Modified-Since
  
      - 告知服务器若If-Modified-Since的值早于资源更新时间，则处理该请求
      - 否则返回状态码304
      - If-Modified-Since用于确认代理或客户端拥有的本地资源的有效性。获取资源的更新日期时间，可通过If-Modified-Since来确定。
  
    - If-None-Match
  
      - 在If-None-Match与ETag值不一致时，处理该请求
      - 在GET/HEAD方法中使用If-None-Match可获取最新的资源。
  
    - If-Range
  
      - 指定的If-Range字段值（ETag值/时间）和请求资源的ETag或时间相一致，则作为范围请求资源
      - 否则返回全体资源
  
    - If-Unmodified-Since
  
      - 指定的请求只有在字段值内指定的日期时间之后，未发生更新情况下处理请求
      - 否则返回412
  
    - Max-Forwards
  
      - 通过TRACE/OPTIONS方法，发送包含Max-Forwards的请求时，该字段以十进制形式指定可经过的服务器最大数目
      - 服务器在往下一个服务器转发请求之前，Max-Forwards的值减1后重新赋值
      - 当服务器接收到Max-Forwards为0时，则不再进行转发，直接返回响应
  
    - Proxy-Authorization
  
      - 与Authorization区别 
        - Proxy-Authorization认证行为发生在客户端与代理之间
        - Authorization发生在客户端与服务器之间
  
    - Range
  
      - 处理请求之后返回状态码为206的响应
      - 无法处理该范围请求时，则返回200响应及全部资源
  
    - Referer
  
      - 告知服务器原始资源的URI
  
    - TE
  
      - 告知服务器客户端能够处理响应的传播编码方式及相对优先级
      - 还可以指定伴随trailer字段的分块传播编码的方式
  
    - User-Agent
  
      - 将创建请求的浏览器和用户代理名称等信息传达给服务器
  
  - 响应首部字段
  
    - Accept-Ranges
      - 告知客户端服务器是否能处理范围请求，以获取服务器端某个部分的资源
      - bytes/none
    - Age
      - 告知客户端，源服务器在多久之前创建了响应（单位秒）
    - ETag
      - 强ETag 不论实体发生多么细微的变化都会改变其值
      - 弱ETag 只会提示资源是否相同。只有资源发生了根本改变，产生差异时才会改变ETag值，会在最开始处附加W/
    - Location
      - 可以将响应接收方式引导与某个请求URI位置不同的资源 3xx
      - 浏览器会强制性地尝试对已提示的重定向资源访问
    - Proxy-Authenticate
      - 会把由代理服务器所要求的认证信息发送给客户端
      - 与WWW-Authorization区别
        - Proxy-Authenticate是客户端与代理之间进行的
        - WWW-Authorization是客户端与服务器之间进行的
    - Retry-After
      - 告知客户端应该在多久之后再次发送请求 配合503或 3xx一起使用
      - 值可以是具体日期时间或创建响应后的秒数
    - Server
      - 告知客户端当前服务器上安装的HTTP服务器应用程序信息
    - Vary
      - 源服务器会对代理服务器传达关于本地缓存使用方法的命令
      - 从代理服务器接收到源服务器返回包含Vary指定项的响应之后
      - 若要再进行缓存，仅对请求中含有相同Vary指定首部字段的请求返回缓存
      - 即使对相同资源发起请求，但由于Vary指定的首部字段不相同，因此必须要从源服务器重新获取资源
    - WWW-Authorization
      - 401中肯定带有WWW-Authorization字段
  
  - 实体首部字段
  
    - Allow
      - 通知客户端所有支持的HTTP方法
      - 不支持则返回405 并将所有支持的方法写入Allow返回
    - Content-Encoding
      - 告知客户端对实体的主体部分选用的内容编码方式
        - gzip
        - compress
        - deflate
        - identity
    - Content-Language
      - 告知客户端主体使用的自然语言
    - Content-Length
      - 表明实体主体部分的大小（字节）
    - Content-Location
      - 与报文主体部分相对应的URI
      - 与Location不同 Content-Location表示的是报文主体返回资源对应的URI
    - Content-MD5
      - 保证报文主体在传输过程中是否保持完整
      - 将报文主体执行MD5算法获得128位二进制数
      - HTTP首部无法记录二进制值，所以将其通过Base64编码后存入Content-MD5
      - 客户端会对其进行相同MD5算法，判断报文的准确性
    - Content-Range
      - 针对范围请求，告知客户端作为响应返回的实体的哪个部分符合范围请求。
    - Content-Type
      - 说明实体内对象的媒体类型 字段采用type/subtype
    - Expires
      - 将资源失效的日期告知客户端
      - 缓存服务器在接收到含有首部字段的Expires的响应后，会以缓存来应答请求
      - 在Expires字段指定的时间之前，响应的副本会一直被保存
      - 当超过指定的时间后，缓存服务器在请求发送过来时，会转向源服务器请求资源
      - 源服务器不想缓存服务器对资源缓存时，可以在Expires字段内写入与首部字段Date相同的时间值
      - 但是首部Cache-Control有指定max-age指令时，会优先处理Cache-Control
    - Last-Modified
      - 指明资源最终修改的时间
  
  - 为Cookie服务的首部字段
  
    - Set-Cookie
  
      | 属性         | 说明                                                         |
      | ------------ | ------------------------------------------------------------ |
      | NAME=VALUE   | 赋予Cookie的名字和其值（必须项）                             |
      | expires=DATE | Cookie的有效期（若不明确指定则默认为浏览器关闭前为止）       |
      | path=PATH    | 将服务器上的文件目录作为COokie的适用对象（若不指定则默认值认为文档所在的文件目录） |
      | domain=域名  | 作为Cookie使用对象的域名（若不指定则默认为创建Cookie的服务器域名） |
      | Secure       | 仅在HTTPS安全通信时才发送Cookie                              |
      | HttpOnly     | 加以限制，使Cookie不能被JavaScript脚本访问                   |
  
      - expires
        - 指定浏览器可发送Cookie的有效期
        - 若不明确指定则默认为浏览器关闭前为止
        - 一旦Cookie从服务器端发送至客户端，服务端就不存在可以显示删除Cookie的方法，但可以通过覆盖已过期的Cookie
      - HttpOnly
        - 防止XSS对Cookie信息劫取
        - 无法使用document.Cookie读取附加在HttpOnly属性后的Cookie内容
  
    - Cookie
  
      - 告知服务器，当客户端想获取HTTP状态管理支持时
      - 就会在请求中包含从服务器接收的Cookie
  
  - 其他首部字段
  
    - X-Frame-Options
      - 属于HTTP响应首部
      - 用户控制网站内容在其他Web网站的Frame标签内的显示问题，防止点击劫取
      - 值
        - DENY 拒绝
        - SAMEORIGIN 仅同源域名下的页面匹配时许可
    - X-XSS-Protection
      - 属于HTTP响应首部
      - 针对跨站脚本攻击XSS的一种策略，用于控制浏览器XSS防护机制的开关
      - 值
        - 0 将XSS过滤设置为无效状态
        - 1 将XSS过滤设置为有效状态
    - DNT
      - 属于HTTP请求首部
      - 拒绝被精准广告追踪的一种方法
      - 值
        - 0 同意被追踪
        - 1 拒绝被追踪
    - P3P
      - 属于HTTP响应首部
      - 可以让Web网站上的个人隐私变成一种仅供程序可理解的形式，达到保护用户隐私的目的
      - 步骤
        - 创建P3P隐私
        - 创建P3P隐私对照文件，保存命名在/w3c/p3p.xml
        - 从P3P隐私中创建Compact policies后，输出到HTTP响应中

## 输入URL

- 输入url
- 检查判断是否使用缓存
  - 浏览器在加载资源时，根据资源的header判断是否命中强缓存 （Cache-Control/Expires）
  - 如果命中，浏览器直接从缓存中读取资源，并不向服务器发起请求。返回200
  - 没有命中，浏览器会发送请求到服务器。
    - 当Cache-Control=no-cache时，服务器通过header判断是否命中协商缓存 （Last-Modified/If-Modified-Since 与 ETag/If-None-Match）
    - 当Cache-Control=no-store时 禁止使用缓存
- 域名解析
  - DNS解析查找
    - 首先搜索浏览器的DNS缓存，缓存中维护一张域名与IP地址的表
    - 没有命中，在操作系统中hosts文件搜索
    - 没有命中，则将域名发送至本地域名服务器，本地域名服务器将通过递归查询自己的DNS缓存。
    - 没有命中，则本地域名服务器向上级域名服务器进行迭代查询。
      - 本地域名服务器向根域名服务器发起请求，根域名服务器返回顶级域名服务器的地址给本地域名服务器。
      - 本地域名服务器拿到这个顶级域名服务器的地址后，向其发起请求，获取权限域名服务器的地址。
      - 本地域名服务器根据权限域名服务器的地址，向其发起请求，最终得到对应的IP地址。
    - 本地域名服务器得到IP地址返回给操作系统，同时将IP缓存起来
    - 操作系统将IP返回给浏览器，也将IP缓存起来。
    - 浏览器得到了对应IP地址，也将IP缓存起来。
- 建立TCP连接
  - 将HTTP请求报文分割成报文段
  - 三次握手
    - 发送端首先发送一个带SYN标志的数据包给对方。
    - 接收到接收后，回传一个带有SYN + ACK标志的数据包以示传达确认信息。
    - 最后，发送端再回传一个带有ACK标志的数据包，表示握手结束。
- HTTP协议
  - 发送HTTP请求
- 解析渲染
  - 查看响应头的信息，根据不同的指示做对应处理，比如重定向，存储`cookie`，解压`gzip`，缓存资源等等
  - 查看响应头的 `Content-Type`的值，根据不同的资源类型采用不同的解析方式
    - 解析HTML，构建 `DOM` 树（深度遍历过程）
    - 解析 `CSS` ，如果是style标签则开始解析`CSS`，如果是link标签则优先异步下载后解析`CSS`，生成 `CSS` 规则树
    - 遇到`script`标签，行内直接执行，如果是`src`引入则同步下载脚本文件，立即执行。
    - 合并 `DOM` 树和 `CSS` 规则，生成 `render` 树
    - 布局 `render` 树（ `Layout / reflow` ），负责各元素尺寸、位置的计算
    - 绘制 `render `树（` paint `），绘制页面像素信息
    - 浏览器会将各层的信息发送给 `GPU`，`GPU `会将各层合成（` composite `），显示在屏幕上

## AJAX

```js
const xhr = new XMLHttpRequest()
xhr.open("GET", "baidu.com", false);
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
xhr.onreadystatechange = function () {
    console.log('[ xhr ] >', xhr.readyState)
    if (this.readyState === 4) {}
}
xhr.send("name=John&age=18");
```

- readyState
  - 0 UNSENT 已创建 未调用 `open()`方法
  - 1 OPENED `open()` 方法已经被调用
  - 2  HEADERS_RECEIVED `send()` 方法已经被调用，并且头部和状态已经可获得
  - 3 LOADING `responseText` 属性已经包含部分数据
  - 4 DONE 下载操作已完成。

## WebScoket

```js
let ws = new WebSocket('ws://xxx')
ws.onopen = e => {
    ws.send('我发送消息给服务端')
}
ws.onmessage = e => {
    console.log('onmessage')
}
ws.onclose = e => {
    console.log('onclose')
}
ws.onerror = e => {
    console.log('onclose')
}
console.log(ws.readyState);
```

- readyState
  - 0 CONNECTING 正在连接中
  - 1 OPEN 已连接并且可以通讯
  - 2 CLOSING 连接正在关闭
  - 3 CLOSED 连接已关闭或没有连接成功