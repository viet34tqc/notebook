# Nest Framework

## Reference

- <https://www.freecodecamp.org/news/the-nestjs-handbook-learn-to-use-nest-with-code-examples>

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
`@Injectable()` can also be applied for normal class. By this way, you are telling Nest this is a class that can have dependencies that should be instantiated by Nest and its DI system

In other words, **`@Injectable()` allows class to be injected in other class and also to inject other class into it**

### Services

Normally, for each route, `Controller` will invoke a `Service` method to perform the logic. A `Service` is a `Provider`, which can be injectable. 

In order to inject a service into a controller, we have to: 

- Add that service to the `providers` array of the module. 
- Use the decorator `@Injectable()` at the top of the service. All the injectable class must be added into `providers` array of that module. 

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


### How DI Works in NestJS

When your application boots, Nest builds a module-based IoC container. Each `@Injectable()` provider is registered in with a token (by default, its class). Nest collects metadata from decorators `(@Injectable()`, `@Controller()`) and builds a graph of providers. When you call `NestFactory.create(AppModule)`, it resolves that graph and wires everything together. When a class declares a dependency in its constructor, Nest looks up that token and injects the matching instance.

There are 3 ways to implement DI in NestJS

- using `useClass`.

```ts
// cats.module.ts
@Module({
  controllers: [CatsController],
  providers: [{
    provide: 'CATS_SERVICE',
    useClass: CatsService
  }],
})

// cats.controller.ts
export class CatsController {
  constructor(@Inject('CATS_SERVICE') private readonly catsService: CatsService) {}
  // other code
}
```

- using value provider: The `useValue` syntax is useful for injecting a constant value, putting an external library into the Nest container, or replacing a real implementation with a mock object. 

```ts
const mockSongsService = {
  findAll() {
    return [
      {
        id: 1,
        title: 'Lasting lover',
      },
    ];
  },
};

@Module({
  controllers: [SongsController],
  providers: [
    SongsService,
    {
      provide: SongsService,
      useValue: mockSongsService,
    },
  ],
})
```

- `useFactory`: inject dynamic value

```ts
const devConfig = {port: 3000};
const prodConfig = {port: 3001};

@Module({
  controllers: [CatsController],
  providers: [{
    provide: 'CONFIG',
    useFactory: () => process.env.NODE_ENV === 'development' ? devConfig : prodConfig
  }],
})

export class ConfigController {
  constructor(@Inject('CONFIG') private readonly config: {port: string}) {}
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

When the user send requests to `Controller`, you might want to validate or transform the data before it reaches the controller. This step is called `Pipe`. `Pipe` provides many methods via 2 packages: `class-validator` and `class-transformer`, which you need to install to use pipe in NestJS. 

- Transformation: Convert input data (for example, a string "123") into the desired type (number 123).
- Validation: Check that incoming data meets certain rules and throw an exception (usually a BadRequestException) if it doesn’t.

By default, pipes run after middleware and before guards/interceptors, for each decorated parameter (`@Body()`, `@Param()`, and so on).

<img src="https://i.imgur.com/MTuZ1An.png">

### Validation

There are some ways to apply validation pipe:

Firtst, we apply the validation using `ValidationPipe` class

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

Lastly, we can apply the validation pipe globally in main.ts

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
```

And now, we use `class-validator` to validate the data of the DTO. By doing this, any DTO annotated with validation decorators will be checked before your handler runs:

```ts
import {IsString, IsInt} from 'class-validator';
export class CreateUserDTO {
  @IsString()
  name: string

  @IsInt()
  age: number
}
```

### Transformation

We can also transform the data before sending request. We have two type of transform: `auto transformation` and `explicit transformation`

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

You can even customize the http status code when the type of param is invalid, for example, you pass the id as a string like `abc`

```ts
@Get(':id')
findOne(
  @Param('id', new ParseIntPipe({ errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE })) id: number,
) {
  console.log(typeof id === 'number'); // true
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

It's like `middleware` and sits before route handler (`controller`) to determine if a request can be handled by the route handler or not. The difference between `guard` and `middleware` is `guard` has access to `ExecutionContext`, so they can know which `controller` they are guarding

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

There are 3 scopes where you can apply your guard: route handler, controller and global. 

- Global guard: apply for the whole app. An example of global guard is package @nestjs/throttler, which limit the number of request in a period of time
- Controller guard: apply for the controller class. We often use controller guard for JWT authentication. Only user with JWT token can access to controller's route
- Route guard: apply for a specific route in controller. For example, only the user that create the entry can access to delete entry route.

`Guard` often needs `strategy` to implement

Strategy sits after pipes and before controller.

What `strategy`'s `validation` method returns is passed to the request body. More details in the code below

For example:

Let's say we have a login strategy by JWT

- From `strategy`, we extract the payload from token, then we query the user in db using the data in payload
- `Strategy` should has a name, ex: 'jwt'. We create a `guard` using that strategy name
- In controller, if the protected route use the `guard` created above and the `canActive` in the guard returns true, it will include the data we return from `strategy` in the request body.
- We can also use a custom decorator to extract the data we want to send to the service
- We pass that data to the service and implement our business

## Authentication

In NestJS, we often use `Passport` for authentication. Basically, what it does are:

- Applies the `strategy` to verify the credentials or token by extracting and validating jwt
- Attach user information in the next request (`Guard` sits in front of route handler, then what we return from `validate()` method in `strategy` is passed to the `req` if it passes the guard for further use
- In case there is a custom `handleRequest` in the `guard`, the returned value from `handleRequest` is passed to the Request
  - Default Implementation (no custom `handleRequest`): `validate() → result → req.user.`
  - Custom handleRequest: `validate() → result → handleRequest() → custom result → req.user.`

### Workflow with Passport and JWT

- Login: The user logs in and receives a signed JWT.
- Protected Routes: The client sends the JWT in the Authorization header for protected API endpoints. Before it reaches the controller's handler, it meets `JwtAuthGuard` first
- Guard Invokes Strategy: The `JwtAuthGuard` uses Passport's `authenticate()` to invoke the `JwtStrategy`.
- Token Validation:
  - The `JwtStrategy` extends `PassportStrategy` to check the token (e.g., from the Authorization header), verifies its signature, and decodes its payload.
  - The `validate` method in JwtStrategy is called after successful token verification.
- Request User is Set: The validate method returns the user object (or any data you specify), which is attached to req.user.
- Route Handler Executes:
  - If the guard and strategy succeed, the request proceeds to the route handler.
  - Inside the handler, you can access the authenticated user via req.user.

**So what we need to do is create a `JwtAuthGuard` and `JwtStrategy`**

### Install packages

First, we need to install these packages: `@nestjs/jwt`
If you are using Passport, install these additional packages: `@nestjs/passport`, `passport`, `passport-jwt`

### Create strategy

When using Passport with NestJs, we need to create a strategy. There are lots of strategy: passport-jwt strategy, passport-http-bearer strategy. In this case, we need to use jwt strategy. 

Create `JwtStrategy` and this `JwtStrategy` extends `PassportStrategy`

The purpose of `JwtStrategy`:

- Defines the logic for extracting and validating the JWT.
- Parses the token (e.g., from the Authorization header), verifies its signature, checks for expiration, and decodes the payload.
- The `validate` method is executed after the token is validated, but before access to the protected resource is granted, allowing you to fetch user details or perform additional checks.
- If the token is valid, the returned value of `validate` method is attached to the `Request` Object `req`.

```ts
// jwt.strategy.ts
import { Injectable } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { ExtractJwt, Strategy } from 'passport-jwt';

@Injectable()
// This is equivalent to `export class JwtStrategy extends PassportStrategy(Strategy, 'jwt') {`
// 'jwt' is the default name of `Strategy` from 'passport-jwt'
// We use 'jwt' name in the AuthGuard
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    // Passport automatically verifies the token (e.g., signature and expiration) using the secretOrKey and other configurations provided in the super call of the strategy.
    super({
      // Request need to have a `Bearer` header with JWT value
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: 'abc123',
    });
  }

  // The payload includes jwt payload (such as username, email...) and other jwt information (iat, exp)
  validate(payload: any) {
    // We can simply return the payload
    return payload;

    // Or do some validation before returning
    // Step 1: Perform user validation
    const user = await this.usersService.findOne(payload.email);
    if (!user) {
      throw new UnauthorizedException();
    }

    // Step 2: Attach data to the request
    return {
      user: {
        id: user.id,
        username: user.username,
        email: user.email,
      }
    };
}
```

Add this `JwtStrategy` to `providers` of `auth.module.ts`

### Create JWTAUthGuard

Then we create `JWTAuthGuard` imported from `Passport`.

What `JWTAuthGuard` does: 

- A guard that uses the `JwtStrategy` to protect routes.
- By default, it invokes Passport's `authenticate()` method with the `jwt` strategy under the hood
- Ensures only requests with a valid token (validated by `JwtStrategy`) can access protected endpoints.

```ts
// jwtAuth.guard.ts
import { ExecutionContext, Injectable } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { Observable } from 'rxjs';

@Injectable()
// 'jwt' is the default name of the strategy we defined above, we can change 
export class JwtAuthGuard extends AuthGuard('jwt') {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    return super.canActivate(context);
  }
}
```

When we have the strategy and the guard, we can use the guard to protect the route we want

Example: 

```ts
@UseGuards(JwtAuthGuard)
@Get('protected')
getProtectedResource(@Request() req) {
  return req.user; // User info populated by validation method in JwtStrategy
}
```

In guard, we can define a `handleRequest` method when you need extra checks, error handling, or to modify the returned value from validation. It will override the value from `validate` method from strategy 

```ts
// When you apply the JwtAuthGuard at the controller function it will call the handleRequest function 
handleRequest(err: any, user: any) {
  if (err || !user) {
    throw err || new UnauthorizedException();
  }

  // If user, return user to the controller
  if (user.artistId) {
    return user;
  }
  // Else, throw error, preventing the user from accessing the route this guard protects
  throw err || new UnauthorizedException();
}
```

Best Practice

- Use the default behavior when no additional logic is needed beyond what `validate` provides.
- Override `handleRequest` when you need extra checks, error handling, or to modify the returned user object.

### Import stategy and passport module

We need to tell nestjs about the passport by importing `PassportModule` and then tell it what strategy it uses by adding JwtStrategy to the Provider

```ts
import { Module } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { JwtModule } from '@nestjs/jwt';
import { PassportModule } from '@nestjs/passport';
import { UsersModule } from 'src/users/users.module';
import { AuthController } from './auth.controller';
import { AuthService } from './auth.service';
import { JwtStrategy } from './strategies/jwt.strategy';

@Module({
  providers: [AuthService, JwtStrategy],
  imports: [
    UsersModule,
    PassportModule,
    // To use ConfigService in JwtModule, we need to use registerAsync()
    JwtModule.registerAsync({
      useFactory: (configService: ConfigService) => ({
        secret: configService.get<string>('jwtSecret'),
        signOptions: { expiresIn: '1d' },
      }),
      inject: [ConfigService],
    }),
  ],
  controllers: [AuthController],
})
export class AuthModule {}

```

## Middleware and interceptors

- Interceptors have access to response/request data after or before the route handler is called, accordingly.
- Middleware is called only before the route handler is called.

The execution order is:
Browser -> Middleware -> Interceptors -> Route Handler -> Interceptors -> Exception Filter (if exception is thrown)

## Websockets

First, we define a websocket gateway.

```ts
@WebSocketGateway()
export class EventsGateway {}
```

At this time, the gateway is now listening and clients might want to send messages via an event and server need to listen to that event. In this example, server are listening to an event named 'events'. `body` will be the message body that clients send to server

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

In a NestJS Gateway, an adapter serves as a bridge between the NestJS WebSocket gateway and the underlying WebSocket server implementation (like Socket.IO, WebSocket, etc.). It allows you to customize Websocket server implementation or switch between different WebSocket servers without changing your application logic.

This is like the middleware and run before the gateway. We use adapters to handle authentication or get access to the `ConfigService` to get the environment variables
