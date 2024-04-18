```rust
let f = |x| x*x;
//or
let f: |...| -> {i32} = |x: i32| {
	let fl: f32 = 0.3;  
	if x > 1 {
		x*x
	}
	else {
		fl*x
	}
};
```

Some case I used it in
```rust
pub fn map<T, F, R>(f: F, array: Vec<T>) -> Vec<R>
where
    F: Fn(T) -> R,
{
    let mut mapped = Vec::new();
    for element in array {
        mapped.push(f(element))
    }
    mapped
}

pub fn f(a: f32, b: f32) -> f32 {
    a * b
}

fn function_given_parameter<Tb, Ta, F, F_b, R>(f: F, b: Tb) -> impl FnOnce(Ta) -> R
where
    F: FnOnce(Ta, Tb) -> R,
{
    let closure = |a: Ta| f(a, b);
    closure
}

map(function_given_parameter(f, 3.0), vec![1.3, 1.1, 1.9])
```