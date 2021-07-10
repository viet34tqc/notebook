Typescript:
Functions: 
- Declare types for function arguments
	Declare type: using ':', ex: function rect( width: number, height: number )
	A or B: using '|', ex: string|number.
	optional argument: using '?', ex: function rect( width?: number, height: number )
	default value: function rect( width: number = 10, height: number )

- Type of result:
	function square( side: number ) : number

- Function signature
	let greet: ( a: string, b: number ) => void // function signature
	greet = (name: string, age: number ) => console.log(`${name}`)

Explicit Types
Declare the type of variables first:
	let names: string = 'viet'; // String
	let age: number = 3; // Number
	let arr: string[] = ['viet', 'nguyen'] // Array
	let mixed: (string|number|boolean)[] // Union
	let person = {
		name: string,
		age: number,
	} // Object
	let age: any // whatever types

Type alias:
Create a type base on existing types.
	Convention: Start with uppercase
	type StringOrNumber = string | number;
	type Studen = {
		name: string;
		id: StringOrNumber;
	}

	function( id: StringOrNumber, name: string ): void {}

Class: 
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

Interface:
interface Person {
    name: string
    age: number
    speak?: (lang: string) => void
}

const viet: Person = {
    name: 'viet',
    age: 30
}

Tuple:
	Fixed type
	const tuple: [string, number] = ['viet', 30];
	const tupleFunction = (name: string, age: number): [string, number] => [name, age]

Generic:
	Why:
	Let's say you have a function like this:
	```javascript
	const makeArr = (x: number) => [x]
	```
	But what if we didn't know if we wanted to passed in the number here, maybe say we wanted to pass in string or boolean. What will you do?
	At first, you might want to change the type Number to any. However, this is basically going to remove all of your type safety and TS becomes useless.
	This is where Generic comes in. You can use generic like this:
	```javascript
	const makeArr = <T>(x: T) => [x]
	```
	T can be set manually or automatically
	Manually
		makeArr<string>('3')
		makeArr<string>(3) // error
	TS set automatically.
		makeArr(3): T is number
		makeArr('3'): T is string
		makeArr(3);

	Generic extends
		const genericObject = <T extends {firstName: string, lastName: string}>( obj: T) => ({
		    ...obj,
		    fullname: 'abc'
		})
		const args = {firstName: 'viet', lastName: 'nguyen', age: 30} // args must have firstName and lastName
		const n3 = genericObject(args) // Now T has the type of args

	Generic Interface
		interface Resource<T> {
		    uid: number,
		    data: T
		}

		const resourceOne: Resource<string> = { 
		    uid: 1,
		    data: 'viet'
		}

##@types/* packages and \*.d.ts files
Contains type definitions for libraries originally written in vanilla javascript