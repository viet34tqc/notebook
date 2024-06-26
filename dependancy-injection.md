# Dependancy injection

## Dependency Inversion

Dependency Inversion Principle (DIP) says that high-level modules should not import anything from low-level modules, both should depend on abstractions. What this means, is that any high level module, which naturally could be dependent on implementation details of modules it uses, shouldn't have that dependency.

The high and low-level modules, should be written in a way so they both can be used without knowing any details about the other module's internal implementation. Each module should be replaceable with an alternative implementation of it as long as the interface to it stays the same.

## Inversion of Control

Inversion of Control (IoC) is a principle used to address the dependency inversion problem. It states that dependencies of a module should be provided by an external entity or framework. That way, the module itself only has to use the dependency, it never has to create the dependency or manage it in any way.

## Dependancy injection

<https://blog.codeminer42.com/di-with-some-context/>

Dependancy injection is a common way to implement IoC. It remove hardcoded dependancy between module in an application. The aim of this technique is to make our code more flexible and scalable

So what's hardcoded dependancy?
Hardcode dependancy means class A is tightly coupled to class B, class B is called inside class A. There are some issue with this:

- It's harder for testing: Let's say we want to test class A. And because class A is using class B, it's really hard to mock class B
- It violate the Open-Closed principle: when we don't want to use class B, we need to change the code in class A. And this adjustment could be complicated if the class B implementation in class A is complicated as well. 

Here are some examples of not using dependancy injection that make it harder to change the business in the future

- <https://dev.to/itshugo/applying-design-patterns-in-react-strategy-pattern-enn>
- axios: we can create an axiosInstance, then use it everywhere in the project like: `axiosInstance.get()` and `axiosInstance.post()`. That should be fine if we use axios for this project. But in the future, we might want to replace axios with other library or using fetch, then we will need to remove all `axiosInstance` code. For best practice, we will use a `httpClient` class to handle APIs request. If we use other library just change the code in `httpClient` class

## Dependancy injection in React

Remember that the aim of using DI is to avoid tightly coupled between components and easier testing. So, what code smell when we want to use DI in our component:

- There are hardcoded values, like an API call => using Props as dependancies to inject into the component. 
- Import a lot of child components => using composition component with `children` prop 