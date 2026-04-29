- The **Root Module** (almost always named AppModule) is the **starting point** of the whole NestJS application.
- It is the only module we pass directly to NestFactory.create(AppModule).
- Its main job is to **import all other modules** (feature modules, global modules, config modules, etc.) so NestJS can build the full dependency tree and start the app.

- A **Feature Module** (also called business or domain module) is different — it groups everything related to one specific feature. For example: UserModule, ProductModule, AuthModule, OrderModule.
- **Key differences:**

| Aspect       | Root Module (AppModule)                                 | Feature Module                                                   |
| ------------ | ------------------------------------------------------- | ---------------------------------------------------------------- |
| **Scope**    | Global — orchestrates the whole app                     | Focused — one specific feature/domain                            |
| **Contents** | Mostly imports; rarely has its own controllers/services | Contains its own controllers, services, repositories, DTOs, etc. |
| **Role**     | Entry point & dependency organizer                      | Self-contained building block                                    |
| **Usage**    | Only one root module                                    | Imported by AppModule (or other feature                          |

8

- In NestJS, simply creating a controller or a service class is not enough. We **must** register them inside a module so that the framework knows they exist and should be part of the running application.
- For **controllers**: When we put them in the controllers array, NestJS scans the class, reads all the route decorators (@Get(), @Post(), etc.), and automatically registers those routes on the HTTP server. If we forget to register a controller, those endpoints simply won’t work — even if the class is perfect.
- For **providers** (services, repositories, use-cases, etc.): Registering them in the providers array tells NestJS two important things:
    1. Create an instance of that class
    2. Add it to the Dependency Injection container of that module This is exactly what makes constructor injection (private userService: UserService) work.
	- Without registration inside a module:
    - The DI container has **no knowledge** that the class exists. As a result:
    ++NestJS provides **no automatic lifecycle management** (no instantiation, no proper scoping — singleton/request/transient).
	++Constructor injection fails everywhere because the class is invisible to the framework.
    
    - The classes are treated as normal TypeScript classes with zero framework support
- This design is also what gives us clean module boundaries and encapsulation. It forces us to think about which parts should be private and which should be shared via exports.