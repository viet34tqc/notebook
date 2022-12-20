# Typescript

## Installation

npm i -g typescript

## Command

tsc -w: watch mode. Requires `tsconfig.json` file. It can Work without any configuration.
If you don't set the input files in `tsconfig.json`, Typescript will automatically find all the ts file in all folder, then compile `ts` files to `js` files in the same folder.

## Explicit declartion

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

## Implicit declaration

With common primitive types(string, number, boolean), we can simply omit the type annotation
```ts
let name = 'viet'
let age = 3
```

## Read-only type

If you want to make your object and your array be read-only, you add the `as const` after the declaration

```ts
const student = {
    name: 'bob',
    age: 20
} as const
const studentList = ['bob', 'joe', 'doe'] as const
```

## Union type

Create a type based on two or more other type. Use `|`

```ts
function printId(id: number | string) {
  console.log("Your ID is: " + id);
}
```

## Discriminated union type (tagged union type)

It's a union of other types with discriminant properties (tag)

```ts
type LoadingState = {
  state: "loading"; // tag
};
type FailedState = {
  state: "failed"; // tag
  status: number;
};
type SuccessState = {
  state: "success"; // tag
  response: {
    isLoaded: boolean;
  };
};

type State = LoadingState | FailedState | SuccessState;

// The first | is just for readability
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; x: number }
  | { kind: "triangle"; x: number; y: number };
```

## Intersection type

Create a type based on two or more interfaces with `&`

```ts
interface User {
  id: string;
  firstName: string;
  lastName: string;
}

interface Engineer {
  name: string;
  age: number;
}

type Person = User & Engineer

const a: Person = {
  id: 'string',
  firstName: 'string',
  lastName: 'string',
  name: 'stirng',
  age: 23
}
```

## Type alias

Name a type
Name convention: PascalCase
```typescript
type StringOrNumber = string | number; // Union type
type Student = {
	name: string;
	id: StringOrNumber;
}
function( id: StringOrNumber, name: string ): void {}
```


## Interface

Name an object type
Name convention: PascalCase

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

Difference between `interface` and `type`:
`type` cannot be re-opened to add new properties
`interface` is always extendable.

```TS
interface Student {
  name: string
}

interface Student {
  age: number
}

const student: Student = {
    name: 'Bob',
    age: 20
}

type Window = {
  title: string
}

type Window = {
  ts: TypeScriptAPI
}

 // Error: Duplicate identifier 'Window'.
```

## Never

Often used for function that doesn't `return`

## Declare array with Type and Interface

```ts

// Without Generic
interface List {
    [index: number]: Number
}
type List1 = Number[]

// With Generic
interface List<T> {
    [index: number]: T
}

type List1<T> = T[]

const a: List1<number> = [1,2,3]
```

## Functions

- Define type for arguments: using ':'
```typescript
function rect( width: number, height: number )
```

- Define type of return, default is `void`:
```ts
    function square( side: number ) : number
```
- Function signature
```typescript
    let greet: ( a: string, b: number ) => void // function signature
    greet = (name: string, age: number ) => console.log(`${name}`)
```

## Class

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

## Enum

A collection of constants that have the same category
Easier to maintain and get the value
Name convention: PascalCase

2 types: number enum and string enum

### Number enum

```TS
enum Status {
    PENDING,         // 0, default is number 0
    IN_PROGRESS,     // 1
    DONE,            // 2
    CANCELLED,       // 3
}

enum Status {
    PENDING = 2,
    IN_PROGRESS,
    DONE,
    CANCELLED,
}

// Get the value

console.log(Status['PENDING'])
console.log(Status.PENDING)
```

### String enum

```TS
enum Type {
    JSON: 'string',
    HTML: 'html'
}
```

### When to use enum

- Static data, not the data from API response. Use **union type** for data from API response.

## Tuple

Defined fixed types in an array

```ts
const tuple: [string, number] = ['viet', 30];
const tupleFunction = (name: string, age: number): [string, number] => [name, age]
```

## Generic

Why:
Enable types, interface to be used as a parameter => help to reuse the same code for different type of inputs

Let's say you have a function like this:
```ts
const makeArr = (x: number) => [x]
```
But what if we didn't know if we wanted to passed in the number here, maybe say we wanted to pass in string or boolean. What will you do?
At first, you might want to change the type Number to any. However, this is basically going to remove all of your type safety and TS type functionality becomes useless.
This is where Generic comes in. You can use generic like this:
```ts
const makeArr = <T>(x: T) => [x]
```
T is shorthand for 'Types' and can be set manually or automatically
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

### Syntax

With function:
```tsx
const foo = <T, >(x: T) => x;
function foo<T>(x: T): T {
    return x;
}
```

With type and interface:
```tsx  
type foo<T> = {
};
```

With array
```tsx
foo<T>[]
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

## Utility Types

Selectively construct type from other type

- `Partial<Type>`: a type that has all props of `Type` to optional
- `Required<Type>`: a type that has  all props of `Type` to required
- `Pick<Type, Keys>`: a type by picking a set of `Keys` from Types
- `Omit<Type, Keys>`: a type by omitting a set of `Keys` from Types

## `any` and `unknown`

<https://thewebdev.info/2020/08/12/typescript-any-and-unknown-types/>

### `any`

The `any` type variables lets us assign anything to it

```ts
let bar: any;

bar = null;
bar = true;
bar = {};
```
### `unknown`

The `unknown` type is a safer version of `any`
This is because `any` lets us do anything, but `unknown` has more restrictions.
Before we can do anything with `unknown` value, we have to make the type known first by using type assertions, equality, type guards
To add type assertions, we can use the `as` keyword.

```ts
function func(value: unknown) {
  return (value as number).toFixed(2);
}
```

## Overload functions

<https://thewebdev.info/2020/12/19/how-does-typescript-know-which-types-are-compatible-with-each-otherenums-and-more/>
Functions overloads, where there’re multiple signatures for a function with the same name, must have arguments that match at least one of the signatures for TypeScript to accept the function call as valid. For example, if we have the following function overload:

```ts
function fn(a: { b: number, c: string, d: boolean }): number
function fn(a: { b: number, c: string }): number
function fn(a: any): number {
  return a.b;
};
```

Then we have to call the fn function by passing in at a number for the first argument and a string for the second argument like we do below:

```ts
fn({ b: 1, c: 'a' });
```
TypeScript will check against each signature to see if the property structure of the object we pass in as the argument matches any of the overloads. If it doesn’t match like if we have the following:
```ts
fn({ b: 1 });
```

Then we get the following error message:

No overload matches this call.

Overload 1 of 2, '(a: { b: number; c: string; d: boolean; })

## @types/* packages and .d.ts files

Contains type definitions for libraries originally written in vanilla javascript

`.d.ts` file is where we put all the custom type definitions and ambient type declaration 
However, they are not available in other files yet. To make our types available, we also have to export them 

```ts
declare const isDevelopment: boolean // Ambient type
export type StorageItem = {
    weight: number
}
```

## Ignore the error

You can ignore the typescript error by adding the exclamation mark, if you know that it's safe for sure.

`data.value!`

## Tips

### Assigning dynamic keys to an object

```ts
// Method 1
const cache: Record<string, string> = {}

// Method 2
const cache: {[id: string]: string} = {}
```

### Typing async function

```ts
type getUser = (id: string) => Promise<User>
```

### Using Type or Interface

You can use whatever you want. Type is more preferred because we can use it to define union type which is not possible with Interface

### Null and undefined check

Just use `== null` or `!= null` (**not** `===` and `!==`)

```ts
function foo(a?: number | null) {
  if (a == null) return;

  // a is number now.
}
```

### Error check

We can put this in `catch` block
```ts
if (error instanceof Error) {
  error
// ^? const error: Error
}

// Check axios error
if (axios.isAxiosError(error))
```