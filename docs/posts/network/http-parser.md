
# Http-parser 代码阅读笔记

## http的请求和响应报文的格式

### request 报文格式

请求报文的格式

```
<request-line>
<headers>
<blank line>
[<request-body>]
```

#### request-line

`<request-line>`: 请求行格式为 `${method} /${path} HTTP/${version}`

- method: http的请求方法，包含：
  - GET: GET 方法代表查询服务器资源。需要注意的是GET方法不应该带 request body
  - POST: POST 方法代表发送数据到服务器。可以带有 request body
  - HEAD: 同GET方法，但是语义上是告诉服务器返回时不需要返回response body；不带request body
  - OPTIONS: 查询对应URL支持的http方法
  - PUT: 修改服务器资源（不存在时创建），可以带request body
  - DELETE: 删除服务器资源。rfc规范中没有明确DELETE可不可以带body
  - TRACE: 请求服务器返回请求的信息，使得客户端可以知道中间的服务器对这个请求做了什么操作
  - CONNECT: 可以开启一个客户端与所请求资源之间的双向沟通的通道。它可以用来创建隧道（tunnel）
  - PATCH: 对资源做出部分更改
- path: http请求的url，其结构为：${protocol}://${hostname}:${port}/${pathname}?${search}#${hash}
  - protocol: 指定使用的协议，常见: http, https
  - hostname: 地址，一般为域名或ip地址
  - pathname: 资源位于服务器的相对位置
  - search: 查询字符串
  - hash: 锚点
- version:
  - http 0.9: 仅支持GET方法，整个协议请求只有一行 `GET 文档路径`
  - http 1.0: 增加了响应状态码，HTTP头，HTTP增加HEAD和POST方法
  - http 1.1: 我们最常用的版本
  - http 2.0: spdy，2009年谷歌版本

!!! note "同源策略"

    protocol, hostname, port 完全相同的请求称之为同源请求

!!! note "search 参数重复"

    如: GET baidu.com/?test=abc&test=bcd&test=def。这样的请求在rfc规范中是没有进行定义的，取决于后端框架的实现。因为我认为定义重复参数这样的查询字符串接口是非常不好的实践。

#### headers

请求头包含这次请求很多关于客户端的信息和一些服务端需要的补充信息

**这些信息在headers部分，每行一条，格式为:`key`:`name`**

#### blank-line

headers和请求体之间有一个空行，这个行非常重要，它表示请求头已经结束，接下来的是请求正文

#### request-body

request-body代表请求体，包含本次请求的信息。在http协议中，request-body不一定存在。例如在GET方法中，标准中就规定是不含有request-body的。

### response 报文格式

响应报文的格式，和request报文格式相似

```
<reponse-line>
<headers>
<blank line>
[<response-body>]
```

#### response-line

`response-line`: 响应行的格式为 `HTTP/${version} ${status} ${message}`

- version: http的版本
- status：状态码
- message：http对应状态码的一个字符串，如 200 OK, 404 NOT FOUND

## 代码中关于http常量定义

### http状态码

- (100, CONTINUE,                        Continue)
- (101, SWITCHING_PROTOCOLS,             Switching Protocols)
- (102, PROCESSING,                      Processing)
- (200, OK,                              OK)
- (201, CREATED,                         Created)
- (202, ACCEPTED,                        Accepted)
- (203, NON_AUTHORITATIVE_INFORMATION,   Non-Authoritative Information)
- (204, NO_CONTENT,                      No Content)
- (205, RESET_CONTENT,                   Reset Content)
- (206, PARTIAL_CONTENT,                 Partial Content)
- (207, MULTI_STATUS,                    Multi-Status)
- (208, ALREADY_REPORTED,                Already Reported)
- (226, IM_USED,                         IM Used)
- (300, MULTIPLE_CHOICES,                Multiple Choices)
- (301, MOVED_PERMANENTLY,               Moved Permanently)
- (302, FOUND,                           Found)
- (303, SEE_OTHER,                       See Other)
- (304, NOT_MODIFIED,                    Not Modified)
- (305, USE_PROXY,                       Use Proxy)
- (307, TEMPORARY_REDIRECT,              Temporary Redirect)
- (308, PERMANENT_REDIRECT,              Permanent Redirect)
- (400, BAD_REQUEST,                     Bad Request)
- (401, UNAUTHORIZED,                    Unauthorized)
- (402, PAYMENT_REQUIRED,                Payment Required)
- (403, FORBIDDEN,                       Forbidden)
- (404, NOT_FOUND,                       Not Found)
- (405, METHOD_NOT_ALLOWED,              Method Not Allowed)
- (406, NOT_ACCEPTABLE,                  Not Acceptable)
- (407, PROXY_AUTHENTICATION_REQUIRED,   Proxy Authentication Required)
- (408, REQUEST_TIMEOUT,                 Request Timeout)
- (409, CONFLICT,                        Conflict)
- (410, GONE,                            Gone)
- (411, LENGTH_REQUIRED,                 Length Required)
- (412, PRECONDITION_FAILED,             Precondition Failed)
- (413, PAYLOAD_TOO_LARGE,               Payload Too Large)
- (414, URI_TOO_LONG,                    URI Too Long)
- (415, UNSUPPORTED_MEDIA_TYPE,          Unsupported Media Type)
- (416, RANGE_NOT_SATISFIABLE,           Range Not Satisfiable)
- (417, EXPECTATION_FAILED,              Expectation Failed)
- (421, MISDIRECTED_REQUEST,             Misdirected Request)
- (422, UNPROCESSABLE_ENTITY,            Unprocessable Entity)
- (423, LOCKED,                          Locked)
- (424, FAILED_DEPENDENCY,               Failed Dependency)
- (426, UPGRADE_REQUIRED,                Upgrade Required)
- (428, PRECONDITION_REQUIRED,           Precondition Required)
- (429, TOO_MANY_REQUESTS,               Too Many Requests)
- (431, REQUEST_HEADER_FIELDS_TOO_LARGE, Request Header Fields Too Large
- (451, UNAVAILABLE_FOR_LEGAL_REASONS,   Unavailable For Legal Reasons)
- (500, INTERNAL_SERVER_ERROR,           Internal Server Error)
- (501, NOT_IMPLEMENTED,                 Not Implemented)
- (502, BAD_GATEWAY,                     Bad Gateway)
- (503, SERVICE_UNAVAILABLE,             Service Unavailable)
- (504, GATEWAY_TIMEOUT,                 Gateway Timeout)
- (505, HTTP_VERSION_NOT_SUPPORTED,      HTTP Version Not Supported)
- (506, VARIANT_ALSO_NEGOTIATES,         Variant Also Negotiates)
- (507, INSUFFICIENT_STORAGE,            Insufficient Storage)
- (508, LOOP_DETECTED,                   Loop Detected)
- (510, NOT_EXTENDED,                    Not Extended)
- (511, NETWORK_AUTHENTICATION_REQUIRED, Network Authentication Required

### http 请求方法

- (0,  DELETE,      DELETE)
- (1,  GET,         GET)
- (2,  HEAD,        HEAD)
- (3,  POST,        POST)
- (4,  PUT,         PUT)
- (5,  CONNECT,     CONNECT)
- (6,  OPTIONS,     OPTIONS)
- (7,  TRACE,       TRACE)
- (8,  COPY,        COPY)
- (9,  LOCK,        LOCK)
- (10, MKCOL,       MKCOL)
- (11, MOVE,        MOVE)
- (12, PROPFIND,    PROPFIND)
- (13, PROPPATCH,   PROPPATCH)
- (14, SEARCH,      SEARCH)
- (15, UNLOCK,      UNLOCK)
- (16, BIND,        BIND)
- (17, REBIND,      REBIND)
- (18, UNBIND,      UNBIND)
- (19, ACL,         ACL)
- (20, REPORT,      REPORT)
- (21, MKACTIVITY,  MKACTIVITY)
- (22, CHECKOUT,    CHECKOUT)
- (23, MERGE,       MERGE)
- (24, MSEARCH,     M-SEARCH)
- (25, NOTIFY,      NOTIFY)
- (26, SUBSCRIBE,   SUBSCRIBE)
- (27, UNSUBSCRIBE, UNSUBSCRIBE)
- (28, PATCH,       PATCH)
- (29, PURGE,       PURGE)
- (30, MKCALENDAR,  MKCALENDAR)
- (31, LINK,        LINK)
- (32, UNLINK,      UNLINK)


## 参考资料

[http 版本历史](https://zhuanlan.zhihu.com/p/23366045)
[http headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
[开源解析器 http-parser, fast-parser](http://www.cnblogs.com/arnoldlu/p/6497837.html)