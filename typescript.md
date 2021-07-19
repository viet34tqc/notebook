# Typescript

## Installation
npm i -g typescript

## Command
tsc -w: watch mode. Requires `tsconfig.json` file. It can Work without any configuration.
If you don't config the input files in `tsconfig.json`, Typescript automatically finds all the ts file in all folder, then compile `ts` files to `js` files in the same folder.

## Functions

- Types for function arguments: using ':'
```typescript
function rect( width: number, height: number )
```
- A or B: using '|', ex: `string|number`
- Optional argument: using '?', ex: function rect( width?: number, height: number )
```typescript
function rect( width: number = 10, height: number )
```
- Type of result:
```javascript
	function square( side: number ) : number
```
- Function signature
```typescript
	let greet: ( a: string, b: number ) => void // function signature
	greet = (name: string, age: number ) => console.log(`${name}`)
```

## Explicit Types
Declare the type of variables first:
```typescript
let names: string = 'viet'; // String
let age: number = 3; // Number
let arr: string[] = ['viet', 'nguyen'] // Array
let mixed: (string|number|boolean)[] // Union
let person = {
	name: string,
	age: number,
} // Object
let age: any // whatever types
```

## Type alias:
Create a type base on existing types.
Convention: Start with uppercase
```typescript
	type StringOrNumber = string | number;
	type Student = {
		name: string;
		id: StringOrNumber;
	}
	function( id: StringOrNumber, name: string ): void {}
```


## Class:
```typescript
class Employee {
    public name: string
    private age: number

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    // You can also write your constructor like this
    constructor(
    	public name: string
    	private age: number
    ) {}


    public get getAge() {
        return this.age
    }

    public set setAge(v : number) {
        this.age = v;
    }

}

const viet = new Employee( 'viet', 30 )
viet.setAge = 32
console.log( viet.getAge );
```

## Interface:
```typescript
interface Person {
    name: string
    age: number
    speak?: (lang: string) => void
}

const viet: Person = {
    name: 'viet',
    age: 30
}
```

## Tuple:

Fixed type
const tuple: [string, number] = ['viet', 30];
const tupleFunction = (name: string, age: number): [string, number] => [name, age]

## Generic:

Let's say you have a function like this:
```javascript
const makeArr = (x: number) => [x]
```
But what if we didn't know if we wanted to passed in the number here, maybe say we wanted to pass in string or boolean. What will you do?
At first, you might want to change the type Number to any. However, this is basically going to remove all of your type safety and TS type functionality becomes useless.
This is where Generic comes in. You can use generic like this:
```javascript
const makeArr = <T>(x: T) => [x]
```
T can be set manually or automatically
- Manually
```typescript
	makeArr<string>('3')
	makeArr<string>(3) // error
```
- Automatically
```typescript
	makeArr(3) // T is number
	makeArr('3') // T is string
```

### Generic extends
The type can be restricted to a certain class family or interface, using the extends keyword:
```typescript
const genericObject = <T extends {firstName: string, lastName: string}>( obj: T) => ({
...obj,
    fullname: 'abc'
})
const args = {firstName: 'viet', lastName: 'nguyen', age: 30} // args must have firstName and lastName
const n3 = genericObject(args) // Now T has the type of args
```

### Generic Interface

```typescript
interface Resource<T> {
    uid: number,
    data: T
}

const resourceOne: Resource<string> = {
    uid: 1,
    data: 'viet'
}
```

## Enums
Enums are one great way to define named constant
```typescript
enum Order {
  First,
  Second,
  Third,
  Fourth
}
```
You can assign values to the constant explicitly:
```typescript
enum Order {
  First = 0,
  Second = 1,
  Third = 2,
  Fourth = 3
}
```
or also use strings:
```typescript
enum Order {
  First = 'FIRST',
  Second = 'SECOND',
  Third = 'THIRD',
  Fourth = 'FOURTH'
}
```
Then you can access the constant like property of Object: `Order.First`, `Order.Second`

##@types/* packages and \*.d.ts files
Contains type definitions for libraries originally written in vanilla javascript