String slices are types which contain in the stack:
- A pointer to the start of the slice of the string in the heap
- A length

```rust
let s = "hello world";
```
s is by default a string slice for some reason.