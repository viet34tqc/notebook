# Nest Framework

## Common commands

- `nest new <PROJECT_NAME>`: create new Nest project. You will need to install `@nest/cli` first
- `npx nest generate resource <YOUR_SERVICE>`: create resource. This command will create the boilerplate code for your service.

## Concepts

NestJS runs in `Module` structure. You will have many modules in the app, ex: user module, authentication module...

Each module has a seperate `Controller` to handle incoming request an return responses to the client. For each route, the Controller will invoke a `Service` method to perform the logic. A service is a `Provider`, which can be injectable. So, before invoking a method from `Service` class, we have to add the service to the `providers` array of that module. 

```ts
// cats.module.ts
@Module({
  controllers: [CatsController],
  providers: [CatsService],
})

export class CatsController {
  constructor(private readonly catsService: CatsService) {}
  // other code
}
```

This is called Dependancy Injection. Without DI, in the Controller class, we have to intiate the Service class first => code is cumbersome

In NestJS, if we want to inject other module into a class, we need to use the decorator `@Injectable()` at the top of the class. All the injectable class must be added into `providers` array of that module. 

When the user send requests to `Controller`, you might want to validate or transform the data before it reachs the controller. This step is called `Pipe`. `Pipe` provides many methods via 2 packages: `class-validator` and `class-transformer`, which you need to install to use pipe in NestJS. We will apply this pipe in DTO class

```ts
import {IsString, IsInt} from 'class-validator';
export class CreateUserDTO {
  @IsString()
  name: string

  @IsInt()
  age: number
}
```

Then we applied `ValidationPipe` to where we need validation

```ts
@Post()
  create(
    @Body(ValidationPipe)
    createUserDto: CreateUserDto,
  ) {
    return this.usersService.create(createUserDto);
}
```

## Flows

We often use services in modules. In services, we will inject other services

## Export and import

In order to use service in another module, you need to export it first in its module and import the module where you want to use, just like `import` and `export` js modules

```ts
// prisma.module.ts
@Module({
  exports: [PrismaService],
})

// And we want to use prisma service in auth.module.ts
@Module({
  imports: [PrismaModule],
})
```

However, if you are finding that you are importing it in many places, you might want to make your imported module globally

```ts
// By using global, you don't need to import PrismaModule everywhere
@Global()
@Module({
  providers: [PrismaService],
  exports: [PrismaService],
})
export class PrismaModule {}
```


