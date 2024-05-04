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

There is also another way to inject services is to use `token`.

```ts
// cats.module.ts
@Module({
  controllers: [CatsController],
  providers: [{
    provide: 'CATS_SERVICE',
    useClass: 'CatsService'
  }],
})

// cats.controller.ts
export class CatsController {
  constructor(@Inject('CATS_SERVICE') private readonly catsService: CatsService) {}
  // other code
}
```

## Flows

<https://i.stack.imgur.com/2lFhd.jpg>

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

## Exception

Built-in exception

```ts
throw new HttpException('User not found', HttpStatus.BAD_REQUEST)
```

Custom exception: use when we don't want to duplicate the message

```ts
export class UserNotFoundException extends HttpException {
  constructor(msg?: string, status?: HttpStatus) {
    super(msg || 'User not found', status || HttpStatus.BAD_REQUEST)
  }
}
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

## Middleware and interceptors

- Interceptors have access to response/request before and after the route handler is called.
- Middleware is called only before the route handler is called. You have access to the response object, but you don't have the result of the route handler

The execution order is:
Middleware -> Interceptors -> Route Handler -> Interceptors -> Exception Filter (if exception is thrown)

## Websockets

First, we define a websocket gateway. Websocket is now listening for the clients to connnect.

```ts
@WebSocketGateway()
export class EventsGateway {}
```

At this time, clients can connect to websocket. Now, clients might want to send messages via an event and server need to listen to that event. In this example, server are listening to an event named 'events'. `body` will be the message body that clients send to server

```ts
@WebSocketGateway()
export class EventsGateway {
  @SubscribeMessage('events')
  handleMessage(@MessageBody() body: any) {}
}
```

Upon clients send messages, server might also want to broadcast messages to clients. Server will `emit` an event, named 'onMessage' in this example. All the clients that listen to this event will receive the message from server.

```ts
handleMessage(@MessageBody() body: any) {
  this.server.emit('onMessage', {
    msg: 'New message',
    content: body,
  });
}
```
