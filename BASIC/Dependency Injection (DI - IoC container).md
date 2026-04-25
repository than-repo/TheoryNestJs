### 1. What is Dependency Injection (DI)?
- **DI** = A design pattern where NestJS automatically creates objects and **injects** (passes) the dependencies they need.
- **IoC (Inversion of Control)** = You no longer manually new CatsService() inside your controller. The framework’s container controls the creation and lifetime of all objects.
- Result: Your code becomes **loosely coupled**, easier to test, and much more maintainable.
In plain English: Instead of you wiring everything together, NestJS’s **DI container** does it automatically.

By default, NestJS creates one instance of each `@Injectable()` (singleton) and reuses it across the app. However, depending on scope, it can also create new instances per request or per injection.
### 2. How DI Works in NestJS (2025-2026)
NestJS has a **built-in DI container** (powered by TypeScript’s reflection metadata). Here’s the exact flow:
1. You mark a class with @Injectable()
2. You register it in a module’s providers array
3. You ask for it in a constructor using private readonly xxxService: XxxService
4. NestJS sees the type and **automatically injects** the correct instance
### 3. Live Example (Connecting Everything We’ve Learned)
```TypeScript
// cats.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()   // ← Tells NestJS: "I can be injected"
export class CatsService {
  private readonly cats: any[] = [];
  findAll() {
    return this.cats;
  }
  create(cat: any) {
    this.cats.push(cat);
    return cat;
  }
}
```

```TypeScript
// cats.controller.ts
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CatsService } from './cats.service';   // ← Import the class

@Controller('cats')
export class CatsController {

  // ← Dependency Injection happens here!
  constructor(
    private readonly catsService: CatsService   // ← No `new` keyword!
  ) {}
  @Get()
  findAll() {
    return this.catsService.findAll();   // ← Service is already ready
  }
  @Post()
  create(@Body() cat: any) {
    return this.catsService.create(cat);
  }
}
```

```TypeScript
// cats.module.ts
@Module({
  controllers: [CatsController],
  providers: [CatsService],     // ← Register once here
})
export class CatsModule {}
```

**That’s it.** No manual instantiation anywhere.
### 4. What the DI Container Actually Does Behind the Scenes
When your app starts (NestFactory.create(AppModule)):
1. NestJS scans all modules
2. Builds a **dependency graph**
3. Creates instances in the correct order (resolving dependencies first)
4. Reuses the same instance (singleton scope by default)
5. Injects the instances wherever they are requested
This graph is built only once at startup — very fast in production.
### 5. Most Common DI Patterns You’ll Use Every Day

| Pattern               | Code Example                          | When to Use                      |
| --------------------- | ------------------------------------- | -------------------------------- |
| Constructor Injection | constructor(private service: Service) | 99% of cases (standard)          |
| Property Injection    | @Inject() service: Service (rare)     | Only in very special cases       |
| Optional Dependency   | @Optional() private service?: Service | When dependency may not exist    |
| Custom Token          | @Inject('CUSTOM_TOKEN')               | Advanced (third-party libraries) |
|                       |                                       |                                  |
### 6. Key Rules & Best Practices (Current in 2026)
- Always use **constructor injection** — it’s the cleanest and most recommended way.
- Services are **singleton** by default (one instance for the whole app).
- You can change scope later if needed:
    - SCOPE_DEFAULT (singleton)
    - SCOPE_REQUEST (new instance per HTTP request)
    - SCOPE_TRANSIENT (new instance every time it’s injected)
- Circular dependencies can be solved with forwardRef() — we’ll cover that when we reach advanced topics.
- Makes unit testing trivial: just pass a mock in the constructor.
### 7. Why This Matters for Building Real Backends
- You can swap CatsService with a mock or a different implementation without changing any controller code.
- Your controllers stay tiny and focused only on HTTP.
- Your services stay pure and reusable across modules.
- The entire app becomes extremely maintainable even when it grows to 50+ modules.
This is exactly why big companies choose NestJS for large-scale backends.