### 微服务安全设计方案

#### 传统单体应用的访问安全设计

**无 session 时**：验证用户身份信息为其创建session。

1. 验证当前用户的credential。

2. 获取当前用户的identity。

3. 为当前用户创建并启用session。

**有 session 时**：直接验证session。

1. 验证当前 session 是否有效。

#### 微服务架构下的访问安全设计

- 单点登录
- 无状态
- 细粒度鉴权

微服务架构下的访问安全常用设计方案

- HTTP Basic Authentication + Independent Auth DB（独享 Auth DB）
- HTTP Basic Authentication + Central Auth DB（共用 Auth DB）
- API Tokens
- SAML

#### Spring Cloud Security 解决方案

UAA 是由CloudFoundry发起的，也是CloudFoundry平台的身份管理服务。

UAA 主要功能是基于 OAuth2，当用户访问客户端应用时，生成并发放 token 给目标客户端。

UAA认证服务包含如下几个方面的内容：

- 认证对象：用户、客户端以及目标资源服务器。
- 认证类型：授权码模式 ( Authorization Code )、密码模式 ( Resource Owner Paassword Credential ) 以及客户端模式 ( Client Credential )。
- 认证范围 ( Scope ) ：即认证权限，并作为一个命名的参数附加到 AccessToken 上。













