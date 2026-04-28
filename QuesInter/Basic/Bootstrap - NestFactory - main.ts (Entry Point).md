### 1. What is the purpose of the main.ts file in a NestJS application?**

- The main.ts file is the **entry point** of a NestJS application.
- It contains the bootstrap() function which is responsible for:
    - Creating the application instance using NestFactory.create(AppModule)
    - Starting the HTTP server with app.listen(port)
- It is where we usually set up global configurations such as:
    - Global pipes, interceptors, filters, guards
    - CORS, validation, etc.
- In short, main.ts bootstraps and launches the entire NestJS app.

---

### 2. What is the bootstrap() function and why is it async?**
bootstrap() is the main entry-point function (usually in main.ts) that creates and starts your application.

Because NestFactory.create() and app.listen() return Promises. You need await to properly initialize the app before it starts listening.

---
### 3. What does NestFactory.create(AppModule) actually do?
- It's a static method from "@nestjs/core" that creates the root application instance.
- it takes AppModule (RootModule) as input and builds the full module dependency tree.
- it initializes DI container, instantiates all controllers, providers, guards, etc.
- it also sets up HTTP adapter (express by default)
- it return a Promise<INestApplication> (or NestExpressApplication).

---

### 4. How do you start the HTTP server and apply basic global settings (like port) in main.ts?

After creating the root application instance by:
```ts
  const app = await NestFactory.create(AppModule);
```
Applying the global configuration:
- app.setGlobalPrefix('api')
- app.enableCors()
- app.useGlobalPipes(new ValidationPipe())
- app.useGlobalInterceptors(...) etc.

then, starts the HTTP server:
```ts
await app.listen(process.env.PORT ?? 3000);
```


