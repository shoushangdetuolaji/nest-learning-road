## nestjs中配置静态资源

> [MVC | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/mvc)

```
app.useStaticAssets('public');
```

在入口文件配置main.ts

```ts
import { NestFactory } from '@nestjs/core';
import { NestExpressApplication } from '@nestjs/platform-express';
import { join } from 'path';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create<NestExpressApplication>(AppModule);
  // app.useStaticAssets('public')
  // http://localhost:3000/1.jpg
  app.useStaticAssets(join(__dirname, '..', 'public'), {
    prefix: '/static/' // 设置虚拟路径 http://localhost:3000/static/1.jpg
  })
  app.setBaseViewsDir('views');
  app.setViewEngine('ejs');
  await app.listen(3000);
}
bootstrap();
```

## nestjs配置模板引擎

安装对应的模板引擎 这里用ejs

```
npm i ejs --save
```

配置模板引擎

```
app.setBaseViewsDir(join(__dirname, '..', 'views')) // 放视图的文件
app.setViewEngine('ejs');
```

渲染页面

```ts
import { Get, Controller, Render } from '@nestjs/common';

@Controller()
export class AppController {
	@Get()
	@Render('index')
	root() {
		return { message: 'Hello world!' };
	}
}
```

项目根目录新建views目录然后新建index.ejs

```ejs
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
	这是 ejs 演示代码
	<br>
	<%=message%>
</body>
</html
```



## nestjs中模板引擎结合post请求以及路由跳转

```TS
import { Controller, Get, Post, Body,Response, Render} from '@nestjs/common';

@Controller('user')
export class UserController {
	@Get()
	@Render('default/user')
	index(){
		return {"name":"张三"};
	}

    @Post('doAdd')
	doAdd(@Body() body,@Response() res) {
		console.log(body);
		res.redirect('/user'); //路由跳转
	}
}
```

模板

```ejs
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title>Document</title>
	<link rel="stylesheet" href="/static/base.css"><!-- 静态资源目录 -->
</head>
<body>
	<img src="/static/1.png" alt="" />
	<br>
	<form action="/user/doAdd" method="post">
		<input type="text" name="username" placeholder="请输入用户名" />
		<br>
		<br>
		<input type="text" name="age" placeholder="年龄" />
		<br>	
		<br>
		<input type="submit" value="提交">
	</form>
</body>
</html>
```

