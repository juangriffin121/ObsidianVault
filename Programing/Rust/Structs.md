~Objecto
```rust
struct NobmbreDelStruct {
	atributo_1: type1,
	atributo_2: type2
}

let my_struct: NombreDelStruct = NombreDelStruct{
	atributo1: my_atributo1,	
	atributo2: my_atributo2,
}
```

si tengo un struct con atributos llamados :nombre, apellido, dni, y tengo tres variables nombre, apellido, dni puedo generarla asi:
```rust
let persona1 = Persona{
	nombre,
	apellido,
	dni
}
```
para acceder a los atributos:
```rust
Persona.apellido
```

generar personas con todo de persona 1 menos el nombre
```rust
let persona2 = Persona{
	nombre = "Juan",
	..persona1
}
```

## Tuple Structs
```rust
struct Coordenada(f32, f32);
```

## Metodos

```rust
impl Coordenada {
	// le mando self por referencia porque no quiero que el metodo me desaparezca a self
	fn es_la_plata(&self) -> bool{
		code
	}
}

let my_pos: Coordenada = Coordenada(69.0, 420.0);
my_pos.es_la_plata()
```

funciones asociadas no metodos:
```rust
impl Coordenada {
	fn new(x, y) {
		Coordenada{x, y}
	}
}

Coordenada::new(23.0, 24.0)
```

>[!info]
>```
#[derive(debug)]



