# Dependancy injection

Dependancy injection is a popular pattern that solve the big problem: hardcoded dependancy. So what's hardcoded dependancy?

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