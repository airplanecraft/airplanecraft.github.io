+++
Categories = ["Technology"]
Description = ""
Tags = ["jwt","token","jwk","jws"]
date = "2021-12-03T10:30:37+08:00"
menu = "token"
title = "JWT、JWE、JWS 、JWK 到底是什么"
toc = true
+++

### 什么是 JWT

一个JWT，应该是如下形式的： 


eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.  
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.  
TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ 

### JWT 解决什么问题？

JWT的主要目的是在服务端和客户端之间以安全的方式来转移声明。主要的应用场景如下所示：

 认证 Authentication；
 授权 Authorization // 注意这两个单词的区别；
 联合识别；
 客户端会话（无状态的会话）；
 客户端机密。

### JWT 的一些名词解释

 JWS：Signed JWT签名过的jwt
 JWE：Encrypted JWT部分payload经过加密的jwt；目前加密payload的操作不是很普及；
 JWK：JWT的密钥，也就是我们常说的 scret；
 JWKset：JWT key set在非对称加密中，需要的是密钥对而非单独的密钥，在后文中会阐释；
 JWA：当前JWT所用到的密码学算法；
 nonsecure JWT：当头部的签名算法被设定为none的时候，该JWT是不安全的；因为签名的部分空缺，所有人都可以修改。

 ### JWT的组成

一个通常你看到的jwt，由以下三部分组成，它们分别是：

 header：主要声明了JWT的签名算法；
 payload：主要承载了各种声明并传递明文数据；
 signture：拥有该部分的JWT被称为JWS，也就是签了名的JWS；没有该部分的JWT被称为nonsecure JWT 也就是不安全的JWT，此时header中声明的签名算法为none。
三个部分用·分割。形如 xxxxx.yyyyy.zzzzz的样式。


#### JWT header 


{  
  "typ": "JWT",  
  "alg": "none",  
  "jti": "4f1g23a12aa"  
} 

jwt header 的组成

头通常由两部分组成：令牌的类型，即JWT，以及正在使用的散列算法，例如HMAC SHA256或RSA。

当然，还有两个可选的部分，一个是jti，也就是JWT ID，代表了正在使用JWT的编号，这个编号在对应服务端应当唯一。当然，jti也可以放在payload中。

另一个是cty，也就是content type。这个比较少见，当payload为任意数据的时候，这个头无需设置，但是当内容也带有jwt的时候。也就是嵌套JWT的时候，这个值必须设定为jwt。这种情况比较少见。

jwt header 的加密算法

加密的方式如下： 


base64UrlEncode(header)  
>> eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIiwianRpIjoiNGYxZzIzYTEyYWEifQ 

####  JWT payload 


{  
  "iss": "http://shaobaobaoer.cn",  
  "aud": "http://shaobaobaoer.cn/webtest/jwt_auth/",  
  "jti": "4f1g23a12aa",  
  "iat": 1534070547,  
  "nbf": 1534070607,  
  "exp": 1534074147,  
  "uid": 1,  
  "data": {  
    "uname": "shaobao",  
    "uEmail": "shaobaobaoer@126.com",  
    "uID": "0xA0",  
    "uGroup": "guest"  
  }  
} 

jwt payload的组成payload通常由三个部分组成，分别是 Registered Claims ; Public Claims ; Private Claims ;每个声明，都有各自的字段。

Registered Claims

 iss  【issuer】发布者的url地址
 sub 【subject】该JWT所面向的用户，用于处理特定应用，不是常用的字段
 aud 【audience】接受者的url地址
 exp 【expiration】 该jwt销毁的时间；unix时间戳
 nbf  【not before】 该jwt的使用时间不能早于该时间；unix时间戳
 iat   【issued at】 该jwt的发布时间；unix 时间戳
 jti    【JWT ID】 该jwt的唯一ID编号
Public Claims 这些可以由使用JWT的那些标准化组织根据需要定义，应当参考文档IANA JSON Web Token Registry。

Private Claims 这些是为在同意使用它们的各方之间共享信息而创建的自定义声明，既不是注册声明也不是公开声明。上面的payload中，没有public claims只有private claims。

jwt payload 的加密算法

加密的方式如下： 


base64UrlEncode(payload)  
>>eyJpc3MiOiJodHRwOi8vc2hhb2Jhb2Jhb2VyLmNuIiwiYXVkIjoiaHR0cDovL3NoYW9iYW9iYW9lci5jbi93ZWJ0ZXN0L2p3dF9hdXRoLyIsImp0aSI6IjRmMWcyM2ExMmFhIiwiaWF0IjoxNTM0MDcwNTQ3LCJuYmYiOjE1MzQwNzA2MDcsImV4cCI6MTUzNDA3NDE0NywidWlkIjoxLCJkYXRhIjp7InVuYW1lIjoic2hhb2JhbyIsInVFbWFpbCI6InNoYW9iYW9iYW9lckAxMjYuY29tIiwidUlEIjoiMHhBMCIsInVHcm91cCI6Imd1ZXN0In19 

暴露的信息

所以，在JWT中，不应该在载荷里面加入任何敏感的数据。在上面的例子中，我们传输的是用户的User ID，邮箱等。这个值实际上不是什么敏感内容，一般情况下被知道也是安全的。但是像密码这样的内容就不能被放在JWT中了。如果将用户的密码放在了JWT中，那么怀有恶意的第三方通过Base64解码就能很快地知道你的密码了。

当然，这也是有解决方案的，那就是加密payload。在之后会说到

#### JWS 的概念

JWS 的结构

JWS ，也就是JWT Signature，其结构就是在之前nonsecure JWT的基础上，在头部声明签名算法，并在最后添加上签名。创建签名，是保证jwt不能被他人随意篡改。

为了完成签名，除了用到header信息和payload信息外，还需要算法的密钥，也就是secret。当利用非对称加密方法的时候，这里的secret为私钥。

为了方便后文的展开，我们把JWT的密钥或者密钥对，统一称为JSON Web Key，也就是JWK。

jwt signature 的签名算法 

复制
RSASSA || ECDSA || HMACSHA256(  
  base64UrlEncode(header) + "." +  
  base64UrlEncode(payload),  
  secret)  
>>GQPGEpixjPZSZ7CmqXB-KIGNzNl4Y86d3XOaRsfiXmQ  
>># 上面这个是用 HMAC SHA256生成的 
到目前为止，jwt的签名算法有三种。

 对称加密HMAC【哈希消息验证码】：HS256/HS384/HS512
 非对称加密RSASSA【RSA签名算法】（RS256/RS384/RS512）
 ECDSA【椭圆曲线数据签名算法】（ES256/ES384/ES512）
最后将签名与之前的两段内容用.连接，就可以得到经过签名的JWT，也就是JWS。 

复制
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiIsImp0aSI6IjRmMWcyM2ExMmFhIn0.eyJpc3MiOiJodHRwOi8vc2hhb2Jhb2Jhb2VyLmNuIiwiYXVkIjoiaHR0cDovL3NoYW9iYW9iYW9lci5jbi93ZWJ0ZXN0L2p3dF9hdXRoLyIsImp0aSI6IjRmMWcyM2ExMmFhIiwiaWF0IjoxNTM0MDcwNTQ3LCJuYmYiOjE1MzQwNzA2MDcsImV4cCI6MTUzNDA3NDE0NywidWlkIjoxLCJkYXRhIjp7InVuYW1lIjoic2hhb2JhbyIsInVFbWFpbCI6InNoYW9iYW9iYW9lckAxMjYuY29tIiwidUlEIjoiMHhBMCIsInVHcm91cCI6Imd1ZXN0In19.GQPGEpixjPZSZ7CmqXB-KIGNzNl4Y86d3XOaRsfiXmQ 
1.
当验证签名的时候，利用公钥或者密钥来解密Sign，和 base64UrlEncode(header) + "." + base64UrlEncode(payload) 的内容完全一样的时候，表示验证通过。

JWS 的额外头部声明

如果对于CA有些概念的话，这些内容会比较好理解一些。为了确保服务器的密钥对可靠有效，同时也方便第三方CA机构来签署JWT而非本机服务器签署JWT，对于JWS的头部，可以有额外的声明，以下声明是可选的，具体取决于JWS的使用方式。如下所示：

 jku: 发送JWK的地址；最好用HTTPS来传输
 jwk: 就是之前说的JWK
 kid: jwk的ID编号
 x5u: 指向一组X509公共证书的URL
 x5c: X509证书链
 x5t：X509证书的SHA-1指纹
 x5t#S256: X509证书的SHA-256指纹
 typ: 在原本未加密的JWT的基础上增加了 JOSE 和 JOSE+ JSON。JOSE序列化后文会说及。适用于JOSE标头的对象与此JWT混合的情况。
 crit: 字符串数组，包含声明的名称，用作实现定义的扩展，必须由 this->JWT的解析器处理。不常见。

### A JWT typically looks like this:

![jwt sample](/images/jwtsample.png)

![jwt diagram](/images/jwt.jpeg)