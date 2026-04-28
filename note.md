- The **Root Module** (almost always named AppModule) is the **starting point** of the whole NestJS application.
- It is the only module we pass directly to NestFactory.create(AppModule).
- Its main job is to **import all other modules** (feature modules, global modules, config modules, etc.) so NestJS can build the full dependency tree and start the app.

- A **Feature Module** (also called business or domain module) is different — it groups everything related to one specific feature. For example: UserModule, ProductModule, AuthModule, OrderModule.
- **Key differences**:
    - Root Module (AppModule) has a **global scope** — it wires everything together. Usually it has very few (or even zero) controllers and providers of its own. It mainly contains imports.
    - Feature Module has a **focused scope** — it contains its own controllers, services, repositories, etc. for that particular feature.
    - Feature modules are **imported** into the root module (or sometimes into other feature modules). They are not the entry point.
- In real projects, this separation keeps the codebase clean and scalable. If the app grows to 20–30 modules, everything still stays organized because the root module acts like the “main” and feature modules act like independent building blocks.
8

- In NestJS, simply creating a controller or a service class is not enough. We **must** register them inside a module so that the framework knows they exist and should be part of the running application.
- For **controllers**: When we put them in the controllers array, NestJS scans the class, reads all the route decorators (@Get(), @Post(), etc.), and automatically registers those routes on the HTTP server. If we forget to register a controller, those endpoints simply won’t work — even if the class is perfect.
- For **providers** (services, repositories, use-cases, etc.): Registering them in the providers array tells NestJS two important things:
    1. Create an instance of that class
    2. Add it to the Dependency Injection container of that module This is exactly what makes constructor injection (private userService: UserService) work.
- Without registration inside a module:
    - The Dependency Injection system has no idea these classes exist
    - Lifecycle management (singleton, request-scoped, etc.) doesn’t happen
    - The classes are treated as normal TypeScript classes with zero framework support
- This design is also what gives us clean module boundaries and encapsulation. It forces us to think about which parts should be private and which should be shared via exports.