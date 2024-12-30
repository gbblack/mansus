---
tags:
  - type/chapter
  - status/day
created: 2024-12-11
---
[[The TypeScript Workshop]]
# **TypeScript Fundamentals**

> [!abstract] Summary
> Introduction to TypeScript, its compiler and the the basic programming constructs as they apply to TypeScript.
## **Note**
---
### **Introduction**
- TypeScript is a superset of JavaScript. It adds many features that make our code better to read, write and debug.
##### **Evolution of TypeScript**
- Designed by Microsoft.
- JavaScript has outgrown its initial design function and needed major upgrades to raise it to "professional dev" standard.
- Previously [[transpilers]] were used to turn software code in other languages into JS. This wasn't great because the transpiled JS was stored in huge libraries in the browser and not human readable.
##### **Design Goals of TypeScript**
- The solution to this was TypeScript, you couldn't take JS out of the browser so instead of translating another language into JS we just improve it.
- The major goals of TS was to make JS: strongly typed, better supported with tooling, and to make it a language capable of handling applications.
### **Getting Started with TypeScript**
##### **The TypeScript Compiler**
- Files use the `.ts` extension.
- These files still need to be compiled into matching JS, which can be done with the [[TypeScript compiler]] `tsc`.
- This compiler take TS code and transpiles it into JS.
- Important flags for `tsc`:
	- `-outFile`: name of the output file that is generated.
	- `-outDir`: location of the output files.
	- `-types`: specify additional types allowed in source code.
	- `-lib`: which library files need to be loaded.
	- `-target`: which version of JS we're targeting.
##### **Setting Up A TypeScript Project**
- To save having to input all our compile options each time we run `tsc` we can save all these options inside a `tsconfig.json` file.
- This is best generated with this command: `tsc --init`.
- the config file should stay in the root directory.
##### **Types And Their Uses**
- TypeScript is a strongly typed language.
- To bind a variable to a type we can annotate it: `let variable: number`.
- A value can just be assigned to a variable and the compiler will use that initial value as the "type" of the variable, this is *type inference*.
### **TypeScript and Functions**
- TS has automatic function invocation checking, this means that even without being explicit the compiler still know certain information and can check against it, i.e. the parameters
- This way a function cannot be invoked with the incorrect number or type of parameters.
- The return type may also be annotated.
### **TypeScript and Objects**
- In both JS and TS objects are treated as literals, we can create and use them like any other value.
- Object fields can be made optional using a `?`, `age?: number`.
- Interfaces may be used as type annotations.
- Interfaces in TS are structural, which means that so long as an object matches the "rules" of the interface it is considered to be of the interface.
### **Basic Types**
- To get the type of a variable we use `typeof`, `typeof variable`.
- TypeScript has many primitive types which include: `number`, `boolean`, `string`, `enum`, `undefined`, … .
- It also has two structural types: objects and functions.
##### **Strings**
- All words or text fall into the `string` type category.
- In JS, and thus TS, strings are a [[first-order citizen]], which mean it is supported by all operations available to other entities.
- This differs from most languages which treat strings as an array of characters.
- A `string` can be declared with `'` or `"`.
- There is also a special string called a [[template string]] which is delimited by backticks: `` ` ``.
- These template strings support newlines and embedded expressions.
- This allows us to generate better HTML as it avoids the mess of string concatenation.
##### **Numbers**
- In TS all numbers fall into the `number` primitive, which is treated as a 64 bit floating-point number.
- So `4` and `4.44` are both stored as the same type `number`.
- This can result in some weirdness, in JS dividing by 0 does not cause an error. Instead it will return `NaN` or `Infinity`.
##### **Booleans**
- The boolean data type can only have one of two values: `true` or `false`.
##### **Arrays**
- In JS arrays are written as: `const numbers = [1, 2, 3, 4, 5];`.
- Arrays can be accessed using their index: `numbers[0] // is 1`.
- We can crawl over an array using a `for` loop, either:
  `for (let index=0; index<condition; index+=1) {}` or
  `for (const element of numbers) {}`.
- In JS the values of an array can be of any type.
- In TS we can restrict the type of the array: `let number: number[];`.
- Or with the [[generic type]] syntax: `let numbers: Array<number>;`.
##### **Tuples**
- In TS when we know the size of the array it is known as a tuple.
- Written as: `const person: [string, string, number]`.
##### **Enums**
- Enum is a set of predetermined value where only one can be valid at a time, think the 4 directions or the 4 suits in cards.
- Written as: `enum Suit { Hearts, Diamonds, Clubs, Spades }`
- We can access the values using dot notation: `let suit = Suit.Hearts;`.
- Any attempt to assign a value that isn't part of the enum will cause an error, e.g. `suit = Suit.Coins;`.
- Unique to TypeScript.
- We can assign numbers (`Hearts=10`) or strings (`Hearts: "hearts"`) to the enum values as well.
##### **Any and Unknown**
- To keep JS "anything goes" behaviour we can assign the `any` type to a variable.
- This reverts the variable to the default behaviour where all calls on it won't be checked by the compiler.
- This type is considered *highly contagious* as results from any call to it will become type `any`.
- Best used as infrequently as possible.
- The `unknown` type exists to help counteract this contagious effect. It works on the opposite principles of the `any` type in that if we don't know that something is a `string` we assume it isn't rather than it is.
##### **Null and Undefined**
- There are two JS values to express no value: `null` and `undefined`.
- `null` must be set explicitly.
- `undefined` is usually if the value was never set.
- If a property in an object is optional its default value is `undefined`.
##### **Never**
- There is a special "not a value" type unique to TS: `never`. It expresses a value that *never* occurs.
- A use case for this is if there is a logical condition that cannot be true, such as checking a condition that is impossible to occur, e.g. `x = true; if (!x) <- not possible`. Here the compiler will infer the `never` type.
##### **Function Types**
- As a first-order object in TS functions can be types as well.
- To define a function type we need all parameters, their types and the return values and types.
- Written as: `(x: number, y: number) => number`.
### **Making Your Own Types**
- We can also define our own types. We can do this using in several ways: `class`, `interface` or `type`.
- `class` is a specification from JS. We can declare properties and methods within a class, create objects of that class and then access the defined areas.
> [!example]- Class
> ```ts
> class Person {
> 	constructor (
> 	public firstName: string,
> 	public lastName: string,
> 	public age?: number
> 	) {}
> 	getFullName() {
> 		return `$(this.firstName) $(this.lastName)'
> 	}
> }
> 
> const person = new Person("Ada", "Lovelace");
> console.log(person.getFullName());
> ```
- `interface` is a TS only construct, they differ from classes in that they are statically checked at compile time but NOT included in the compiled code.
- Interfaces are also not constructed the way classes are.
> [!example]- Interface
> ```ts
> interface Person {
> 	firstName: string;
> 	lastName: string;
> 	age?: string;
> }
> ```
- `type` keyword is for [[type aliasing]], which can be applied to anything. e.g. we want to give a name to a tuple: `type Person = [string, string, number?];`.
- Objects and functions can also be used by the type alias.
> [!example]- Type
> ```ts
> type Person {
> 	firstName: string;
> 	lastName: string;
> 	age?: number;
> }
> type FilterFunction = (person: Person) => boolean;
> ```
##### **Citation**
---
```
B. Grynhaus, J. Hudgens, R. Hunte, M. Morgan and W. Stefanovski, "TypeScript Fundamentals" in *The TypeScript Workshop*, 1st. M. Dhyani, R. Rodrigues, S. Shinde, S. Zagade, Eds. Birmingham, UK: Packt, 2021, pp. 1-61.
```