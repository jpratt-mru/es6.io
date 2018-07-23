# JP Notes for Wes Bos ES6 Course

## 02

- if you have no arg to an arrow function, need to use ()
  - foo(() => blah);
- brackets on params is optional only if _one_
- the return value can be implicit (though you can make it explicit if you want to!)
- if you want to return an object from an arrow function, you need to "escape" the curly braces with brackets!
  - => ({blah})
- **SWEET TIP** use console.table(array or object literal) to get a sweet table pop up in the console!
- in an object literal, if your attribute and value have the same name, you can omit one
  - instead of race: race, you can just have race
- if you use an => function, then "this" inherits its value from its parent!
- when using default args, if you want to call and "leave a hole", you have to put in "undefined"
  - calcTax(100, undefined, 0.12)
- if you use arrow functions, you don't have access to the arguments object
- **SWEET TIP** Array.from() can turn node lists into true arrays!
- you can nest template literals inside of template literals - nice, because if you're writing code (loop, ternary, method call) inside of one literal, that code can also use literals

### tagged template literals

- **SWEET TIP** you can put `debugger` into your code (as a statement), and the browser will stop there when run
- you can feed a string literal a function name and it will feed that literals **args and all** into the function
  - `function highlight(strings, ...values)`
  - const sentence = highlight `My dog's name is ${name} and he is ${age} years old`
  - strings ==> string array with 3 things "My dog's name is", " and he is ", and " years old"
  - values ==> value[0] is value of ${name} , etc
- **SWEET TIP** css has `contenteditable` attribute! `<span contenteditable>blah</span>` would create a spot the user could click and replace with their own text!

### sanitizing user data with tagged templates

- there is a library (dompurify - look it up) that you can use to sanitize html
- function sanitize(strings, ...values) {
  const dirty = strings.reduce((prev, next, i) => `${prev}${next}${values[i] || ''}`, '');
  return DOMPurify.sanitize(dirty);
  }
  you could then do sanitize`some template literal here that needs to be sanitized`

  ### new string methods

  - .startsWith() / .endsWith()
    - case sensitive
    - startsWith() has one version that takes a starting index
    - endsWith() has one version that takes in the number of characters to "search"
    - no, you can't use a regex with this
  - .includes()
    - has a version that takes a starting index
  - .repeat(x)
    - nice for doing pad functions

### destructuring objects

- const {attr1, attr2, attrx} = someObjectWithThoseAttributes
  - cool! now you have 3 (or whatever) constants called attrx
- const {attr1, attr2} = some.nested.object
  - so you can reach into different layers of an object, too
- const {attr1:foo, attr2:bar} = object with attributes 1 and 2
  - so you can RENAME the attributes! that's cool!
- const { x = 0, y = 12, z = 3} = obj

  - if obj doesn't have any of those attributes, the constant in question comes back with the default value

### destructuring arrays

- if you have const details = ['foo', 'bar', 'baz'] then you can go const [a, b, c] = details
  - note that destructuring objects uses {}, while the same for arrays uses []
- so that's nice with strings, where we can go [a, b, c] = arr.split()
- if there are more things in the array than the number of vars in the destructure, the extras are ignored
  - arr = [1,2,3,4,5], then [a,b,c] = arr gives you a=1,b=2,c=3 and that's it
- if you _want_ the rest as another array, just use the rest (...) operator: [a,b,...c] = arr

### swapping vars with destructuring

- neat trick if you have two vars you want to swap, just go [a, b] = [b, a]

### destructuring functions (well, more precisely function args!)

- if you have a function that returns an object, you can use destructuring to capture all the attributes you want with a single call:
  - const {USD, CAD, MEX} = convertCurrency(100)
  - note that the order doesn't matter, since the destructuring process (with objects) latches on to attribute names
- you can use destructuring to pass in an object to a function
  - if you have function tipCalc({total, tip = 0.15, tax = 0.35}), then you can call tipCalc({tip: 20, total: 200}) etc.
  - then the order of your args doesn't matter (though you do now have to remember the names)
  - if you do that, you can make it possible to call tipCalc() without args and not blow up....but then you have to modify the function so that it's tipCalc({total, tip = 0.15, tax = 0.35} = {})

### the for-of loop

- for-of can be used with Iterables
- for (const blah of interable-blah) { do loop stuff with blah - can even do breaks and continues, unlike forEach }

### the for-of in action

- Array.entries() gives you an ArrayIterator, which has a next() method which returns an Object that has a done property and an array called value, which has the current index and the value at that index
  - so you can get index and values like this for (const [i, val] of array.entries())
- for-of is nice for arguments (the special `arguments` that is available in any function call)
  - for (const arg in arguments) blah

### for-of with Objects

- can't for-of an Object directly
- might be coming in ES2017 (can use a polyfill currently)

### Array.from() and Array.of()

- Array.from(foo) ... you know this, but did you know there is also Array.from(foo, function()), which determines what's in the array that gets built?
  - Array.from(people, person => person.textContent) would (assuming the array is one of those DOM Collections), give you a "real" array with just people names in it
- Array.of(x, y, z) just returns an array of...x,y,z. Not sure why you wouldn't just use square-bracket notation....

### Array.find() and Array.findIndex()

- you know find() already....findIndex() is the same thing, except returning the object found, it returns the index of the object found

### spread operator intro

- useful tip: copy arrays such: copy = [...orig]

### object literal upgrades

- const obj = {
  create() {
  // function stuff here
  }
  }
  instead of const obj = {
  create: function() {

  }
  }

- you can create dynamically named attributes in objects:
  - const key = 'pocketColor';
    const tshirt = {
    [`${key}Altered`]
    }
    you now have an attribute called 'pocketColorAltered'

### promises

- a promise is a bit like an IOU
- promise
  .then(data => blah)
  .then(data => blah)
  .catch(err => blah) // use for "promise NOT fulfilled" case

### building your own promises

- const p = new Promise((resolve, reject) => {
  reject(Error('some error message')) // wrap in Error object to help with stack traces
  })

### working with multiple promises

- if you have multiple response objects, you can react to all of them:
- Promise
  .all([array of promise objects])
  .then(responses => {
  return Promise.all(responses.map(res => res.json()))
  })
  .then(responses => {
  actually do something with the parsed responses
  })
  - the time needed to wait is determined by the slowest
- NOT SURE IF I QUITE GROK THIS YET

### Symbols

- const foo = Symbol('bar') the "bar" is just a descriptor - the underlying value of foo is a unique identifier...kind of like a GUID?
  - even if two symbols have the same _descriptor_, they won't have the same _value_
    - foo = Symbol('baz')
    - bar = Symbol('baz')
    - foo === bar is FALSE
- if you have an object with a number of symbols as properties, you can't easily iterate over them (blah in collection) won't work
  - this makes symbols a useful mechanism for storing private data...I think I've seen this mentioned online when the topic of private vars come up
  - you can iterate with a bit of work:
    Object.getOwnPropertySymbols(some obj with symbol props).map(s => objName[s])
