# Nest JS
Express와 마찬가지로 Node.js 서버 사이드에서 사용 가능한 프레임워크

Express 기반으로 제작되었으며, TypeScript를 사용해 서버 코드를 작성할 수 있음.

    src 
    * app.controller.ts // 싱글 라우트를 가진 간단한 샘플 컨트롤러
    * app.module.ts // Application의 root module
    * main.ts // async 함수를 포함하고, application을 bootstrap하는 역할

## Platforms

- platform-agnostic framework가 목표

> Express vs Fastify

## [Decorators](https://docs.nestjs.com/controllers)

### @Controller()

- required
- 관련된 라우터들을 그룹핑하는 역할

### @Get()

- HTTP request method decorator before the `findAll()` method tells Nest to create a handler for a specific endpoint for HTTP requests

### @Req()

- request detail에 접근할 수 있도록 하는 데코레이터
- request object를 inject해서 쓸 수 있도록 해준다.

    import { Controller, Get, Req } from '@nestjs/common';
    import { Request } from 'express';
    
    @Controller('cats')
    export class CatsController {
      @Get()
      findAll(@Req() request: Request): string {
        return 'This action returns all cats';
      }
    }

## Providers

![providers 구조](https://docs.nestjs.com/assets/Components_1.png)

### Services

- service단 코드를 정의하는 부분.
- data storage, 구조 등과 연관이 있음. `@Injectable()`과 함께 쓰여진다.

```ts
    // cats.service.ts
    
    import { Injectable } from '@nestjs/common';
    import { Cat } from './interfaces/cat.interface';
    
    @Injectable()
    export class CatsService {
      private readonly cats: Cat[] = [];
    
      create(cat: Cat) {
        this.cats.push(cat);
      }
    
      findAll(): Cat[] {
        return this.cats;
      }
    }
```

### Dependency injection

- Nest는 Dependency injection패턴으로 만들어졌다.
