# 1
$ git clone https://github.com/nestjs/typescript-starter.git project
$ cd project
$ npm install
$ npm run start

# 2
$ npm i -g @nestjs/cli
$ nest new my-nestjs-01

# 3
## main.ts: 
Entry point of application. 
By using the NestFactory.create() method a new Nest application instance is created.

## app.module.ts: 
Contains the implementation of the application’s root module.

## app.controller.ts: 
Contains the implementation of a basic NestJS controller with just one route.

## app.service.ts: 
Contains the a basic service implementation.

## app.controller.spec.ts: 
Testing file for controller.

# 4
## Step 1: Edit the main.ts file.

```sh
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

declare const module: any;

async function bootstrap() {

  const app = await NestFactory.create(AppModule, {cors: true});

  app.listen(5000)
  .then(() => {
    console.log("successfully stared on port 5000");
  })
  .catch((error) => {
    console.log(error);
  })
}
bootstrap();
```

## Step 2: Install typeorm and make a database.config.ts file in src/config folder.
$ npm install — save @nestjs/typeorm typeorm pg

```sh
import { PostgresConnectionOptions } from "typeorm/driver/postgres/PostgresConnectionOptions";
 
export const devConfig: PostgresConnectionOptions = {
  type: 'postgres',
  host: 'localhost',
  port: 5432,
  username: 'postgres',
  password: '1234',
  database: 'AWT',
  entities: ["dist/**/*.entity{.ts,.js}"],
  synchronize: true,
}
```

## Step 3: Make a Todo.entity.ts file in src/entity folder.

```sh
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';
 
@Entity('Todo')
export class Todo {
  @PrimaryGeneratedColumn()
  id: number;
 
  @Column()
  name: string;
 
  @Column({ default: false })
  complete: boolean;
}
```

## Step 4: Make a todo.controller.ts file in src/controller folder.

```sh
import { Body, Controller, Delete, Get, Param, Post, Put, Req } from '@nestjs/common';
import { Request } from 'express';
import { TodoInterface, TodosService } from 'src/provider/todo.service';
interface CreateTodoDto {
  name: string,
  complete: boolean
}
@Controller('cats')
export class TodosController {
constructor(private todosService: TodosService) {}
@Post()
  async create(@Body() createTodoDto: CreateTodoDto) {
    const todo = await this.todosService.create(createTodoDto);
    if(!todo) {
      return 'error in creating todo'
    }
    return 'todo created successfully'
  }
@Get()
  async findAll(@Req() request: Request) {
    const cats: Array<TodoInterface> = await this.todosService.findAll()
    return cats
  }
@Put(':id')
  async update(@Param('id') id: string, @Body() body: any) {
    const newCat: any = await this.todosService.update(id, body)
    return "cat updated";
  }
@Delete(':id')
  async remove(@Param('id') id: string) {
    await this.todosService.delete(id)
    return "cat deleted"
  }
}
```

## Step 5: Make a todo.service.ts file in src/provider folder.

```sh
import { Body, Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Todo } from 'src/entity/Todo.entity';
import { Repository } from 'typeorm';
export interface TodoInterface {
  name: string,
  complete: boolean,
}
@Injectable()
export class TodosService {
  constructor(
    @InjectRepository(Todo)
    private todoRepository: Repository<TodoInterface>,
  ) {}
create(todo: TodoInterface): Promise<TodoInterface> {
    return this.todoRepository.save(
      this.todoRepository.create(todo)
    );
  }
findAll(): Promise<TodoInterface[]> {
    return this.todoRepository.find();
  }
update(id: string, data: any): Promise<any> {
    return this.todoRepository
    .createQueryBuilder()
    .update()
    .set({
      name: data.name
    })
    .where('id = :id', { id })
    .execute()
  }
delete(id: string): Promise<any> {
    return this.todoRepository
    .createQueryBuilder()
    .delete()
    .from(Todo)
    .where('id = :id', { id })
    .execute()
  }
}
```
## Step 6: Make a todo.module.ts file in src/module folder.

```sh
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { Todo } from 'src/entity/Todo.entity';
import { TodosController } from '../controller/todo.controller';
import { TodosService } from '../provider/todo.service';
@Module({
  imports: [TypeOrmModule.forFeature([Todo])],
  controllers: [TodosController],
  providers: [TodosService],
})
export class TodosModule {}
```
## Step 7: run the project.

npm run start:dev

