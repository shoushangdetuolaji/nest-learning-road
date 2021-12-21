### 关于nestjs中的守卫

守卫是一个使用 @Injectable() 装饰器的类。 守卫应该实现 CanActivate 接口。 

守卫有一个单独的责任。它们确定请求是否应该由路由处理程序处理。到目前为止，访问限 制逻辑大多在中间件内。这样很好，因为诸如 token 验证或将 request 对象附加属性与 特定路由没有强关联。但中间件是非常笨的。它不知道调用 next() 函数后会执行哪个处 理程序。另一方面，守卫可以访问 ExecutionContext 对象，所以我们确切知道将要执行 什么。 



说白了：在 Nextjs 中如果我们想做权限判断的话可以在守卫中完成，也可以在中间件中完 成。



### nestjs中创建守卫、以及控制器中单独配置守卫

创建守卫

`nest g guard auth`

```TS
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common';
import { Observable } from 'rxjs';
@Injectable()
export class AuthGuard implements CanActivate {
	canActivate(
		context: ExecutionContext, ): boolean | Promise<boolean> | Observable<boolean> {
		return true;
	}
}
```

使用守卫

```TS
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
  
    //做权限判断
    // var n = Math.random();
    // console.log('守卫执行了'+n);
    // if (n > 0.5) {
    //   return true;
    // }

    var req=context.switchToHttp().getRequest();
    if(req.path=='/admin/login'){
      return true;
    }else{
      var userinfo=context.switchToHttp().getRequest().session.username;

      if(userinfo){
        return true;
      }
      return false;

    }

  }
}

```



全局使用守卫

```
app.useGlobalGuards(new AuthGuard());
```



### 在nestjs中的守卫中获取cookie和session

```TS
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common';
import { Observable } from 'rxjs';
@Injectable()
export class AuthGuard implements CanActivate {
	canActivate(
		context: ExecutionContext, ): boolean | Promise<boolean> | Observable<boolean> {
		console.log(context.switchToHttp().getRequest().cookies);
		console.log(context.switchToHttp().getRequest().session);
		return true;
	}
}

```

