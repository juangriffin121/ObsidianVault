Accesing methods or atributes from an object is done with . e.g: object.method or object.attr
Accesing things from modules and such is done with :: like use std::io

This is allowed
```rust
io::stdin() 
	.read_line(&mut index) 
	.expect("Failed to read line");
```