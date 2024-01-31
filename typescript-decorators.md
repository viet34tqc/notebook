# Typescript Decorators

- <https://www.youtube.com/watch?v=O6A-u_FoEX8>
- <https://www.youtube.com/watch?v=gn3060fnn3k>

It's a function that lets you hook into the source code and to extend the functionality, ex: modify an array before returning it, map the data to the DTO before returning...

## Function descriptor

Descriptor for function must accepts 3 arguments: the class instance, the method name, and the `propertyDescriptor`. `propertyDescriptor` is a record that returns the descriptions of a property/method. it's like: 

```js
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
```

Here is a example of function decorator

```ts
function logMethod(
  obj: any,
  propertyName: string,
  descriptor: PropertyDescriptor
) {
  console.log('descriptor', descriptor.value); // The descriptor value returns the method `testMethod` itself
  // Now, you are overriding the original method
  descriptor.value = () => {}

  // If you want to utilize the orignal method
  const originalMethod = descriptor.value;
  descriptor.value = (...args: any[]) => {
  	return originalMethod(args)
  }
}

// The above method doesn't allow you to pass a param, so you will need a decorator wrapper
function decoratorWrapper(x) {
	return function(
	  obj: any,
	  propertyName: string,
	  descriptor: PropertyDescriptor
	) {
		return x * descriptor.value()
	}
}

class Test {
	@logMethod
	testMethod() {

	}
}
```

## Class decorator

Class Decorator only accepts one parameters, which is the class itself
