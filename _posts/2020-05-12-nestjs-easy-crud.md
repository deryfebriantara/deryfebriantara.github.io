---
layout: post
title:  "Membuat CRUD Super Cepat Dengan Nestjs"
image: ''
date:   2020-05-15 00:06:31
tags:
- framework
description: ''
categories:
- node
serie: learn
---


<p class="music-read"><a href="spotify:track:5Z34KcxLFW9t2DKudEVcws">Music for reading(spotify)</a></p>
<img src="/assets/img/nestjs/nest.png">

## Perkenalan

Karena kesal main game moba kena lose streak, jadi saya kali ini iseng share membuat CRUD dengan cepat menggunakan nestjs framework. bagi yang belum mengenal nestjs bisa membaca terlebih dahulu di artikel <a target="_blank" href="https://medium.com/@ahmadarif/berkenalan-dengan-nestjs-e9c628b7194"> ini (berkenalan dengan nestjs oleh ahmad arif) </a> penjelasannya sangat mudah di mengerti.<br>

Di tutorial ini saya akan membuat Backend CRUD TodoList. Kita akan menggunakan `mysql` dan `TypeOrm` untuk ORM -nya. Kita akan menggunakan `nestjsx/crud` untuk operasi CRUD nya.<br>
Sebelum kita mulai pastikan sudah terinstall `mysql` dan `nestjs`.

## Kita Mulai yukz!

Pertama-tama kita buat projectnya dengan `nestjs-cli`:

{% highlight sh %}
$ nest new crud-todo
{% endhighlight %}

lalu setelah itu kita install `typeorm`, `mysql library` dan `dotenv`:

{% highlight sh %}
$ yarn add  dotenv
$ yarn add  @nestjs/typeorm typeorm mysql
{% endhighlight %}

kita buat `.env` dan koneksi database untuk konfigurasi aplikasinya:

{% highlight sh %}
$ touch .env
$ mkdir src/shared && mkdir src/shared/services
$ touch src/shared/services/database-connection.service.ts
{% endhighlight %}

kita edit `.env` yang telah di buat dan pastikan konfigurasi nya sesuai dengan laptop:

{% highlight sh %}
DATABASE_HOST = "localhost"
DATABASE_PORT = 3306
DATABASE_USER = "root"
DATABASE_PASSWORD = "root"
DATABASE_DB = "nestjs_crud"
{% endhighlight %}

sekarang kita edit `database-connection.service.ts` dan tambahkan kode berikut: 


{% highlight javascript %}
import { Injectable } from '@nestjs/common'
import { TypeOrmOptionsFactory, TypeOrmModuleOptions } from '@nestjs/typeorm'
import 'dotenv/config'

@Injectable()
export class DatabaseConnectionService implements TypeOrmOptionsFactory {

  createTypeOrmOptions(): TypeOrmModuleOptions {
    return {
      name: 'default',
      type: 'mysql',
      host: process.env.DATABASE_HOST,
      port: Number(process.env.DATABASE_PORT),
      username: process.env.DATABASE_USER,
      password: process.env.DATABASE_PASSWORD,
      database: process.env.DATABASE_DB,
      synchronize: true,
      dropSchema: false,
      logging: true,
      entities: ['dist/**/*.entity.js'],
    }
  }
}
{% endhighlight %}

setelah membuat koneksi database, sekarang kita import typeORM module di `app.module.ts` :

{% highlight javascript %}

import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { TypeOrmModule } from '@nestjs/typeorm';
import { DatabaseConnectionService } from './shared/services/database-connection.service';

@Module({
  imports: [
    TypeOrmModule.forRootAsync({
      useClass: DatabaseConnectionService
    })
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}

{% endhighlight %}

Database telah di konfigurasi, kita akan check apakah konfigurasi kita berhasil:
{% highlight javascript %}
$ yarn start:dev
{% endhighlight %}

kita akan melihat seperti ini jika koneksi database berhasil : 
<img src="/assets/img/nestjs/nest1.jpeg">

sekarang kita akan buat todo module, service dan controller:

{% highlight javascript %}
$ nest g module todo
$ nest g service todo
$ nest g controller todo
{% endhighlight %}

setelah generate module, service dan controller kita akan menginstall `nesjsx/crud` :
{% highlight javascript %}
$ yarn add @nestjsx/crud class-transformer class-validator
$ yarn add @nestjsx/crud-typeorm
{% endhighlight %}

sekarang kita buat todo entity :

{% highlight javascript %}
$ touch src/todo/todo.entity.ts
{% endhighlight %}

lalu kita edit `todo.entity.ts` :
{% highlight javascript %}
import { Entity, PrimaryGeneratedColumn, Column, CreateDateColumn, UpdateDateColumn } from "typeorm";

@Entity('todos')
export class TodoEntity {

  @PrimaryGeneratedColumn() 
  id: number;

  @Column() 
  title: string;

  @Column()
  description: string

  @Column({
    type: 'boolean',
    default: false
  })
  is_done: boolean

  @CreateDateColumn()
  create_at: Date

  @UpdateDateColumn()
  updated_at: Date
}
{% endhighlight %}

kita telah berhasil membuat table todos di database.

sekarang kita mulai membuat operasi crud nya.
kita edit `todo.service.ts` dan tambahkan kode berikut:

{% highlight javascript %}

import { Injectable } from '@nestjs/common';
import { TypeOrmCrudService } from "@nestjsx/crud-typeorm";
import { TodoEntity } from './todo.entity';
import { InjectRepository } from '@nestjs/typeorm';

@Injectable()
export class TodoService extends TypeOrmCrudService<TodoEntity>{
  constructor(@InjectRepository(TodoEntity) repo) {
    super(repo)
  }
}

{% endhighlight %}

lalu kita edit `todo.controller.ts` dan tambahkan juga kode di bawah ini:

{% highlight javascript %}

import { Controller } from '@nestjs/common';
import { Crud, CrudController } from '@nestjsx/crud';
import { TodoEntity } from './todo.entity';
import { TodoService } from './todo.service';

@Crud({
  model: {
    type: TodoEntity
  }
})

@Controller('todo')
export class TodoController implements CrudController<TodoEntity> {
  constructor(public service: TodoService) {}
}
{% endhighlight %}

dan jangan lupa tambahkan di `app.module.ts` :

{% highlight javascript %}

import { Module } from '@nestjs/common';
import { TodoService } from './todo.service';
import { TodoController } from './todo.controller';
import { TypeOrmModule } from '@nestjs/typeorm';
import { TodoEntity } from './todo.entity';

@Module({
  imports: [
    TypeOrmModule.forFeature([TodoEntity])
  ],
  providers: [TodoService],
  controllers: [TodoController]
})
export class TodoModule {}

{% endhighlight %}

Sebenarnya kita sudah beres membuat operasi crud sampai disini, bingung ga? kenapa ga ada  @Post, @Get, @Put, @Patch, @Delete methods? itu karena `nestjsx/crud` telah meng-handle semua fungsi tersebut. <br>

sekarang kita akan setup OpenAPI, kita akan menggunakan nestjs swagger:

{% highlight javascript %}
$ yarn add @nestjs/swagger swagger-ui-express

{% endhighlight %}

setelah menginstall kita edit `main.ts`:
{% highlight javascript %}

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  const options = new DocumentBuilder()
    .setTitle('Todo Crud')
    .setDescription('The todo API description')
    .setVersion('1.0')
    .addTag('todo')
    .build();
  const document = SwaggerModule.createDocument(app, options);
  SwaggerModule.setup('api/docs', app, document);

  await app.listen(3000);
}
bootstrap();

{% endhighlight %}

lalu sekarang buka browser dan akses ke `localhost:3000/api/docs`:


<img src="/assets/img/nestjs/nest2.png">

sekarang kita bisa lihat bebagai macam todo endpoints, tapi kayanya ada yang kurang. kita harus membuat api tags dan field.

kalau begitu mari kita buat DTO nya:
{% highlight javascript %}
$ touch src/todo/todo.dto.ts
{% endhighlight %}

dan tambahkan kode berikut di `todo.dto.ts`

{% highlight javascript%}

import { ApiProperty } from '@nestjs/swagger';

export class CreateTodoDto {
  @ApiProperty()
  title: string;

  @ApiProperty()
  description: string;

  @ApiProperty()
  is_done: boolean;

}

{% endhighlight %}

edit juga di `todo.controller.ts` :
{% highlight javascript%}
import { Controller } from '@nestjs/common';
import { Crud, CrudController } from '@nestjsx/crud';
import { TodoEntity } from './todo.entity';
import { TodoService } from './todo.service';
import { CreateTodoDto } from './todo.dto'


@Crud({
  model: {
    type: TodoEntity
  },
  dto: {
    create: CreateTodoDto,
  }
})

@Controller('todo')
export class TodoController implements CrudController<TodoEntity> {
  constructor(public service: TodoService) { }
}
{% endhighlight %}

dan sekarang kita sudah melihat fields di swaggernya:
<img src="/assets/img/nestjs/nest3.png">

`nestjsx/crud` lengkap dengan bebagai query untuk kebutuhan pagination dan searching

<img src="/assets/img/nestjs/nest4.png">


## Kesimpulan

`nestjsx/crud` memudahkan kita untuk membuat crud secara cepat tapi kita harus banyak membaca dokumentasinya di sesuaikan dengan kebutuhan kita.


## thank you  ......

