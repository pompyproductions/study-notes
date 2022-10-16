# JS Notes

## General

You can add to html via the two following: 
```html
<script>console.log("kek")</script>
<script src="kek.js"></script> 
```

either put the script in the body, or put it in the head and defer

myNum.toFixed(x) will round to X decimal places
1.943132.toFixed(2) = 1.94
1.949314.toFixed(2) = 1.95 (round, not floor)


---

## Data types

8 types: number, bigint, bool, string, object, null, undefined, special.

typeof() gives type, === compares value AND type

### NUMBER & BIGINT

JS does NOT define different types of numbers, it's one type (Number) and is 64-bit floating point.

There's also BigInt (came in handy for Project Euler exercises).

!! Number(250) is not the same as new Number(250)
Number() tries to convert into number (like toString)

Division by zero returns Infinity, string as operand returns NaN. Both are of type number.

UNARY PLUS: converts other type into num
+true === 1, +false === 0

### STRING

Different from java: not an object, and char type doesn't exist

### UNDEFINED and NULL

Undefined is when a var is initialized, whereas null is for "empty" values.

Do NOT use `myVar = undefined`.

null == undefined (true), null !== undefined (true)

### BOOLEAN

`Boolean(myVal)` to check if value is truthy or not.

All strings except "" (empty) are truthy ("false" etc as well).  
All numbers except 0 are truthy (-1 too).  
Empty arrays are still truthy.


---
## String Manipulation

\ is your escape character as always

template literals: ``

slice(from, to), includes from but not to
    "pompy".slice(0, 4) will evaluate to "pomp"
substr() and substring() are different
    substring(from, to) is like slice but taking negative values as 0
    substr(start, length)
    substr(start) will take all the rest of the string

text.replace() does not modify the variable, but returns a new one
    it only replaces the first occurrence, so use regex instead
    case-sensitive too...
regex is as such: "hello anam, hello babam".replace(/hello/ig, "selam")
    i and g are flags (insensitive and global)

toUpperCase(), toLowerCase(), trim()
"hello".padStart(3, "_") == "__hello" (one less!)

charAt(), charCodeAt(), property access [] (try to avoid)
    prop access [] can't be used to replace char at position
    charAt doesn't accept negative values, so use substr(-1) instead
    substr(-2).charAt(0)

split(char), splits on every occurrence of char

txt1 = txt1.concat(" ", txt2); returns new string


---
Array Manipulation

.includes(): has, contains element

.splice(): for removing with index

.map(): returns a new array with same length, but "extracting" whatever data you need

.filter(): returns a new array with different length, filtering out the ones that don't
match the criteria

.reduce(): returns a new array while boiling down information, good for iterating

.every()/.some(): returns true if every/any element in the array meets the criteria

.forEach(item => {}): for iteration

.push()/.pop(): adds new element and returns new length & removes and returns last element

---
Functions

functions are VALUES
they store a "function value" which is a code block to evaluate

myFunc() executes the function, whereas myFunc references it
if you say:
    let myCopy = myFunc;
    myCopy(); // this works

they can be passed into other functions as arguments
-> callback functions

    function ask(question, yes, no) {
    if (confirm(question)) yes()
    else no();
    }

    ask(
    "Do you agree?",
    function() { alert("You agreed."); },
    function() { alert("You canceled the execution."); }
    );

function DECLARATIONS and EXPRESSIONS:
    function sum(a,b){return a+b} // declaration
    let sum = function(a,b) {return a+b} // expression

declarations are hoisted, expressions are created like "let" or "const"
both have block scope, but you can create functions in local scope
    and pass it on to a variable (let smth, go to local scope, smth = function(){};)

use the ARGUMENTS OBJECT for overloading etc:
    it is an "array-like" that only has the arguments.length method
    and you can access arguments[i]
    
    but if it's just for default values, this will already do the trick:
    function myFunc(myVar="default string"){} 

arrow function: (arg) => {execute}
addEventListener("keydown", (event) => {console.log(event.key)})


---
Conditionals

if else is same as java (if (bool) {expr;} else if (bool) {expr;} else {expr;})
||, &&, ! as you'd expect. ?? is nullish coalescing and idk what that means

|| and && will find the first true/false value and not check the rest (left to right)
so can be used for short circuiting:
    5 > 3 || alert("false"); // NO ALERTS ISSUED
be warned, it is NOT a stand-in for if/else, it can wreak havoc

double NOT (!!) can be used for converting to bool
    and is faster?! but still less readable
    like !1 for false and !0 for true, good for minifying

switch () {
    case something:
        expr;
        break;
    case other thing;
        expr;
        break;
    default;
        expr;
}

ternary:
    (condition) ? run if true : run if false
    (isBirthday) ? "Happy birthday!" : "Good morning!"
    select.addEventListener('change', () => ( select.value === 'black' ) ? update('black','white') : update('white','black'));

ternary with multiple options:
    let message = (age < 3) ? 'Hi, baby!' :
        (age < 18) ? 'Hello!' :
        (age < 100) ? 'Greetings!' :
        'What an unusual age!';

javascript.info says "use TERNARY to assign variables, don't use them to replace if/else
" The purpose of the question mark operator ? is to return one value or another depending on its condition. 
Please use it for exactly that. Use if when you need to execute different branches of code."


---
Error handling

ReferenceError;
    happens when a variable can't be accessed
    (not initialized, out of scope...)

you can do not only console.log()
but also console.table(array)


---
DOM Manipulation

SELECTION:
    document.querySelector(""); document.querySelectorAll("");
        qAll does not return ARRAY, returns NODELIST
        if NODELIST is missing a function, use Array.from()

    div.display
    .display
    #container > .display
    div#container > div.display
    etc etc

    const container = document.querySelector("#container");
    console.dir(container.firstElementChild);
        (lastElementChild)

CREATION:
    document.createElement("div"); // create in memory, not render
    parentNode.appendChild(childNode);
    parentNode.insertBefore(newNode, referenceNode);

    parentNode.removeChild(childNode); // POPS the node, returning it

ALTERATION:
    element.style.color = "blue";
    element.style.backgroundColor = "blue"; // notice the camelCase
    element.setAttribute('style', 'color: blue; background: white;'); 


---
Python-like custom range()

function* range(start, stop, step = 1) {
    if (stop == null) {
        // one param defined
        stop = start;
        start = 0;
    }

    for (let i = start; step > 0 ? i < stop : i > stop; i += step) {
        yield i;
    }
}


---
Other

equality and comparison work slightly differently
comparison converts the value into number, equality has its own way
null == 0 is false
null > 0 evaluates to 0 > 0 and therefore is false
null >= 0 evaluates to 0 >= 0 and therefore is true

null converts to 0
undefined converts to NaN

you can chain let variables:
let a = 5, b = "beibe", c = 1.1

or also:
let a = 5
  , c = 10

!!!
https://www.theodinproject.com/lessons/foundations-fundamentals-part-1#avoid-var
Knowledge check link 2
shouldn't it link to: https://javascript.info/var ?

