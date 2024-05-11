```rust
enum NombreEnum {
	Variacion1,
	Variacion2,
	Variacion3,
	...
}
```

```rust
NombreEnum::Variacion1
```

Ejemplo 
```rust
enum Rol {
	Padre,
	Hijo
}
```


```rust
enum Rol {
	Padre(i32),
	Hijo(i32)
}
```


```rust
enum Rol {
	Padre(StructPadre),
	Hijo(StructHijo)
}
```

tambien puedo darle metodos
```rust
impl Rol {
	fn hace_algo(self) {
		match
			Rol::PADRE(StructPadre{})
	}
}
```
