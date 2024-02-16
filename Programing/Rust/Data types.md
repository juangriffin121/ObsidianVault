Rust is a _statically typed_ language, which means that it must know the types of all variables at compile time. The compiler can usually infer what type we want to use based on the value and how we use it.
If there's ambiguity in the type you must declare it explicitly:
```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

## Scalar types
A _scalar_ type represents a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters. You may recognize these from other programming languages. Let’s jump into how they work in Rust.

### Integers

| Length | Signed | Unsigned |
| ---- | ---- | ---- |
| 8-bit | `i8` | `u8` |
| 16-bit | `i16` | `u16` |
| 32-bit | `i32` | `u32` |
| 64-bit | `i64` | `u64` |
| 128-bit | `i128` | `u128` |
| arch | `isize` | `usize` |

|Number literals|Example|
|---|---|
|Decimal|`98_222`|
|Hex|`0xff`|
|Octal|`0o77`|
|Binary|`0b1111_0000`|
|Byte (`u8` only)|`b'A'`|
### Floats
### Bools
### Chars

## Compound types
_Compound types_ can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.

### Tuples
~= python, cant be modified
```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
```
The variable `tup` binds to the entire tuple because a tuple is considered a single compound element. To get the individual values out of a tuple, we can use pattern matching to destructure a tuple value, like this:
```rust
let tup = (500, 6.4, 1)
let (x, y, z) = tup;
```
This is called _destructuring_ in rust, unpacking in python

### Arrays
Another way to have a collection of multiple values is with an _array_. Unlike a tuple, every element of an array must have the same type. Unlike arrays in some other languages, arrays in Rust have a fixed length.

```rust
let a = [1, 2, 3, 4, 5];
or
let a: [i32; 5] = [1, 2, 3, 4, 5];
let a = [3; 5]; -> [3, 3, 3, 3, 3]
```
Arrays are useful when you want your data allocated on the [[stack]] rather than the [[heap]] or when you want to ensure you always have a fixed number of elements. An array isn’t as flexible as the vector type, though. A _vector_ is a similar collection type provided by the standard library that _is_ allowed to grow or shrink in size. If you’re unsure whether to use an array or a vector, chances are you should use a vector.
Array elements are accesed like in python, a\[i\]  

