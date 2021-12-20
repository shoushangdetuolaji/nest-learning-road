## nestjs中session

> session是一种记录客户状态的机制，不同的是cookie保存再客户端浏览器中，而session保存再服务器上



### session的工作流程

当浏览器访问服务器并发送第一次请求时，服务器端会创建一个session对象，生成一个类似于key，value的键值对，然后将key(cookie)返回到浏览器(客户)端，浏览器下次再访问时，携带key(cookie)找到对应的session(value)。客户的信息都保存在session中



### nestjs中的express-session的使用

> [Session | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/session#session)

#### 安装 express-session

```
npm i express-session --save
```

```
npm i -D @types/express-session --save
```

#### 引入express-session

```
import * as session from 'express-session'
```

#### 设置官方文档提供的中间件

```
app.use(session({secret: 'keyboard cat', cookie: {maxAge: 60000}}))
```

#### 使用

```
# set
req.session.username = '张三'
# get
req.session.username
```



### express-session的常用参数

```
app.use(session({
	secret: '12345',
	name: 'name',
	cookie: {maxAge: 60000},
	resave: false,
	saveUninitialized: true
}))
```



| 参数              | 作用                                                         |
| ----------------- | ------------------------------------------------------------ |
| secret            | 一个String类型的字符串，作为服务器端生成session的签名        |
| name              | 返回客户端的key的名称，默认为connect.sid 可以自己设置        |
| resave            | 强制保存session即使它并没有变化，默认为true，建议设置成false |
| saveUninitialized | 强制将未初始化的 session 存储。当新建了一个 session 且未设定属性或值时，它就处于 未初始化状态。在设定一个 cookie 前，这对于登陆验证，减轻服务端存储压力，权限控制是有帮助的。（默 认：true）。建议手动添加。 |
| cookie            | 设置返回到前端 key 的属性，默认值为{ path: ‘/’, httpOnly: true, secure: false, maxAge: null } |
| rolling           | 在每次请求时强行设置 cookie，这将重置                        |





### express-session的常用方法：

```
req.session.destroy(function(err) { /*销毁 session*/
})
req.session.username='张三'; //设置 session
req.session.username //获取 session
req.session.cookie.maxAge=0; //重新设置 cookie 的过期时
```

