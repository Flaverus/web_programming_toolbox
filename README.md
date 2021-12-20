# web_programming_toolbox

In this directory I documente and collect code examples in form of a 'toolbox'

## Variabletypes
```
x       = mutable, global scope   //not realy used
var x   = mutable, hoisted scope  //do not use any more. May lead to problems!
let x   = mutable, local scope 
cont x  = immutable, local scope  //value can not be redefined
```

## Immediately Invoked Function Expression (IIFE)
```
//functions which get immediately executed after declaration 
function foo() {…}; foo()
(function foo() {…}) ()
(function() {…}) ()
```

## Lambda
```
// Modern synthax to write a functiion easily
// from this
const returning = function(x){return x;}
// to this
const returning = x => x
```

## Alpha Translation
```
const id = x => x
const id = y => y
```

## Beta Reduktion
```
( f => x => f (x)) (id) (1)
( x => id (x)) (1)
( id (1))
( x => x) (1)
1
```

## Eta Reduktion
```
x => y => plus (x) (y)
x =>      plus (x)
          plus
```

## Usefull functions
```
const id    = x => x;
const konst = x => y => x; // Kestrel
const snd   = x => y => y; // Kite

const T     = konst;
const F     = snd;
```

## AND
```
const and = p => q => p( q(T)(F)) (q(F)(F)); 
```

## OR
```
const or    = p => q => p(q)(p);
```

## Pair
```
const Pair   = x => y => f => f(x)(y);
const fst    = p => p(T);
const snd    = p => p(F);
const person = 
             firstname =>
             lastname
             age
             pair (pair(firstname)(lastname)) (age);

const firstn = p => fst(fst(p));
const lastn  = p => snd(fst(p));
const age    = p => snd(p);
```

## Either
```
const Left   = x => f => g => f(x);
const Right  = x => f => g => f(x);
const either = e => f => g => e (f) (g);
```

## Exampleusage for either in safe divider
```
const safeDivider = num => divisor =>
      divisor === 0
      ? Left("Do not devide by 0!")
      : Right(num / divisor);
      
either( safeDivider(1)(1) )
      ( x => console.error(x))
      ( x => console.log(x));
```

## Map
```
//Take an array and create new one with content multiplyed by x
const times = a => b => a * b;

[1, 2, 3].map(times(3)); //[2, 4, 6]
```

## Filter
```
//Takes values out of array if condition is met and places into new array
const divides = x => y => x % y === 0;

[1, 2, 3, 4, 5, 6 ,7, 8].filter(divides(4)); //[4, 8]
```

## Reduce
```
//Loops trough array element by element and makes result + current available to work with
const join = joiner => (accu, cur) => accu + joiner + cur;

[1, 2, 3, 4].reduce(join('/')) //'1/2/3/4'
```

## Objects
```
//Not safe but dynamic. To share structure
const person = {
      firstname : "Florian",
      lastname  : "Schnidrig",
      getName   : function() {
	        return this.firstname + " " + this.lastname
      }
};

//safe and to share structure
function Person(first, last) {
         let firstname = first;
         let lastname  = last;
         return {
	   getName : function() {
	       return firstname + " " + lastname }
	   }
         }
}

//dynamic and instances can be changed via the prototype
const Person = ( () => { // lexical scope
      function Person(first, last) { // ctor, binding
	     this.firstname  = first;
	     this.lastname   = last;
      }
      Person.prototype.getName = function() {
	   return this.firstname + " " + this.lastname;
      };
      return Person;
}) ();
//new Person("Florian", "Schnidrig") instanceof Person
```

## Class

```
class Person {
      constructor(first, last) {
          this.firstname = first;
          this.lastname  = last
      }
      getName() {
          return this.firstname + " " + this.lastname
      }
}
// new Person("Florian", "Schnidrig") instanceof Person
```

```
class Student extends Person {
      constructor (first, last, grade) {
          super(first, last); // never forget
          this.grade = grade;
      }
}
const s = new Student("Florian","Schnidrig", 5.5);
```

```
//Prototype chain
const s = new Student()
// s.__proto__ === Student.prototype; //DO NOT EVEN THINK ABOUT IT!!!
// Object.getPrototypeOf(s) === Student.prototype;
// => s instanceof Student
```

## Observable
```
const Observable = value => {
	const listeners = []; // many
	return {
		onChange: callback => listeners.push(callback),
		getValue: () => value,
		setValue: val => {
			if (value === val) return; // protection
			// ordering
            value = val;
			listeners.forEach(notify => notify(val));
		}
	}
};
```

## Callback, Events
```
function start() {
	//...
	window.onkeydown = evt => {
		// doSomething();
	};
	setInterval(() => {
		// doSomething();
	}, 1000 / 5);
}
```

## Promise
```
fetch ('http://fhnw.ch/json/students/list')
.then(response => response.json())
.then(students => console.log(students.length))
.catch (err => console.log(err)
Success / Failure callbacks:

// definition
const processEven = i => new Promise( (resolve, reject) => {
	if (i % 2 === 0) {
		resolve(i);
	} else {
		reject(i);
	}
});

// use
processEven(4)
.then ( it => {console.log(it); return it} ) // auto promotion
.then ( it => processEven(it+1))
.catch( err => console.log( "Error: " + err))
Async / Await
const foo = async i => {
	const x = await processEven(i).catch( err => err);
	console.log("foo: " + x);
};
foo(4);
```

## Modules

Modules help to organize the Code, make clear dependencies and avoid errors. (Globals, Scoping, Namespace)

The following example of addinf js files to your page might nt work depending on the dependencies between each other:
```
<script src="fileA.js">
<script src="fileB.js">
<script src="fileC.js">
// Won't work if fileA.js has a reference on fileC.js.
```

Makes it clear on how you want to edit the code and how you want the code to be delivered

Do not confuse Modules with the following:

Packages (they have versions)
Dependencies, Libraries, Releases
Units of publication
Objects

Modules are async
```
// Use URI format as follows: "./myFile.js"
<script src="./myFile.js" type="module"> // type implies "defer"
import ("./myFile.js").then( modules => ... )
```
### Import Variants
(Always explicit!)

```
// most used
import "module-name";
import { export1, export2 } from "module-name";

// other variants
import defaultExport from "module-name";
import * as name from "module-name";
import { export } from "module-name";
import { export as alias } from "module-name";
var promise = import("module-name");
```

### Export Variants
Always explicit!

```
// most used
export { name1, name2, ... , nameN };

// other variants
export function FunctionName() { .. }
export const name1, name2, ... , nameN; // or let
export class ClassName { .. }
export default expression;
export { name1 as default, .. };
export * from .. ;
export { name1, name2, ... , nameN } from .. ;
```

### Impacts
- implicit "use-strict" exports are read-only.
- no Global object, no Global "this", no Global hoisting
- implicit "defer" mode => document.writeln is no longer useful
- Modules are Namespaces
- Modules are Singletons

### SOP - Same Origin Policy
Modules are subject to the SOP
Problem you might face at construction time: the File System is a null origin!

Useful Tools to prevent this problem:
- Developer Mode (suppress SOP) -> don't forget to set back!
- Local Webserver
- Disable cache!
- Bundler (Rollup, Parcel, Webpack, ...)
- Start Browser in Debug mode
