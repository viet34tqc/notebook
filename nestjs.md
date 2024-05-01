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

In NestJS, if we want to inject a service into another service, we need to use the decorator `@Injectable()` at the top of the service. All the injectable class must be added into `providers` array of that module. 

## Flows

We often define services in modules as `providers`. In services, we will inject other services

## Data transfer object

DTO - Data transfer object is basically a schema and defines how the data is sent over the network. For example, you want to send a POST request, the DTO will be the type of the data in your request body

```ts
createUser(@Body userData: CreateUserDto)
```

## Validation and transform data

When the user send requests to `Controller`, you might want to validate or transform the data before it reachs the controller. This step is called `Pipe`. `Pipe` provides many methods via 2 packages: `class-validator` and `class-transformer`, which you need to install to use pipe in NestJS. 

Here, we are using `class-validator` to validate the data of the DTO

```ts
import {IsString, IsInt} from 'class-validator';
export class CreateUserDTO {
  @IsString()
  name: string

  @IsInt()
  age: number
}
```

We can also transform the data before sending request. We have two type of transform: auto transformation and explicit transformation

Here is auto transformation. The payload will be transform automatically into the types that matches the DTO. For example, if the `age` in the payload is string, it will be converted into number, according to the `CreateUserDTO` above

```ts
@Post()
@UsePipes(new ValidationPipe({ transform: true }))
async create(@Body() createUserDTO: CreateUserDTO) {
  this.catsService.create(createUserDTO);
}
```

Alternatively, we can explicitly cast values using the `ParseIntPipe` or `ParseBoolPipe`, expecially useful when we work with params and query because by default, every path parameter and query parameter comes over the network as a string

```ts
@Get(':id')
findOne(
  @Param('id', ParseIntPipe) id: number,
  @Query('sort', ParseBoolPipe) sort: boolean,
) {
  console.log(typeof id === 'number'); // true
  console.log(typeof sort === 'boolean'); // true
  return 'This action returns a user';
}
```

## Common Decorators

- `@Injectable()`: put at the top of the service, so that service can be injected into another service or module
- `@Body()`: get the request body
- `@Params()`: get the params object

```ts
// Route: /user/:id
@Get(":id")
// The name 'id' is what you pass into @Get(':id')
getUserById(@Params('id') id: string){}
```

- `Query()`: get the query object

```ts
// Route: /user/?sortBy
@Get('users')
getUsers(@Query('sortBy') sortBy: string) {}
```

## Export and import

In order to use service in another module (not that service's module), you need to export it first in its module and import the module where you want to use, just like `import` and `export` js modules

```ts
// prisma.module.ts
@Module({
  exports: [PrismaService],
})

// And we want to use prisma service auth service
// We need to import it first in auth.module.ts
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

## Guards

Use to protect private route

`Guard` often need `strategy` to implement

What `strategy`'s `validation` method returns is pass to the request body

For example:

Let's say we have a login strategy by JWT

- From strategy, we extract the payload from token, then we query the user in db using the data in payload
- Strategy has a name, ex: 'jwt'. We create a `guard` using that strategy name
- In controller, if the protected route use the `guard` created above, it will includes the data we return from `strategy` in the request body.
- We can also use a custom decorator to extract the data we want to send to the service
- We pass that data to the service and implement our business