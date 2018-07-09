# JP Notes for Wes Bos ES6 Course

## 02

- if you have no arg to an arrow function, need to use ()
  - foo(() => blah);
- brackets on params is optional only if _one_
- the return value can be implicit (though you can make it explicit if you want to!)\
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
