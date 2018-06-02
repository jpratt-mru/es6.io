# JP Notes for Wes Bos ES6 Course

## 02

* if you have no arg to an arrow function, need to use ()
  * foo(() => blah);
* brackets on params is optional only if _one_
* the return value can be implicit (though you can make it explicit if you want to!)\
* if you want to return an object from an arrow function, you need to "escape" the curly braces with brackets!
  * => ({blah})
* **SWEET TIP** use console.table(array or object literal) to get a sweet table pop up in the console!
* in an object literal, if your attribute and value have the same name, you can omit one
  * instead of race: race, you can just have race
* if you use an => function, then "this" inherits its value from its parent!
* when using default args, if you want to call and "leave a hole", you have to put in "undefined"
  * calcTax(100, undefined, 0.12)
* if you use arrow functions, you don't have access to the arguments object
* **SWEET TIP** Array.from() can turn node lists into true arrays!
