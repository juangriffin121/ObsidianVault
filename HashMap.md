---
id: HashMap
aliases:
  - HashMap
tags: []
---

# HashMap

The HashMap is a key-value pair data structure that uses a [[Hash Function]] to relate the key data to the address of the value in memory. examples of it are rusts HashMap and python's dictionary.

## Under The Hood
Internally to locate the value, you hash the key and perform one of this operations (depending on the programing language):
    - `hash(key) % capacity`
    - `hash(key) & (capacity - 1)` bitmasking (keeps the lowest log_2(capacity - 1) bits of the hash used when capacity is power of two, used by rust, the hash function should spread the randomness to the lower bits for this to work)
        0000001111
        &
        0110111010
        =
        0000001010
When the hashes dont collide but the buckets do, there are a few solutions, none of which are that good but whatever
1. Separate Chaining 🧶

Each slot holds a list of key-value pairs.

So `bucket[2]` will look like:

`bucket[2] = [ ("cat", "meow"), ("dog", "woof") ]`

To get "dog", we:

    Hash "dog" → index 2

    Scan the list at bucket[2] until we find "dog"

✔️ Pros: simple to implement
❌ Cons: performance degrades if many keys land in the same bucket (becomes like a linked list)
2. Open Addressing 🛣️

Instead of putting multiple items in one bucket, we find another open slot.

Let’s say:

bucket[2] already has "cat"

We look for the next free spot:

    Try index 3 → if empty, insert "dog" there

This is called probing — strategies include:
a. Linear probing

Try index + 1, index + 2, … until you find a free slot
b. Quadratic probing

Try index + 1^2, index + 2^2, …
c. Double hashing

Use another hash function to decide the next step size

So now:
```
bucket[2] = ("cat", "meow")
bucket[3] = ("dog", "woof")
```

To find "dog":

    Hash to 2 → it’s not "dog"

    Try index 3 → found it!

✔️ Pros: No extra memory for lists
❌ Cons: Can lead to clustering and performance drops when the table is too full





