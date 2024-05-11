guardadas en el heap

Hashmaps
secuencias Vec

en ves de my_vec[i]
puedo hacer
```rust
my_vec.get(i)
my_vec.get_mut(i)
```
retornan Option<&T> y Option<&mut T> 

```rust
use std::collections::VecDeque;

fn main() {
	let mut buf = VecDeque::new();
	push_back(val)
	push_push_front(val)
	let val = pop_back()
	let val = pop_front()
}
```

linked list