### 1. What is a Module?
- A **module** is a class decorated with @Module().
- It acts as a **container** (or "box") that groups together:
    - Related controllers
    - Services / providers
    - Other modules
- It tells NestJS: "These things belong together and should be managed as one unit."
- Every app has **at least one module** called the **root module** (AppModule).
- Modules make your code **organized, reusable, and testable**.
### 2. The @Module() Decorator (Official Structure)
```TypeScript

// src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],          // ← Import other modules
  controllers: [],      // ← HTTP controllers
  providers: [],        // ← Services + other injectable classes
  exports: [],          // ← What this module exposes to others
})
export class AppModule {}
```
### 3. Breakdown of Each Property (2025-2026)

| Property    | What it does                                                           | When to use it                               | Example                                 |
| ----------- | ---------------------------------------------------------------------- | -------------------------------------------- | --------------------------------------- |
| imports     | Imports **other modules** so you can use their exported providers      | When you need services from another module   | imports: [UsersModule, DatabaseModule]  |
| controllers | Registers HTTP controllers (routes)                                    | Always for REST/GraphQL/WebSocket            | controllers: [AppController]            |
| providers   | Registers injectable classes (services, repositories, factories, etc.) | Almost every business logic                  | providers: [AppService, UserRepository] |
| exports     | Makes providers available to **other modules** that import this one    | When another module needs to reuse a service | exports: [AppService]                   |

**Important rule**: A provider (service) can only be injected in a module where it is either:
- Declared in that module’s providers, **or**
- Exported from a module listed in imports
### 4. Real Example: Root Module (AppModule)

This is what a clean AppModule looks like in most real projects in 2025-2026:
```TypeScript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { UsersModule } from './users/users.module';
import { DatabaseModule } from './database/database.module';
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({ isGlobal: true }),   // global config
    DatabaseModule,
    UsersModule,                                 // feature module
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```
### 5. Feature Modules (Best Practice)
Instead of putting everything in AppModule, you split your app into **feature modules** (recommended way in 2025-2026):
text
```
src/
├── users/
│   ├── users.module.ts
│   ├── users.controller.ts
│   ├── users.service.ts
│   └── dto/
├── products/
│   ├── products.module.ts
│   ├── products.controller.ts
│   └── products.service.ts
├── app.module.ts
```

```TypeScript
// src/users/users.module.ts
import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';

@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService],        // ← other modules can use UsersService
})
export class UsersModule {}
```

### 6. Key Concepts You Must Remember

- **Root Module** (AppModule): Imported in main.ts → everything starts here.
- **Global Modules**: You can make a module global with global: true so you don’t have to import it everywhere (e.g. ConfigModule).
- **Dynamic Modules**: Some modules (like TypeOrmModule, ConfigModule) have .forRoot() or .forFeature() — we’ll cover these when we reach advanced topics.
- Modules create **module boundaries** — this is how NestJS keeps your Dependency Injection clean and scoped.

### 7. Quick Tips & Best Practices (2025-2026)

- Keep AppModule as small as possible — only import other modules.
- Use **feature modules** for every major part of your app (users, auth, products, orders, etc.).
- Never put business logic directly in AppModule.
- A module can import itself? No. But it can re-export things.