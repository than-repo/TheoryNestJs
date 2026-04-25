### 1. What is Bootstrap?
- src/main.ts is the **entry point** of your entire NestJS application (just like index.js in a plain Node.js app).
- "Bootstrap" = the process of **starting up** the application:
    - Creating the NestJS application instance
    - Initializing all modules, providers, controllers, etc.
    - Starting the HTTP server so it can receive requests
- Everything happens inside an async function bootstrap().
### 2. The Official Basic main.ts (2025-2026)
This is the **exact minimal code** you will see in every new NestJS project created with the CLI:
```TypeScript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```
### 3. Line-by-Line Breakdown (Super Important for Beginners)

| Line                                             | Explanation                                                                                                                                                                                   |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| import { NestFactory } from '@nestjs/core';      | Imports the core factory class that creates the NestJS app.                                                                                                                                   |
| import { AppModule } from './app.module';        | Imports your **root module** (the starting point of your whole app structure).                                                                                                                |
| async function bootstrap()                       | All startup logic goes here. It must be async because create() and listen() return Promises.                                                                                                  |
| const app = await NestFactory.create(AppModule); | **The most important line** — creates the full NestJS application instance (including Dependency Injection container, all modules, etc.). Returns an object that implements INestApplication. |
| await app.listen(process.env.PORT ?? 3000);      | Starts the HTTP server. Uses the PORT environment variable if it exists, otherwise defaults to port 3000.                                                                                     |
| bootstrap();                                     | Actually calls the function to start everything.                                                                                                                                              |
### 4. What Happens Behind the Scenes (Simple Explanation)
1. Node.js runs main.ts
2. NestFactory.create() builds the entire application tree using your AppModule
3. All @Module(), @Controller(), @Injectable() classes are instantiated
4. app.listen() starts the web server (Express by default)
5. Your app is now ready to handle HTTP requests
### 5. Common Real-World main.ts (What Most Developers Actually Use)
Here’s a more practical version you will see in almost every production project in 2025-2026:
```TypeScript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ValidationPipe } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Global configurations (highly recommended)
  app.useGlobalPipes(new ValidationPipe({ whitelist: true, transform: true }));
  app.enableCors();
  app.setGlobalPrefix('api');

  const configService = app.get(ConfigService);
  const port = configService.get<number>('PORT') ?? 3000;

  await app.listen(port);
  console.log(`Application is running on: http://localhost:${port}`);
}
bootstrap();
```
### 6. Useful Options You Can Pass to NestFactory.create()
You can pass a second argument with options:
```TypeScript
const app = await NestFactory.create(AppModule, {
  abortOnError: false,        // Don't exit process on bootstrap error
  logger: ['error', 'warn'],  // Control logging level
  // snapshot: true,          // Useful for debugging in some cases
});
```
### 7. Quick Tips & Best Practices (2025-2026)
- Always keep main.ts clean — put **global settings** here (pipes, CORS, prefixes, etc.).

- Use ConfigService (from @nestjs/config) for ports and environment variables.

- Enable enableShutdownHooks() if you need graceful shutdown (good for production).
-`enableShutdownHooks()` lets Nest **catch shutdown signals** and run cleanup logic.
(has: - DB connections (Postgres, Mongo)
Message queues (Kafka, RabbitMQ)
Background jobs
 Open sockets / streams)
 
- The app works with **Express** by default. You can switch to **Fastify** for better performance (we’ll cover that later).