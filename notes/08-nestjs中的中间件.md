### 关于nestjs中间件

中间件就是匹配路由之前或者匹配路由完成做的一系列的操作。

中间件中如果想往下匹配的话，那么需要些next()



Client Side --->(httpRequest) --->Middleware ----> Route Handler



nestjs的中间件实际上等价于express中间件。

中间件函数可以执行的任务：

```
执行任何代码。
对请求和响应对象进行更改。
结束请求-响应周期。
调用堆栈中的下一个中间件函数。
如果当前的中间件函数没有结束请求-响应周期，它必须调用next()将控制传递给下一个中间件函数。否则请求会被挂起。
```

nest中间件可以是一个函数，可以是一个带有@Injectable()装饰器的类



### nestjs中创建使用中间件

创建中间件

`nest g middleware init`

```ts
import { Injectable, NestMiddleware } from '@nestjs/common';
@Injectable()
export class InitMiddleware implements NestMiddleware {
	use(req: any, res: any, next: () => void) {
		console.log('init');
		next();
	}
}
```

配置中间件

在app.module.ts中继承NestModule然后配置中间件

```ts
export class AppModule implements NestModule {
	configure(consumer: MiddlewareConsumer) {
		consumer
			.apply(InitMiddleware)
			.forRoutes({ path: '*', method: RequestMethod.ALL })
			.apply(NewsMiddleware). forRoutes({ path: 'news', method: RequestMethod.ALL })
            .apply(UserMiddleware). forRoutes({ path: 'user', method: RequestMethod.GET },{ path: '', method:
RequestMethod.GET });
	}
}
```





### 多个中间件

```
consumer.apply(cors(), helmet(), logger).forRoutes(CatsController);
```



### 函数式中间件

```
export function logger(req, res, next) {
	console.log(`Request...`);
	next();
};
```



### 全局中间件

```ts
const app = await NestFactory.create(ApplicationModule);
app.use(logger);
await app.listen(3000);
```

