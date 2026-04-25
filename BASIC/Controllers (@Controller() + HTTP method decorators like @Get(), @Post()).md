### 1. What is a Controller?
- A **class** decorated with @Controller()
- It defines **routes** (URLs) and what should happen when a request hits them
- Each method in the controller becomes a **route handler**
- Controllers are **registered** in a Module (that’s why we learned Modules first)
### 2. Basic Syntax (Official 2026 Style)

```TypeScript
// src/cats/cats.controller.ts
import { Controller, Get, Post, Body, Param } from '@nestjs/common';
import { CatsService } from './cats.service';
import { CreateCatDto } from './dto/create-cat.dto';

@Controller('cats')                    // ← base path prefix
export class CatsController {
  constructor(private readonly catsService: CatsService) {}
  @Get()                              // GET /cats
  findAll() {
    return this.catsService.findAll();
  }
  @Get(':id')                         // GET /cats/123
  findOne(@Param('id') id: string) {
    return this.catsService.findOne(id);
  }
  @Post()                             // POST /cats
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }
}
```
### 3. All Standard HTTP Method Decorators (2026)

| Decorator  | HTTP Method | Typical Use Case          | Example Route         |
| ---------- | ----------- | ------------------------- | --------------------- |
| @Get()     | GET         | Fetch data                | @Get() or @Get(':id') |
| @Post()    | POST        | Create new resource       | @Post()               |
| @Put()     | PUT         | Replace entire resource   | @Put(':id')           |
| @Delete()  | DELETE      | Delete resource           | @Delete(':id')        |
| @Patch()   | PATCH       | Partial update            | @Patch(':id')         |
| @Options() | OPTIONS     | CORS preflight / metadata | @Options()            |
| @Head()    | HEAD        | Headers only (no body)    | @Head()               |
| @All()     | ALL         | Catch any method (rare)   | @All()                |
### 4. Most Important Parameter Decorators (used inside methods)

| Decorator        | What it extracts                    | Common Example                  |
| ---------------- | ----------------------------------- | ------------------------------- |
| @Body()          | Request body (JSON)                 | @Body() dto: CreateCatDto       |
| @Param('id')     | URL path parameter                  | @Param('id') id: string         |
| @Query()         | Query string (?page=1&limit=10)     | @Query() query: any             |
| @Headers()       | HTTP headers                        | @Headers('authorization') token |
| @Res() / @Next() | Raw Express/Fastify response object | (use only when needed)          |

### 5. How It Connects to the Module (Important!)
You **must register** the controller in its module:
```TypeScript
// src/cats/cats.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],   // ← register here
  providers: [CatsService],
  exports: [CatsService],          // optional
})
export class CatsModule {}
```

Then import CatsModule into AppModule (or another module).
### 6. Real-World Best Practices (2025-2026)
- Always use a **feature folder** structure:
     
    ```
    src/cats/
      ├── cats.controller.ts
      ├── cats.service.ts
      ├── cats.module.ts
      └── dto/
    ```
    
- Use **DTOs** with validation (we’ll cover this soon).
- Keep controller methods **thin** — only call service methods.
- Return plain objects/arrays → NestJS automatically converts to JSON.
- Use async methods when talking to database:
    ```TypeScript
    @Get()
    async findAll(): Promise<Cat[]> {
      return this.catsService.findAll();
    }
    ```
    
