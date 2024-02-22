# Nest Framework

## Common commands

- `nest new <PROJECT_NAME>`: create new Nest project. You will need to install `@nest/cli` first
- `npx nest generate resource <YOUR_SERVICE>`: create resource. This command will create the boilerplate code for your service.

## Concepts

NestJS runs in `Module` structure. You will have many modules in the app, ex: user module, authentication module...

Each module has a seperate `Controller` to handle routing. For each route, the Controller will invoke a `Service` method to perform the logic. Before invoking a method from `Service` class, we have to add the service to the `providers` array of that module. This is called Dependancy Injection. Without DI, in the Controller class, we have to intiate the Service class first => code is cumbersome

```ts
// cats.module.ts
@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
```

When the user send requests to `Controller`, you might want to validate or transform the data before it reach the controller. This steps is called `Pipe`. `Pipe` provides many methods via 2 packages: `class-validator` and `class-transformer`, which you need to install to use pipe in NestJS

