# Nest Framework

## Why use

- Modular architechture => easier to organize your code and reuse code
- Use DI => easier to manage dependancies, ensure your app is modular

## Common commands

- `nest new <PROJECT_NAME>`: create new Nest project. You will need to install `@nest/cli` first
- `npx nest generate resource <YOUR_SERVICE>`: create resource. This command will create the boilerplate code for your service.

## Concepts

### Module

NestJS runs in `Module` structure. You will have a root module and many other modules in the app, ex: user module, authentication module, etc. Root module imports other modules.

### Controller

Each module has a seperate `Controller` to handle incoming request an return responses to the client. Controllers are classes that are anotated with `Controller` decorator. 

The methods in the Controller are used to handle incoming requests. Those methods are anotated HTTP method decorator like `Get()` or `Post()` decorator...

```ts
@Controller()

class CatsController {
  @Get()
  getCats() {
    retrun 'List of cats'
  }
}
```

### Provider

Most of the code we will be using in NestJs is within providers. Provider is simply a class that can be injected in other classes as dependancy. They are anotated with `Injectable()` decorator

### Services

Normally, for each route, `Controller` will invoke a `Service` method to perform the logic. A `Service` is a `Provider`, which can be injectable. 

In order to inject a service into a controller, we have to add that service to the `providers` array of the module. 

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

The pattern that we using above with Service is called `Dependancy Injection`. Without DI, in the Controller class, we have to intiate the Service class first => code is cumbersome

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

## Pipes

NestsJs use pipes for validation and transform data

<img src="https://i.imgur.com/MTuZ1An.png">

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

Then, we apply the validation using `ValidationPipe` class

```ts
@UsePipes(new ValidationPipe())
@Controller('polls')
export class CatsController {}
```

Or we can apply validation for each route

```ts
@Post()
async create(
  @Body(new ValidationPipe()) createCatDto: CreateCatDto,
) {
  this.catsService.create(createCatDto);
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

- `exports`: exports the current features (services,...) so that other modules can import to use 
- `imports`: allows us to import other modules' features into our module

You need to export the service in its module first, and then import that module wherever you want to use the service, similar to how you import and export JavaScript modules.

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

## Exception filter

NestJs uses Exception filter to catch and process errors from routes and return it to client

<img src="https://i.imgur.com/eG9eniZ.png">

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

It's like interceptor and sits before route handler to determine if a request can be handled by the route handler or not

**Use cases**: Authorization, use to protect private route

Here is the basic implementation of `guard`. Basically, when using guard, we can get access to ExecutionContext, that allows us to inspect some details about the request.

```ts
// auth.guard.ts
@Injectable()
export class AuthGuard implements CanActivate {
  // The returned value of this method indicates whether or not the request is allow to proceed
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> { 
    return true // Or false.
  }
}

// user.controller.ts
@Get()
@UseGuards(AuthGuard)
getUser() {
  return {
    name: 'test',
  };
}
```

There are 3 scopes where you can apply your guard: route handler, controller and global. Here is an example of global guard

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalGuards(new AuthGuard())
  await app.listen(3000);
}
bootstrap();
```

`Guard` often needs `strategy` to implement

What `strategy`'s `validation` method returns is pass to the request body

For example:

Let's say we have a login strategy by JWT

- From strategy, we extract the payload from token, then we query the user in db using the data in payload
- Strategy has a name, ex: 'jwt'. We create a `guard` using that strategy name
- In controller, if the protected route use the `guard` created above, it will includes the data we return from `strategy` in the request body.
- We can also use a custom decorator to extract the data we want to send to the service
- We pass that data to the service and implement our business

## Authentication

In NestJS, we often use Passport for authentication. Basically, what it does are:

- Authenticate user's credentials (like username/password, JWT)
- Issue JWT token
- Attach user information in the next request (`Guard` sits in front of route handler, then what we return from `validation` method in strategy is returned for the next request if it pass the guard) for further use

When using Passport with NestJs, we need to create a strategy, then we pass it to `AuthGuard` from `Passport`. For example, if we need to implement an authentication mechanism using Jwt and Passport, first, we need to create `JwtStrategy` that extends `PassportStrategy`

```ts
import { Injectable } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { ExtractJwt, Strategy } from 'passport-jwt';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: 'abc123',
    });
  }

  validate(payload: any) {
    // The payload includes jwt payload (such as username, email...) and other jwt information (iat, exp)
    return payload;
  }
}
```

Add this `JwtStrategy` to `providers`

Then we will attach it to `AuthGuard` from `Passport`. When put a bearer jwt token in the request header, `AuthGuard` calls `JwtStrategy` validation method. For best practice, we will create a `JwtGuard`. 

```ts
import { ExecutionContext, Injectable } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { Observable } from 'rxjs';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    return super.canActivate(context);
  }
}
```

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

### Adapter

This is like the interceptor of the gateway and run before the gateway. We might want to handle authentication or get access to the `ConfigService` to access the environment variable
