### 1. What is a Provider?
- Any class decorated with **@Injectable()**
- It can be **injected** (reused) anywhere in your app via Dependency Injection
- NestJS calls them “providers” because they **provide** something useful (services, repositories, factories, strategies, etc.)
- The most common provider is a **Service**
### 2. Basic Syntax (Official 2026 Style)
```TypeScript
// src/cats/cats.service.ts
import { Injectable } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { Cat } from './interfaces/cat.interface';

@Injectable()                          // ← This is the magic decorator
export class CatsService {
  private readonly cats: Cat[] = [];   // fake database for demo
  findAll(): Cat[] {
    return this.cats;
  }
  findOne(id: string): Cat | undefined {
    return this.cats.find(cat => cat.id === id);
  }
  create(createCatDto: CreateCatDto): Cat {
    const cat: Cat = {
      id: Date.now().toString(),
      ...createCatDto,
    };
    this.cats.push(cat);
    return cat;
  }
}
```
### 3. How Providers Connect Everything (Important!)
You must **register** the service in its module:
```TypeScript
// src/cats/cats.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],     // ← Register here
  exports: [CatsService],       // ← Optional: make it reusable by other modules
})
export class CatsModule {}
```
Now the controller can **use** the service:
```TypeScript
// src/cats/cats.controller.ts
import { Controller, Get, Post, Body, Param } from '@nestjs/common';
import { CatsService } from './cats.service';
import { CreateCatDto } from './dto/create-cat.dto';

@Controller('cats')
export class CatsController {
  constructor(
    private readonly catsService: CatsService   // ← Injected automatically
  ) {}

  @Get()
  findAll() {
    return this.catsService.findAll();
  }

  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }
}
```
### 4. Types of Providers (Beyond Simple Services)

| Type            | Decorator / Pattern             | Common Use Case                          |
| --------------- | ------------------------------- | ---------------------------------------- |
| Service         | @Injectable()                   | Business logic (what we use 90% of time) |
| Repository      | @Injectable()                   | Data access layer                        |
| Factory         | useFactory in module            | Dynamic creation of objects              |
| Custom Provider | useClass, useValue, useExisting | Advanced DI overrides                    |

### 5. Key Rules You Must Remember (2025-2026)

- Every @Injectable() class must be listed in **some module’s providers array** (or come from an imported module).
- Once registered, NestJS’s **Dependency Injection container** automatically creates and injects it wherever you ask (via constructor).
- Services are **singleton** by default (one instance for the whole app) → very efficient.
- You can change scope later (REQUEST scope for per-request services, TRANSIENT for new instance every time) — but you don’t need that yet.
### 6. Real-World Best Practices (Current in 2026)
- One service per feature (e.g. UsersService, AuthService, PaymentsService)
- Keep services **pure** (no HTTP decorators inside)
- Use async/await for any database or external calls
- Name files clearly: xxx.service.ts
- Never put business logic in controllers