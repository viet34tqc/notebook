# Prototype

<https://prateeksurana.me/blog/how-javascript-classes-work-under-the-hood/>
<https://zellwk.com/blog/javascript-prototype/>

There are two ways to create an object: via function constructor and via `{}`

## Function constructor

- All the function (**not only function constructor**) has a default property called `proptotype`.
- **Arrow function doesn't have** `prototype` property
- Default, f.prototype is an object like this:
- 
```javascript
f.prototype = { constructor: f }
```

- You can define methods and properties on the prototype object:

```javascript
function Dog() {}
Dog.prototype = {
  constructor: Dog,
  legs: 4,
  bark() {}
}
```

- All the instances inherit the property and method from the function constructor's prototype object

```javascript
 function Dog() {}
 const chihuahua = new Dog() // chihuahua will inherits from Dog.prototype
```
Dog --------> Dog.prototype
                  |
                  |
                  |
               chihuahua

```javascript
chihuahua.__proto__ = Dog.prototype
```

`Dog.prototype` inherits from `Object.prototype` (`Dog.prototype.__proto__`) and `Object.prototype` has no prototype (`Object.prototype.__prototype__` is null). **All the objects has prototype except for Object.prototype**

## {}

Actually, when you create an object using `{}`, you are using `Object` function constructor:
```javascript
const dog = {legs: 4}
```
equals to
```javascript
const dog = new Object({leg: 4})
```

## Native prototypes:

Not only object, string, number, array, date can also be defined like this
`new Number()`, `new String()`, `new Array()`, `new Date()`
`Object`, `String`, `Number`, `Array`, `Date` are the built-in function constructor. Their prototypes inherits from `Object.prototype`

## Inheritance:

```javascript
function Animal() { }
function Bird() { }
function Dog() { }

Bird.prototype = Object.create(Animal.prototype);
Dog.prototype = Object.create(Animal.prototype);

Bird.prototype.constructor = Bird;
Dog.prototype.constructor = Dog;
let duck = new Bird();
let beagle = new Dog();
```

- Override method and property

```javascript
function Animal() { }
Animal.prototype.eat = function() {
  return "nom nom nom";
};
function Bird() { }

Bird.prototype = Object.create(Animal.prototype);

Bird.prototype.eat = function() {
  return "peck peck peck";
};
```
