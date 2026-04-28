### 1. Bootstrap / NestFactory / main.ts (Entry Point)

1. What is the purpose of the main.ts file in a NestJS application?
2. What is the bootstrap() function and why is it async?
3. What does NestFactory.create(AppModule) actually do?
4. How do you start the HTTP server and apply basic global settings (like port) in main.ts?

### 2. Modules (@Module() decorator)
5. What is a Module in NestJS and why does NestJS use a modular architecture?
6. Explain the four main properties of the @Module() decorator (imports, controllers, providers, exports).
7. What is the Root Module (AppModule) and how is it different from a Feature Module?
8. Why do we need to register controllers and providers inside a module?
### 3. Controllers (@Controller() + HTTP method decorators)
9. What is the role of a Controller in NestJS?
10. How does NestJS map routes using @Controller() and HTTP method decorators (@Get(), @Post(), etc.)?
11. What are the common parameter decorators (@Body(), @Param(), @Query()) and what do they extract from the request?
### 4. Providers / Services (@Injectable() classes)
12. What is a Provider in NestJS? What is the most common type of Provider?
13. What does the @Injectable() decorator do?
14. Why do we put business logic in Services instead of putting it directly inside Controllers?
### 5. Dependency Injection (DI / IoC container)
15. What is Dependency Injection (DI) in NestJS?
16. What is Inversion of Control (IoC)?
17. How does NestJS automatically inject dependencies into a constructor?
18. What are the main benefits of using Dependency Injection in NestJS?
### 6. Decorators & Metadata Reflection
19. What are Decorators in NestJS? Give some examples of built-in decorators.
20. What is Metadata Reflection and how does NestJS use it?
21. Why do we need experimentalDecorators and emitDecoratorMetadata in tsconfig.json?

### 7. Basic Architecture Flow

22. Briefly explain the overall flow of an HTTP request in NestJS (from main.ts until the response is sent).
23. How do Modules, Controllers, Providers, and Dependency Injection work together in a basic NestJS application?