### nestjs file-upload

> [File upload | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/file-upload)



### 单个文件上传

upload 控件

```
nest g controller upload
```

```ts
// upload.controller.ts
import { Controller, Get, Render, UseInterceptors, Post, UploadedFile, Body } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';
import { createWriteStream, write } from 'fs';
import { join } from 'path';

@Controller('upload')
export class UploadController {
  @Get()
  @Render('default/upload')
  index() {

  }

  @Post('doAdd')
  @UseInterceptors(FileInterceptor('pic'))
  doAdd(@Body() body, @UploadedFile() file) {
    console.log(body);
    //上传图片的信息  必须在form的属性里面配置enctype="multipart/form-data"
    console.log(file);
    console.log(__dirname);
    let writeStream = createWriteStream(join(__dirname,'../../public/upload',`${Date.now()}-${file.originalname}`));
    writeStream.write(file.buffer);
    return '上传图片成功';
  }
}

```



### 多文件上传

> 利用for循环、 AnyFilesInterceptor

```ts
import { Controller, Get, Render, Post, Body, UseInterceptors, UploadedFiles } from '@nestjs/common';

import { AnyFilesInterceptor } from '@nestjs/platform-express';


import {createWriteStream} from 'fs';

import {join} from 'path';


//文档：https://docs.nestjs.com/techniques/file-upload

@Controller('uploadmany')
export class UploadmanyController {

    @Get()
    @Render('default/uploadmany')
    index() {

    }
    @Post('doAdd')
    @UseInterceptors(AnyFilesInterceptor())
    doAdd(@Body() body, @UploadedFiles() files) {
        //如果要实现图片上传的话还需要进行一些判断
        console.log(body);
        console.log(files);
        for (const file of files)         
        { 
            const writeImage = createWriteStream(join(__dirname, '../../', 'public/upload', `${Date.now()}-${file.originalname}`)); 
            writeImage.write(file.buffer); 
        
        }
        return '上传图片成功';


    }
}

```



### 注意事项

上传图片的时候Form表当中需要配置enctype=""multipart/form-data"

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
    <form action="uploadmany/doAdd" method="post" enctype="multipart/form-data">
    
        <input type="text" name="title1" id="" placeholder="新闻标题"/>

        <br> <br>

        <input type="text" name="keywords" id="" placeholder="关键词"/>

       
        <br> <br>

        <input type="file" name="pic" />

        <br> <br>


        <input type="file" name="aaaa" />

       
        <br>
        <br>

        <input type="submit" value="提交">
        
    </form>
</body>
</html>
```

