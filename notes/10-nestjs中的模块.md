### 关于nestjs中的模块

模块是具有@Module()装饰器的类

@Module()装饰器提供了元数据

Nest用来组织应用程序结构

> [Modules | NestJS - A progressive Node.js framework](https://docs.nestjs.com/modules)



每个nest应用程序至少有一个模块，根模块。

根模块是nest开始安排应用程序树的地方。

拥有多个模块，每个模块都有一组紧密相关的功能。



@Module()装饰器接受一个描述模块属性的对象：v

| providers   | 由 Nest 注入器实例化的提供者，并且可以 至少在整个模块中共享 |
| ----------- | ----------------------------------------------------------- |
| controllers | 必须创建的一组控制器                                        |
| imports     | 导入模块的列表，这些模块导出了此模块中 所需提供者           |
| exports     | 由本模块提供并应在其他模块中可用的提供 者的子集             |





### nestjs中创建模块

`nest g module admin`

- 根模块
  - 后台管理模块
  - 前台模块
  - api接口模块
  - 其他模块



### nestjs中的共享模块

实际上，每个模块都是一个共享模块。一旦创建就能被任意模块重复使用。假设我们将在几 个模块之间共享 CatsService 实例。 我们需要把 CatsService 放到 exports 数组中，如下所示

```ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';
@Module({
    controllers: [CatsController],
    providers: [CatsService],
    exports: [CatsService]
})
export class CatsModule {}
```





