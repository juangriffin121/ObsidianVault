enum disponible en standard lib
dos variantes, Some() y  None

rust nos obliga a manejar explicitamente los vacios

```rust
struct Persona {
	nombre: String,
	apellido: String,
	dni: Option<i32>
}
```

para generar una persona
```rust
let p1 = Persona {
	nombre: "Juan",
	apellido: "griffin",
	dni: Some(42455..)
}
```