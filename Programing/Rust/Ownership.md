If i pass an object into a function as is, the function will take ownership of that object and will deallocate it after the func is done.

```rust
f(x);
f(&x);
```
the first case takes ownership of x and will dealocate it after, while the second one does not.

>[!danger] if you pass a value to a function by value (not as a reference) and the function takes ownership of the value, you won't be able to use the value after the function call. Attempting to do so will result in a compile-time error.