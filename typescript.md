# Typescript

## Installation

`npm i -g typescript`

## Command

tsc -w: watch mode. Requires `tsconfig.json` file. It can Work without any configuration.
If you don't set the input files in `tsconfig.json`, Typescript will automatically find all the ts file in all folder, then compile `ts` files to `js` files in the same folder.

## Type annotation (Explicit declartion)

There are 4 ways of assigning a type to a variable

- colon annotations
- `satisfies`
- `as` annotations
- not annotating and letting TS infer it

```ts
let names: string = 'viet'; // String
let age: number = 3; // Number
```

In fact, most of the time you don't need to type your variables on declartion at all, just let TS infer it
However it's often to use type annotation when you declare an object

```ts
const results = {}
results.abc = 1 // you can't use string to index into result:

// We need to annotate the type of results first
const results: Record<string, unknown> = {}
resuts.abc = 1
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

## Value type

The type is narrowed down to specific value instead of generic type like 'string' or numer

```ts
const newValue = 'A' | 'B' | 'C';
const name = 'viet' // name has 'viet' type
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

`Never` type is a way of saying that this should never happen, this shouldn't be allowed . Often used for function that doesn't `return`

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

Why:

Normally, if we use union type for elements in an array, the element can be one of those type defined in the union

```ts
let arr: Array<string | number>;
arr = ['viet', 1];
arr = [1, 'viet'];
```

There could be problem, when the type of the elements in array are matter, like the first one need to be string, the second one is number. That's why we need tuple.

Tuple defined fixed types in an array

```ts
const tuple: [string, number] = ['viet', 30];
const tupleFunction = (name: string, age: number): [string, number] => [name, age]
```

When to use: when we want to strictly define type for elements in array, like the returned value of React Hook

## Generic

**Must read**: <https://www.totaltypescript.com/mental-model-for-typescript-generics>

Why:

- For types, interface, it turns them into a Javascript-alike function
- For functions, it connects the type of params you pass in and the code in your function body. **If two params has connection to each other, you need to use generics**, e.g one param is object and the other one is its key

When:
When you don't know which type of input you pass into the function

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

With type: When we pass generic to a type, it's like we are tranforming the type to a function and the generic is the param of that type

```ts
type foo<T> = {
};
```

With interface:

```ts
interface Resource<T> {
    uid: number,
    data: T
}

const resourceOne: Resource<string> = {
    uid: 1,
    data: 'viet'
}
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

### Distributive conditional type

```ts
type Fruit = "apple" | "banana" | "orange";

type GetAppleOrBanana<T> = T extends "apple" | "banana" ? T : never;

type AppleOrBanana = GetAppleOrBanana<Fruit> // 'apple' | 'banana'
```

When we pass an union type to a generic, it's like we are iterating it through the type function. We call this as distributive conditional type

```ts
type GetAppleOrBanana =
    | ('apple' extends "apple" | "banana" ? 'apple' : never)
    | ('banana' extends "apple" | "banana" ? 'banana' : never)
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

- `Partial<Type>`: return an object type that has all props of `Type` to optional (Type is object type)
- `Required<Type>`: return an object type that has all props of `Type` to required (Type is object type)
- `Pick<Type, Keys>`: return an object type by picking a set of `Keys` from Types (Type is object type)
- `Omit<Type, Keys>`: return an object type by omitting a set of `Keys` from Types (Type is object type)
- `Extract<Type, Keys>`: return type extracted from `Type`: <https://i.imgur.com/gVCRZQ6.png>, it's kind of like `Pick` but Type in `Pick` is object and `Type` in `Extract` is Union
- `Exclude<Type, Keys>`: return type by excluding from `Type` all union member from `Keys`, it's kind of like `Omit`

## infer

Let's say we have multiple function and we don't know which returned type of each function => we can create a type helper function like this

```ts
type GetReturn<Fun> =
Fun extends (...args: any[]) => infer R ? R : never
```

With infer R we say that no matter the return type of this function, we store it in the type variable R. If we have a valid function, we return R as type

## `null` and `undefined`

Helper function to check `null` and `undefined`

```ts
declare function isAvailable<Obj>(obj: Obj): obj is NonNullable<Obj>

// Implementation
function isAvaialble<Obj>(obj: Obj): obj is NonNullable<Obj> {
    return typeof obj !== 'undefined' && obj !== null
}

// Usage
// orders is Order[] | null
const orders = await fetchOrderList(customer)
if(isAvailable(orders)) {
    //orders is Order[]
    listOrders(orders)
}
```

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

## Working with Array

### Get array value

```ts
const fruits = ["apple", "banana", "orange"] as const;

type AppleOrBanana = typeof fruits[0 | 1];
type Fruit = typeof fruits[number]; // 'apple' | 'banana' | 'orange'
```

### Array to union

```ts
const fruits = ["apple", "banana", "orange"] as const;
type Fruit = typeof fruits[number]; // 'apple' | 'banana' | 'orange'

// So, we can use this technique to handle every elements of array
type Falsy = 0 | '' | false | [] | {[P in any] : never}
type IsTruthy<T extends readonly unknown[]> = T extends Falsy ? false : true
```

### Non empty array

```ts
type NonEmptyArray<T> = [T, ...Array<T>];
type Check = NonEmptyArray<[]> // false
type Check = NonEmptyArray<['a']> // true
```

### Checking empty array

```ts
type Method1<T> = T extends [] ? 'empty' : 'not empty';
type Method2<T extends unknown[]> = T[number] extends never ? 'empty' : 'not empty'
type Method3<T extends unknown[]> = T extends [infer P, ...infer Rest] ? : 'not empty and here you can do something with P and Rest' : 'empty'
```

### Array Destructured

```ts
type T = Array<unknown>
type Check = T extends [infer First, ...infer Rest] ? //...do something : //...do something

// Example
type Includes<T extends readonly any[], U> =
  T extends [infer First, ...infer Rest]
  ? Equal<First, U> extends true
     ? true
     : Includes<Rest, U>
  : false
```

## Working with Object

### Object to Union

The technique below is called mapped type. When you call `K in keyof Values`, it will iterate through the union `keyof Value`. This is similar to distributive condition type.

```ts
interface Values {
  email: string;
  firstName: string;
  lastName: string;
}

type ValuesAsUnionOfTuples = {
  [K in keyof Values]: [K, Values[K]];
}[keyof Values];
```

```ts
// In case you want to extract specific keys from object, just use indexed type
type emailOrFirstName = Values['email' | 'firstName'];
```

### Union type to object

```ts
type Route = "/" | "/about" | "/admin" | "/admin/users";
type RoutesObject = {
    [K in Route]: K
}
```

### Discriminated union to object

```tsx
type Route =
  | {
      route: "/";
      search: {
        page: string;
        perPage: string;
      };
    }
  | { route: "/about"; search: {} }
  | { route: "/admin"; search: {} }
  | { route: "/admin/users"; search: {} };

type RoutesObject = {
  [K in Route as K['route']]: K['search']
};
```

### Combine object

- Using `&`

```ts
type A = { propA: valueA }
type B = { propB: valueB }
type C = A & B // {propA: valueA, propB: valueB}
```

- Convert from Union

```ts
type AppendToObject<T, Prop extends string, Value> = {
  [P in keyof T | Prop]: P extends keyof T ? T[P] : Value;
};
```

## Working with function

### Conditional second parameters

```ts
type Event =
 | {
   type: 'SIGN_IN';
   payload: '123';
   }
 | {
   type: 'SIGN_OUT';
   };

const sendEvent = <Type extends Event['type']>(...args: Extract<Event, {type: Type}> extends {payload: infer Payload} ? [type: Type, payload: Payload] : [type: Type]) => {}
```

### Using generics to declare the relative between params

```ts
const groupBy = <
  TObj extends Record<string, unknown>,
  TKey extends keyof TObj
>(
  arr: TObj[],
  key: TKey
) => {
  const result = {} as Record<
    TObj[TKey] & PropertyKey,
    TObj[]
  >;
  arr.forEach((item) => {
    const resultKey = item[key] as TObj[TKey] &
      PropertyKey;
    if (result[resultKey]) {
      result[resultKey].push(item);
    } else {
      result[resultKey] = [item];
    }
  });
  return result;
};

const result = groupBy(array, "age");

result[20].forEach((item) => {
  // No errors, no validation needed!
  console.log(item.name, item.age);
});

```


## Working with union

Union is iterated by default.

When you put an union into a generics (distributed conditional type) or mapped type of object, it's kind of being iterated through every single condition of that union.

## Tips

<https://github.com/millsp/ts-toolbelt>

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

### Combine `is` and `if` with function to narrow type

```ts

// You could return the boolean type, but it's not good if you use this function with `if` later
// function petIsCat(pet: Pet): boolean {
//   return pet.species === "cat";
// }
function petIsCat(pet: Pet): pet is Cat {
  return pet.species === "cat";
}

//Good
if (petIsCat(p)) {
  p.meow(); // now compiler knows for sure that the variable is of type Cat and it has meow method
}

```

### Type for lookup table object

<https://www.lloydatkinson.net/posts/2022/react-conditional-rendering-with-type-safety-and-exhaustive-checking>

```tsx
import { ReactNode } from 'react';

const Apple = () => <span>🍎🍏</span>;
const Kiwi = () => <span>🥝</span>;
const Cherry = () => <span>🍒</span>;
const Grape = () => <span>🍇</span>;

const icon: Record<Fruit, ReactNode> = {
    apple: <Apple />,
    kiwi: <Kiwi />,
    cherry: <Cherry />,
    grape: <Grape />,
};
```

### Type one or the other

If you have a type with 2 props, both optional, but you want only one of them to exist at the same time. It's like when prop1 is null then prop2 is no longer optional and vice versa.

```ts
type Freecourse = {
    youtube: string;
    price?: never
}

type Paidcourse = {
    youtube?: never;
    price: number
}

type Course = Freecourse | Paidcourse;
```
