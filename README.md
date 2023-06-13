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
## Step 4: Make a todo.controller.ts file in src/controller folder.
## Step 5: Make a todo.service.ts file in src/provider folder.
## Step 6: Make a todo.module.ts file in src/module folder.
## Step 7: run the project.


