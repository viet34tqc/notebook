# Dependancy injection

<https://codedrivendevelopment.com/posts/dependency-injection-in-react>

## Dependency Inversion

Dependency Inversion Principle (DIP) says that high-level modules should not import anything from low-level modules, both should depend on abstractions. What this means, is that any high level module, which naturally could be dependent on implementation details of modules it uses, shouldn't have that dependency.

The high and low-level modules should be used without knowing any details about the other module's internal implementation. Each module should be replaceable with an alternative implementation but the interface they use stays the same.

## Inversion of Control

Inversion of Control (IoC) is a principle used to address the dependency inversion problem. It states that dependencies of a module should be provided by an external entity or framework. 

Why: the module itself only has to import and use the dependency, it never has to create the dependency from scratch or manage it in any way.

## Dependency injection

<https://blog.codeminer42.com/di-with-some-context/>

Dependancy injection is a common way to implement IoC. 

- Dependencies (variables, objects, or services) are injected (provided) into your code (classes) from the outside. 
- Dependencies are not hard-coded or instantiated within your code (classes) directly

So what's hardcoded dependancy? Hardcode dependancy means class A is tightly coupled to class B or method B, class B/method B is initiated and called inside a method of class A. There are some issue with this:

- It's harder for testing: Let's say we want to test class A. And because class A is using class B, it's really hard to mock class B
- It violate the Open-Closed principle: when we don't want to use class B, we need to change the code in class A. And this adjustment could be complicated if the class B implementation in class A is complicated as well. 

Here is an example:

1. Define an abstraction (interface):

```ts
// ILogger.ts
export interface ILogger {
  log(message: string): void;
}
```

2. Define implementations of the interface:

```ts
// ConsoleLogger.ts
import { ILogger } from './ILogger';

export class ConsoleLogger implements ILogger {
  log(message: string): void {
    console.log(message);
  }
}

// FileLogger.ts 
import { ILogger } from './ILogger';

export class FileLogger implements ILogger {
  log(message: string): void {
    console.log(`(Pretending to log to a file): ${message}`);
  }
}
```

3. Use Dependency Injection to supply the dependency:

```ts
// UserService.ts
import { ILogger } from './ILogger';

export class UserService {
  // Dependency is injected via the constructor.
  constructor(private logger: ILogger) {}

  createUser(userName: string): void {
    // Some logic to create a user...
    this.logger.log(`User ${userName} created.`);
  }
}
```

4. Implementation

```ts
import { UserService } from './UserService';
import { ConsoleLogger } from './ConsoleLogger';
// Or you could use FileLogger:
// import { FileLogger } from './FileLogger';

// Create the dependency
const logger = new ConsoleLogger();

// Inject the dependency into the UserService
const userService = new UserService(logger);

// Use the service
userService.createUser('Alice');
```

- Dependency Inversion:
The UserService depends on the abstraction ILogger instead of a concrete logger. This means you can switch out ConsoleLogger for FileLogger (or any other implementation) without modifying UserService.
- Dependency Injection:
The UserService receives its dependency (an ILogger instance) via its constructor. This is a form of dependency injection (specifically, constructor injection). The responsibility of creating the concrete logger instance is moved outside of UserService, making it easier to test and maintain.

## What are the benefits of using dependency injection?

- Easier to maintain code and reusable code
- Making testing easier: you can use tools such as Storybook to preview your individual components and easily inject in mock services/objects (potentially without doing real API calls for example).
- Clear of separtion of concern

## Difficulties with dependency injection

- Complexity: If you use data fetching libraries (such as tanstack query, RTK Query etc) then injecting in your provided objects/services can be quite complex.

## Dependancy injection in React

In a React app, your components should generally just deal with the 'view' part - how it looks and setting up event handlers (onClick etc).

Things with side effects (e.g. making API calls, or interacting with browser web APIs such as localStorage) or business logic generally shouldn't be part of your React components and should be abstracted away.

Remember that the aim of using DI is to avoid tightly coupled between components and easier testing. So, what code smell when we want to use DI in our component:

- There are hardcoded values, like an API call => using Props as dependancies to inject into the component or use `React.context`
- Import a lot of child components => using composition component with `children` prop 
- `window.localStorage`, `window.sessionStorage` 
- configuration or environment variables. You can use DI to inject in variables - not just services/functions
- logging - useful to have a injected in logger which could send the data to your logging service

**Examples**:

- Your component has multiple child components (for example, card header, card footer...). You can
  - Using composition
  - Pass child components via props of parent component
- <https://dev.to/itshugo/applying-design-patterns-in-react-strategy-pattern-enn>
- axios: we can create an axiosInstance, then use it everywhere in the project like: `axiosInstance.get()` and `axiosInstance.post()`. That should be fine if we use axios for this project. But in the future, we might want to replace axios with other library or using fetch, then we will need to remove all `axiosInstance` code. For best practice, we will use a `httpClient` class to handle APIs request. If we use other library just change the code in `httpClient` class