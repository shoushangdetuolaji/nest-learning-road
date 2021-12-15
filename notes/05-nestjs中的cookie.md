## nestjs中cookie

> 客官来subway的曲奇吗，大块甜死人



### cookie的简介

- HTTP 是无状态协议。简单地说，当你浏览了一个页面，然后转到同一个网站的另一个页 面，服务器无法认识到这是同一个浏览器在访问同一个网站。每一次的访问，都是没有任何 关系的。如果我们要实现多个页面之间共享数据的话我们就可以使用 Cookie 或者 Session 实 现
-  cookie 是存储于访问者的计算机中的变量。可以让我们用同一个浏览器访问同一个域 名的时候共享数据。

### cookie特点

- cookie保存在浏览器本地
- 正常设置的cookie是不加密的，用户可以自由查看
- 用户可以删除cookie，或者禁用
- cookie可以被篡改
- cookie可以用于攻击
- cookie存储量小，未来实际上被localstorage替代，但是后者ie9兼容



### nestjs中使用cookie

> nestjs中使用cookie的话我们可以用cookie-parser来实现

>[Cookies | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/cookies#cookies)



#### 安装

```
npm install cookie-parser --save
```

#### 在main.ts中引入cookie-parser

```
import * as cookieParser from 'cookie-parser'
```

#### 在main.ts配置中间件

```
app.use(cookieParser());
```

#### 设置cookie

```
res.cookie("name", 'zhangsan', {maxAge: 900000, httpOnly: true});
// HttpOnly 默认false不允许 客户端脚本访问
```

#### 获取cookies

```
@Get('getCookies')
getCookies(@Request() req) {
	return req.cookies.name
}
```



### nestjs中cookie中的一些参数

| Property | type    | description                                                  |
| -------- | ------- | ------------------------------------------------------------ |
| domain   | String  | 域名                                                         |
| expires  | Date    | 过 期 时 间 （ 秒 ） ， 在 设 置 的 某 个 时 间 点 后 该 Cookie 就 会 失 效 ， 如 expires=Wednesday, 09-Nov-99 23:12:40 GMT |
| httpOnly | Boolean | 是微软对 COOKIE 做的扩展。如果在 COOKIE 中设置了“httpOnly”属性，则通 过程序（JS 脚本、applet 等）将无法读取到 COOKIE 信息，防止 XSS 攻击产生 |
| maxAge   | String  | 最大失效时间（毫秒），设置在多少后失效                       |
| path     | String  | 表示 cookie 影响到的路，如 path=/。如果路径不能匹配时，浏览器则不发送这 个 Cookie |
| secure   | Boolean | 当 secure 值为 true 时，cookie 在 HTTP 中是无效，在 HTTPS 中 |
| signed   | Boolean | 表 示 是 否 签 名 cookie, 设 为 true 会 对 这 个 cookie 签 名 ， 这 样 就 需 要 用 res.signedCookies 而不是 res.cookies 访问它。被篡改的签名 cookie 会被服务器拒绝，并且 cookie 值会重置为它的原始值 |
|          |         |                                                              |



#### 设置cookie

```
res.cookie('remember', '1', {maxAge: 900000, httpOnly: true})
res.cookie('name', 'tobi', {damain: '.example.com', path: '/admin', secure: true})
res.cookie('rememberme', '1', {expires: new Date(Date.now() + 900000), httpOnly: true})
```

#### 获取 cookie

```
req.cookies.name
```

#### 删除cookie

```
res.cookie('rememberme', '', { expires: new Date(0)});
res.cookie('username','zhangsan',{domain:'.ccc.com',maxAge:0,httpOnly:true});
```



### nestjs中cookie加密

1. 配置中间件的时候需传参

   ```
   app.use(cookieParser('123456'))
   ```

2. 设置cookie的时候配置signed属性

   ```
   res.cookie('userinfo','hahaha',{domain:'.ccc.com',maxAge:900000,httpOnly:true,signed:true});
   
   ```

3. signedCookies调用设置的cookie

   ```
   console.log(req.signedCookies)
   ```

   

