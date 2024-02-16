By default, variables are immutable. This is one of many nudges Rust gives you to write your code in a way that takes advantage of the safety and easy concurrency that Rust offers.
When a variable is immutable, once a value is bound to a name, you can’t change that value.

You can make a variable mutable by adding mut before its assignment
```rust
let mut x = 10;
x = 20;
```

 The Rust [[compiler]] guarantees that when you state that a value won’t change, it really won’t change, so you don’t have to keep track of it yourself. Your code is thus easier to reason through.

### Constants
Constants are always immutable, they are declared using const not let.
```rust
const NUMBER_THING: u32 = 40*2
```
The last difference is that constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.

## Shadowing
 you can declare a new variable with the same name as a previous variable. Rustaceans say that the first variable is _shadowed_ by the second, which means that the second variable is what the compiler will see when you use the name of the variable. In effect, the second variable overshadows the first, taking any uses of the variable name to itself until either it itself is shadowed or the scope ends. We can shadow a variable by using the same variable’s name and repeating the use of the `let` keyword as follows:
```rust
 let x = 5; 
 let x = x + 1;
 {let x = x * 2;}
```
Shadowing within a nested block created with the curly brackets When that scope is over, the inner shadowing ends.
Shadowing is different from marking a variable as `mut` because we’ll get a compile-time error if we accidentally try to reassign to this variable without using the `let` keyword. By using `let`, we can perform a few transformations on a value but have the variable be immutable after those transformations have been completed.

The other difference between `mut` and shadowing is that because we’re effectively creating a new variable when we use the `let` keyword again, we can change the type of the value but reuse the same name