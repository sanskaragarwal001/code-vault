# Stack and Heap

## Why This Exists

Programs need memory to store data while they run.

Not all data is stored the same way. Some data is small, predictible, and short-lived, while other data can be large, dynamic or have an unknown size at compile time.

To handle these different requirements efficiently, memory is typically divided into:

- **Stack** -> Fast, fixed size memory.
- **Heap** -> Flexible, dynamically allocated memory.

Understanding the difference helps explain why Rust has concepts like ownership, borrowing, `Box<T>`, and lifetime.

---

## Mental Model

Imagine a stack of plates.

### Stack

You can only:
- put a plates on top (**push**).
- Remove a plate from the top (**pop**).

This makes stack operations extremely fast.

Characteristics:
- Stores data in order.
- Size must be known at compile time.
- Automatically cleaned up when a function exits.
- Very fast allocation and deallocation.

```rust
fn main() {
  let x = 10;
  let y = 20;
}
```
Here `x` and `y` are stored on the stack.

---

### Heap

Think of the heap as a large storage warehouse.

When you need space:
1. Ask the allocator for memory.
2. Allocator finds a suitable location.
3. Store the data there.
4. Keep a pointer to that location.

Characteristics:
- Can store dynamically sized data.
- Allocator is slower.
- Requires tracking where the data lives.
- More flexible than the stack.

```rust
fn main() {
  let name = String::new("Rust");
}
```

The `String` object itself is stored on the stack, but the actual text `"Rust"` is stored on the heap.

---

## Syntax

### Stack Allocation
```rust
let x = 42;
let flag = true;
```

Primitive types with know sizes are usually stored directly on the stack.

---

### Heap Allocation

```rust
let s = String::from("hello");
```

The string contents are allocated on the heap.

---

### Explicit Heap Allocation with Box
```rust
let num = Box::new(10);
```

`Box<T>` stores a value on the heap and keeps a pointer on the stack.

---

## Example 

### Stack Example

```rust
fn main() {
  let a = 5;
  let b = a;

  println!("{}", a);
  println!("{}", b);
}
```

Memory:

```text
Stack
+-----+
| a=5 |
+-----+
| b=5 |
+-----+
```

The value is copied because integers are small and implements `Copy`.

---

### Heap Example

```rust
fn main() {
  let s1 = String::from("hello");
  let s2 = s1;

  // println!("{}", s1); // Error
  println!("{}", s2);
}
```

Memory before move:
```text
Stack                     Heap
+---------+              +---------+
| s1 ---- | -----------> | "hello" |
+---------+              +---------+
```

After: 
```rust
let s2 = s1;
```

Ownership moves to `s2`.

```text
Stack                     Heap
+---------+              +---------+
| s2 ---- | -----------> | "hello" |
+---------+              +---------+
```

Rusr invalidates `s1` to prevent double-free bugs.

---

## Performance Considerations

### Stack

✅ Very fast
✅ Automatically managed
✅ Cache friendly
❌ Size must be known at compile time
❌ Limited memory

### Heap

✅ Flexible size
✅ Can store large data
✅ Supports dynamic structures
❌ Slower allocation
❌ Requires memory management
❌ Extra pointer indirection
