This is the **under-the-hood engine** that powers almost everything in NestJS (Modules, Controllers, Providers, DI, routes, guards, pipes, etc.). It hasn’t changed in any fundamental way in 2025-2026 — NestJS v11 (and the upcoming v12) still relies on exactly the same mechanism.
### 1. What Are Decorators?
In TypeScript, a **decorator** is a special function that you attach to a class, method, property, or parameter using the @ symbol.
```TypeScript
@Controller('cats')        // ← This is a decorator (class level)
export class CatsController {

  @Get()                   // ← This is a decorator (method level)
  findAll() { ... }
}
```
Decorators are **not** executed when the file is imported. Instead, they run **at runtime** and attach **metadata** to the class/method.
### 2. Metadata Reflection (The Real Magic)
NestJS uses the official reflect-metadata library (already installed in every NestJS project) to **store and read** this metadata.
- When you write @Controller('cats'), NestJS internally calls Reflect.defineMetadata(...)
- Later, when the app bootstraps, NestJS reads all this metadata using Reflect.getMetadata(...)
This is how NestJS knows:
- Which classes are modules
- Which classes are controllers
- What routes exist
- What parameters to inject
- Which pipes/guards/interceptors to apply
### 3. How It Actually Works (Simple Example)
```TypeScript
// This is what NestJS does behind the scenes (simplified)
import 'reflect-metadata';

function Controller(prefix: string) {
  return function (target: any) {
    // Attaches metadata to the class
    Reflect.defineMetadata('path', prefix, target);
    Reflect.defineMetadata('isController', true, target);
  };
}

function Get(path: string = '') {
  return function (target: any, propertyKey: string) {
    const existingRoutes = Reflect.getMetadata('routes', target) || [];
    existingRoutes.push({ method: 'GET', path, propertyKey });
    Reflect.defineMetadata('routes', existingRoutes, target);
  };
}
```
When your app starts, NestJS scans every class and reads:
- 'isController'
- 'path'
- 'routes'
- 'injectables' (for DI)
- etc.
### 4. Most Important Built-in Decorators You Already Used

|Decorator|What metadata it attaches|
|---|---|
|@Module()|imports, controllers, providers, exports|
|@Controller()|base path + marks as controller|
|@Get(), @Post()…|HTTP method + route path|
|@Injectable()|marks class as injectable|
|@Body(), @Param()|tells DI what to inject into method params|

### 5. Most Useful Advanced Decorators (You’ll Use Soon)
- **@SetMetadata(key, value)** – attach any custom data
- **Reflector** – class that lets you **read** metadata at runtime (used in guards, interceptors)
Example (very common pattern):
```TypeScript
// roles.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const Roles = (...roles: string[]) => SetMetadata('roles', roles);
```

```TypeScript
// roles.guard.ts (later topic)
import { Reflector } from '@nestjs/core';

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.get<string[]>('roles', context.getHandler());
    // check if user has required roles...
  }
}
```
### 6. Required tsconfig.json Settings (Must Have)
Every NestJS project has these two lines enabled:
JSON
```
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```
These are what allow decorators + metadata reflection to work.
### 7. Key Takeaways (2025-2026)
- Decorators = just functions that add metadata.
- Metadata reflection = the mechanism NestJS uses to “see” your @Controller, @Get, @Injectable, etc.
- This is why you never manually register routes — NestJS reads the metadata automatically.
- You can create your own custom decorators (very powerful and clean).
This concept is what makes NestJS **declarative** instead of imperative.
---
