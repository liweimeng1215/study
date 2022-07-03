# oauth2.0介绍

## 学习资源

1. 极客时间《oauth2.0-王新栋》
2. [官网](https://oauth.net/2/)

## 摘要

![抽象数据流](.\oauth2.0官网抽象数据流.png)

* oauth2.0是一种授权（Authorization，受限资源访问）协议
* oauth2.0是一种安全协议
* oauth2.0不是一个认证（Authentication）协议

*oauth2.0目的在于解决客户端访问资源服务器的授权问题*

注：如果想了解一下Authorization和Authentication的详细区别，[click here](https://auth0.com/intro-to-iam/authentication-vs-authorization/)

## 最常用且最安全的模式——授权码模式

### 以web应用为例的主要流程

![授权码模式时序图](.\授权码模式时序图.png)

### 问题

* 为什么要返回授权码code，能否直接返回access_token？

    <details>
    <summary>个人理解</summary>
    1. 安全性更高
    2. 用户体验更好
    </details>

* 是不是拿到access_token就可以访问受限资源？

    <details>
    <summary>个人理解</summary>
    并不是，一般来说，第三方软件开发者还需在授权服务平台进行注册，得到app_id以及app_secret作为注册标识。
    </details>

### 令牌刷新机制

![refresh_token时序图](.\refresh_token时序图.png)

1. 除了解决功能问题上来看，为什么要有refresh_token，直接在过期前使用access_token延长有效时间不行吗？

    <details>
    <summary>个人理解</summary>
    如果使用access_token换取新的access_token，那么一个有效期内的token在未被主动注销的情况下能够一致延续未被注销的状态，
   在客户端与资源服务器频繁访问的场景下，更容易出现安全风险。
    </details>

### 一些设计，架构应用

* OIDC OpenID Connect。*用于解决用户重复注册、登录问题的一个协议。*

    1. OIDC=授权协议+身份认证。
    2. 应用场景：联合登录和单点登录。
        1. 联合登录
           ![联合登陆](.\OIDC联合登录.png)
        2. 单点登录
           ![单点登录](.\OIDC单点登录.png)


* 微服务典型架构

  ![微服务应用](.\微服务应用.png)
  
  * 分层架构
    * nginx反向代理。流量控制，反向代理
    * web应用。存储html资源
    * Gateway网关。后台服务的反向路由；令牌校验和转换，权限校验
    * IDP(Identity Provider)服务。处理Authentication和Authorization
    * BFF(Backend For Frontend)
    * 领域服务层

  * oauth2.0 token与jwt

### 其它模式

1. 资源拥有者凭证许可
   ![资源拥有者凭证许可](.\资源拥有者凭证许可.png)
2. 客户端凭证许可
   ![客户端凭证许可](.\客户端凭证许可.png)
3. 隐式许可
   ![隐式许可](.\隐式许可.png)

## 实践

* 查看并尝试调用微信等开放平台接口
* 尝试实现一个IDP服务