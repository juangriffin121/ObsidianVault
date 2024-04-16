In rust a value is owned by a variable name.
A value can only have one owner at a time.
When the variable scope where it was declared is done, the memory is deallocated.

Ownership and [[Borrow]] rules apply to types whos data is stored in the heap ([[Heap and Stack]])
this makes it so that only the types with costly clone methods are better borrowed.

```rust
let s: String::from("hello");
let s1 = s;
println!(s);
```
this gets compile error bc s no longer owns the data

```rust
let x = String::from("hello");
something(x);
println!(x)

fn something(input) {
	code
}
```
this compile error is because the name x no longer owns its value, it went to the parameter input and then got deallocated when the function ended

