## nestjs服务

Nestjs 中的服务可以是 service 也可以是 provider。

他们都可以通过 constructor 注 入依赖关系。

服务本质上就是通过@Injectable() 装饰器注解的类。

在 Nestjs 中服务相 当于 MVC的model。

view - controller - model



## nestjs中创建和使用服务

创建服务

```
nest g service user
```

使用服务

1. 需要在根模块引入并配置
2. 在用到的地方引入并配置

