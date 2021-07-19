# OOP

## Function constructor

- All the function constructor has a default property called ```proptotype```
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

- Inheritance:

```javascript
function Animal() { }
function Bird() { }
function Dog() { }

Bird.prototype = Object.create(Animal.prototype);
Dog.prototype = Object.create(Animal.prototype);

// Only change code below this line

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
