```rust
let s1 = String::from("hello");
let len = get_length(s1);

fn get_length(s: String) -> usize {
	let length = code;
	length
}
```
this works but the string will be lost, to solve this we use references instead of passing the values themselves.

```rust
let s1 = String::from("hello");
let len = get_length(&s1);

fn get_length(s: &String) -> usize {
	let length = code;
	length
}
```

We say the variable s inside get_length borrowed from s1
References live in the stack, and they point to the String which 