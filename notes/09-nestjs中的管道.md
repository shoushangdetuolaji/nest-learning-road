### 关于nestjs中的管道

> [Documentation | NestJS - A progressive Node.js framework](https://docs.nestjs.com/pipes)

nestjs中的管道可以将输入数据转换为所需的输出。

它可以处理验证，当数据不正确时可能会抛出异常。

### nestjs中创建和使用管道

创建管道

`nest g pipe user`

管道创建完成后生成如下代码：

```TS
import { ArgumentMetadata, Injectable, PipeTransform } from '@nestjs/common';

@Injectable()
export class UserPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return value;
  }
}
```

使用管道

```ts
import { Controller, Get, Post, Body, Response, Render, Request, UsePipes, Query } from '@nestjs/common';
import { UserPipe } from '../../pipe/user.pipe';

@Controller('user')
export class UserController {

  @Get()
  @Render('default/user')
  index(){
    return {"name":"张三"};
  }

  @Get('pipe')
  @UsePipes(new UserPipe())
  pipe(@Query() info) {
    console.log(info);
    return `this is PipePage`;
  }
}

```





### nestjs中管道结合Joi库实现数据验证

Joi库 : [GitHub - sideway/joi: The most powerful data validation library for JS](https://github.com/sideway/joi)

```TS
// user.pipe.ts
import { ArgumentMetadata, Injectable, PipeTransform } from '@nestjs/common';

@Injectable()
export class UserPipe implements PipeTransform {
  private schema;
  constructor(schema) {
    this.schema = schema;
  }
  transform(value: any, metadata: ArgumentMetadata) {
    const { error } = this.schema.validate(value);
    if (error) {
      // do something for error
      return {
        "success": false
      };
    }
    return value;
  }
}
```

```ts
// user.controller.ts
import { Controller, Get, Post, Body, Response, Render, Request, UsePipes, Query } from '@nestjs/common';
import { UserPipe } from '../../pipe/user.pipe';

import * as Joi from 'joi';
let userSchema = Joi.object().keys({
  name: Joi.string().required(),
  age: Joi.number().integer().min(6).max(66).required()
})

@Controller('user')
export class UserController {

  @Get()
  @Render('default/user')
  index(){
    return {"name":"张三"};
  }

  @Get('pipe')
  @UsePipes(new UserPipe(userSchema))
  pipe(@Query() info) {
    console.log(info);
    return `this is PipePage`;
  }
}

```



