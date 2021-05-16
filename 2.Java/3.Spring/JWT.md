### JWT

#### 传统 token 机制

由于前后端分离使用 Restful 进行数据交互，HTTP 又是无状态的，所以采用 token 机制实现权限管理。

前端登录，后端根据用户信息生成一个 token 并保存到数据库或 session 中，然后将 token 传回前端，前端保存 token 到浏览器 cookie 中，在之后的请求中，前台将 token 添加到 requestHeader 中发送请求。后台通过验证 requestHeader 中 token 判断是否处于登录状态，再进行相应的响应。

设置 httpOnly 选项可以使得 JS 不能读到 cookie，可以解决 XSS 攻击，但是会引发 CSRF 跨站请求伪造。

#### Json Web Token

JWT 是一个开放标准(RFC 7519)，它定义了一种用于简洁，自包含的用于通信双方之间以 JSON 对象的形式安全传递信息的方法。JWT 可以使用 HMAC 算法或者是 RSA 的公钥密钥对进行签名。

- 简洁(Compact)：可以通过URL, POST 参数或者在 HTTP header 发送，因为数据量小，传输速度快
- 自包含(Self-contained)：负载中包含了所有用户所需要的信息，避免了多次查询数据库

**JWT 结构**：头部（header），有效负载（payload），签名（signature）。

**头部（header）**：Algorithm & TokenType

```json
{
    "alg": "HS256",
    "typ": "JWT"
}
```

**有效负载（payload）**：

- iss：jwt签发者
- sub：jwt所面向的用户
- aud：接收jwt的一方
- exp：jwt的过期时间，这个过期时间必须要大于签发时间
- nbf：定义在什么时间之前，该jwt都是不可用的.
- iat：jwt的签发时间
- jti：jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。

```json
{
    "sub": "10001",
    "username": "john",
    "role": "admin",
    "expire": "2018-08-08 20:15:56"
}
```

**签名（signature）**：

签名：使用 header 中指定的算法对头部（header），有效载荷（payload），密钥（secret）进行加密生成签名。

```
HMACSHA256(
	base64UrlEncode(header) + "." +
	base64UrlEncode(payload) + "." +
	secret
)
```

