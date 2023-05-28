# Prototype

<https://prateeksurana.me/blog/how-javascript-classes-work-under-the-hood/>
<https://zellwk.com/blog/javascript-prototype/>
<https://javascript.info/native-prototypes>

There are two ways to create an object: via function constructor and via `{}`

## Function constructor

- All the functions (**not only function constructor**) have two default properties called `proptotype` and `__proto__`
- An instance of a function constructor access `F.prototype` via `__proto__`
- **Arrow function doesn't have** `prototype` property
- Default, f.prototype is an **object** like this:

```javascript
f.prototype = { constructor: f } // It has a property that points back to the function itself
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

- All the instances inherit the property and method from the function constructor's prototype object. Function constructor's prototype is an object, so it inherits from `Object.prototype`

```javascript
function Dog() {}
const chihuahua = new Dog() // chihuahua will inherits from Dog.prototype
```

    .prototype             __proto__
Dog --------> Dog.prototype ---------> Object.prototype
 |                |                          |
 | __proto__      | __proto__                |
 |                |                          |
Function      chihuahua                      |
.prototype------------------------------------
                      __proto__ 

```javascript
chihuahua.__proto__ === Dog.prototype // true
Dog.__proto__ === Function.prototype // true
```

`Dog.prototype` inherits from `Object.prototype` (`Dog.prototype.__proto__`) and `Object.prototype` has no prototype (`Object.prototype.__proto__` is null). **All the objects has prototype except for Object.prototype**

## `{}`

<https://javascript.info/native-prototypes>

Actually, when you create an object using `{}`, you are using `Object` function constructor:

```javascript
const dog = {legs: 4}
```

equals to

```javascript
const dog = new Object({leg: 4})
```

Besides object, function, string, number, array, date can also be defined with `new Number()`, `new String()`, `new Array()`, `new Date()`, `new Function()`, respectively. Only instance of `new Function()` has `prototype` property, other kinds of instance only have `__proto__` property
`Object`, `Function`, `String`, `Number`, `Array`, `Date` are the built-in function constructors. Their prototypes inherits from `Object.prototype`

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
