A string is a type that contains in the stack:
- A pointer to a space in the heap
- A length
- A capacity: The maximum length allowed.
Strings are useful when dealing with texts which cant be known at compile time their lengths, like stdin inputs, so they must live in the heap.