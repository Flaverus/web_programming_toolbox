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


