---
tags:
  - type/chapter
created: 2024-12-11
---
[[The TypeScript Workshop]]
# **TypeScript Fundamentals**

> [!abstract] Summary
### **Note**
---
##### **Introduction**
- TypeScript is a superset of JavaScript. It adds many features that make our code better to read, write and debug.
**Evolution of TypeScript**
- Designed by Microsoft.
- JavaScript has outgrown its initial design function and needed major upgrades to raise it to "professional dev" standard.
- Previously transpilers were used to turn software code in other languages into JS. This wasn't great because the transpiled JS was stored in huge libraries in the browser and not human readable.
**Design Goals of TypeScript**
- The solution to this was TypeScript, you couldn't take JS out of the browser so instead of translating another language into JS we just improve it.
- The major goals of TS was to make JS: strongly typed, better supported with tooling, and to make it a language capable of handling applications.
##### **Getting Started with TypeScript**
**The TypeScript Compiler**
- Files use the `.ts` extension.
- These files still need to be compiled into matching JS, which can be done with the [[TypeScript compiler]] `tsc`.
- This compiler take TS code and transpiles it into JS.
- Important flags for `tsc`:
	- `-outFile`: name of the output file that is generated.
	- `-outDir`: location of the output files.
	- `-types`: specify additional types allowed in source code.
	- `-lib`: which library files need to be loaded.
	- `-target`: which version of JS we're targeting.
**Setting Up A TypeScript Project**
- To save having to input all our compile options each time we run `tsc` we can save all these options inside a `tsconfig.json` file.
- This is best generated with this command: `tsc --init`.
- the config file should stay in the root directory.
**Types And Their Uses**
- TypeScript is a strongly typed language.
- To bind a variable to a type we can annotate it: `let variable: number`.
- A value can just be assigned to a variable and the compiler will use that initial value as the "type" of the variable, this is *type inference*.
##### **TypeScript and Functions**
- TS has automatic function invocation checking, this means that even without being explicit the compiler still know certain information and can check against it, i.e. the parameters
- This way a function cannot be invoked with the incorrect number or type of parameters.
- The return type may also be annotated.
##### **TypeScript and Objects**
- In both JS and TS objects are treated as literals, we can create and use them like any other value.
- Object fields can be made optional using a `?`, `age?: number`.
- Interfaces may be used as type annotations.
- Interfaces in TS are structural, which means that so long as an object matches the "rules" of the interface it is considered to be of the interface.
##### **Basic Types**
- To get the type of a variable we use `typeof`, `typeof variable`.
- TypeScript has many primitive types which include: `number`, `boolean`, `string`, `enum`, `undefined`, … .
- It also has two structural types: objects and functions.

**Strings**
- All words or text fall into the `string` type category.
- In JS, and thus TS, strings are a [[first-order citizen]], which mean it is supported by all operations available to other entities.
- This differs from most languages which treat strings as an array of characters.
- A `string` can be declared with `'` or `"`.
- There is also a special string called a [[template string]] which is delimited by backticks: `` ` ``.
- These template strings support newlines and embedded expressions.
- This allows us to generate better HTML as it avoids the mess of string concatenation.

**Numbers**
- In TS all numbers fall into the `number` primitive, which is treated as a 64 bit floating-point number.
- So `4` and `4.44` are both stored as the same type `number`.
- This can result in some weirdness, in JS dividing by 0 does not cause an error. Instead it will return `NaN` or `Infinity`.

**Booleans**
- The boolean data type can only have one of two values: `true` or `false`.

**Arrays**
- In JS arrays are written as: `const numbers = [1, 2, 3, 4, 5];`.
- Arrays can be accessed using their index: `numbers[0] // is 1`.
- We can crawl over an array using a `for` loop, either:
  `for (let index=0; index<condition; index+=1) {}` or
  `for (const element of numbers) {}`.
- In JS the values of an array can be of any type.
- In TS we can restrict the type of the array: `let number: number[];`.
- Or with the [[generic type]] syntax: `let numbers: Array<number>;`.

**Tuples**
- In TS when we know the size of the array it is known as a tuple.
- Written as: `const person: [string, string, number]`.

**Enums**
- 
##### **Making Your Own Types**

##### **Citation**
---
```
B. Grynhaus, J. Hudgens, R. Hunte, M. Morgan and W. Stefanovski, "TypeScript Fundamentals" in *The TypeScript Workshop*, 1st. M. Dhyani, R. Rodrigues, S. Shinde, S. Zagade, Eds. Birmingham, UK: Packt, 2021, pp. 1-61.
```
---
##### Completion Checklist

###### II. From Dark to Dawn
- [x] Wait at least 5mins before beginning this section.
- [x] In the `Note` section breakdown the chapter into its subheading: all the sections in bold.
- [ ] In bullet points, under each section and sub section heading summarise the major points of that section. In these bullet point make links to anything that could be referenced in a permanent note. This will take awhile.
- [ ] While summating the sections copy paste interesting sections into the note under `Highlights`. Include the page number right above the block. These highlights should only be the author's own reflections that you think are interesting, nothing definitive. There may be nothing.
- [ ] Update status tag to `status/dawn`.
###### III. From Dawn to Day
- [ ] Write the chapter `Summary`.
- [ ] Remove or complete the `Nota Bene` as necessary.
- [ ] Fill in the context tags for the metadata.
- [ ] Update status tag to `status/day`.
- [ ] Remove this checklist.