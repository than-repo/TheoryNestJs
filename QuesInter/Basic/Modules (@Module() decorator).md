### 5. What is a Module in NestJS and why does NestJS use a modular architecture?
- A Module is a class decorated with @Module() — it acts as a container that organizes related controllers, providers, and other modules.
- NestJS uses modular architecture because of many reason:
1/in real large projects, if everything is put in one place (like a big Express app), the code becomes very hard to maintain, test, and scale.
2/Modules give us clear boundaries, better dependency management, encapsulation (hide internal services), and make it easy for a team to work on different features at the same time.
3/It also makes the code reusable and easier to test in isolation.

---
### 6. Explain the four main properties of the @Module() decorator (imports, controllers, providers, exports).###

First of all, i says about @Module() decorator.
It's what define a NestJs Module. 
It's basically a configuration object that tells NestJS how this module should be organized and connects with the rest of the application by @Import, @ Controller, @Provider, and @Export.

For Import, This is where i bring in other Module that this module needs.

Controller, This array contains all the controllers that belong to this module. 
There are classess that handle HTTP requests (routes). 
Only controllers are listed in here will be registered by NestJS for this module.

Providers, this is the most important one for me. 
It's where i put all the services, repositories, factories, et cetera.
NestJs will automatically creates the instance and manage them in the DI container.
Any class i put here becomes injectable anywhere inside this module.

And finally, Export, this array decides what providers i want to share with other modules.
By default, Provider is private- encapsulated. so, if another module imports this module and wants to use one of its services. We must to explicitly export it here. This is how we control the communication between modules. 


---
### 7. What is the Root Module (AppModule) and how is it different from a Feature Module?



---
### 8. Why do we need to register controllers and providers inside a module?