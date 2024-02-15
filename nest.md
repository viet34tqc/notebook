# Nest Framework

## Common commands

- `nest new <PROJECT_NAME>`: create new Nest project. You will need to install `@nest/cli` first
- `npx nest generate resource <YOUR_SERVICE>`: create resource. This command will create the boilerplate code for your service.

## Concepts

NestJS runs in `Module` structure. You will have many modules in the app, ex: user module, authentication module...
Each module has a seperate `Controller` to handle routing. For each route, the Controller will invoke a `Service` method to perform the logic

When the user send requests to `Controller`, you might want to validate or transform the data before it reach the controller. This steps is called `Pipe`