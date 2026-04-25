### Basic / Fundamentals (Essential building blocks)

- **Bootstrap / NestFactory** (main.ts entry point)
- **Modules** (@Module() decorator)
- **Controllers** (@Controller() + HTTP method decorators like @Get(), @Post())
- **Providers / Services** (@Injectable() classes)
- **Dependency Injection (DI / IoC container)**
- **Decorators & Metadata reflection**

### Intermediate (Request lifecycle & request handling)

- **Middleware**
- **Pipes** (built-in + custom: ValidationPipe, ParseIntPipe, ParseUUIDPipe, etc.)
- **Guards** (authentication/authorization)
- **Interceptors** (response transformation, logging, etc.)
- **Exception Filters** (custom error handling)
- **DTOs** (Data Transfer Objects) + Validation (class-validator / class-transformer)
- **Configuration** (@nestjs/config module)

### Advanced / Production-Ready (Architecture & extensibility)

- **Custom Providers** (useClass, useValue, useFactory, useExisting)
- **Dynamic Modules** & **Global Modules**
- **Lifecycle Events** (OnModuleInit, OnApplicationBootstrap, etc.)
- **Custom Decorators**
- **Testing** (Unit, Integration, E2E with @nestjs/testing)
- **Standalone Applications** (newer pattern in v10+)

### Specialized / Advanced Backend Patterns (for complex apps)

- **Microservices** (TCP, Redis, Kafka, RabbitMQ, gRPC, etc.)
- **WebSocket Gateways** (@WebSocketGateway())
- **GraphQL** (Code-first or Schema-first with @nestjs/graphql)
- **CQRS** (Command Query Responsibility Segregation – via official or community packages)
- **Event-based architecture** (Event Emitter, Observables)