In function signatures, you _must_ declare the type of each parameter. This is a deliberate decision in Rust’s design: requiring type annotations in function definitions means the compiler almost never needs you to use them elsewhere in the code to figure out what type you mean. The compiler is also able to give more helpful error messages if it knows what types the function expects.

```rust
fn f(x: u32, letter: char) -> i32 {
	code
}
```
functions can return values when they are ended in expressions instead of statements
- **Statements** are instructions that perform some action and do not return a value.
- **Expressions** evaluate to a resultant value. Let’s look at some examples.

if i try to assign to a variable the result of a statement itll throw an error because statements dont return anything
```rust
let y = {
	let x = 3;
	x + 1
}
```
Note that the `x + 1` line doesn’t have a semicolon at the end, which is unlike most of the lines you’ve seen so far. Expressions do not include ending semicolons. If you add a semicolon to the end of an expression, you turn it into a statement, and it will then not return a value. Keep this in mind as you explore function return values and expressions next.

You can return early from a function by using the `return` keyword and specifying a value, but most functions return the last expression implicitly.