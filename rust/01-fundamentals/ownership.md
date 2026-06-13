# Ownership

## Why this Exists

Memory Management has always been one of the hardest problems in systems programming.

Languages such as C provide direct control over memory throught functions like `malloc()` and `free()`. while this offer excellent performance, it also introduces an important responsibility: The Programmer must manually manage memory.

Conside the following example:
```c
char* create_message() {
  char* msg = malloc(100);
  return msg;
}

int main() {
  char* message = create_message();

  // --snip--

  free(message);
}
```

This looks simple, but several problems can occur:

- Forgetting to call `free()` causes memeory leaks.
- Calling `free()` twice causes undefined behaviour.
- Accessing memory after `free()` creates dangling pointers.
- Large codebases make it difficult to track ownership manually.

As Software grows, memory management becomes increasingly error-prone.

## The C++ Solution: RAII

C++ introduced a concept called **Resource Acquisition is Initialization (RAII)**.

The core idea is simple:
> The lifetime of a resource should be tied to the lifetime of an object.

Instead of manually calling `free()`, resources are released automatically when objects leave scope.

```c++
#include <iostream>
#include <string>

int main() {
  std::String concept_name = "RAII";

  std::cout << concept_name << std::endl;
} // destructor automatically runs here
```

When `concept_name` variable goes out of scope, it's desctructor is automatically called and free the memory.


**RAII** solved many memory management problems by making resource cleanup deterministic.

However, C++ still allows situations that can lead to:

- Use-after-free bugs
- Dangling references
- Ownership confusion
- Data races


## Rust Approach

Rust takes the idea behind **RAII** and makes ownership a language-level rule enforced by the compiler.

Instead of relying solely on developer descipiline, Rust tracks ownership at compile time.

This allows Rust to provide:

- Memory Safety
- Automatic cleanup
- No garbage collector
- No runtime overhead

Many memory-related bugs become compilation errors.

---

### Mental Model

Think of ownerhsip like ownership of a house.

A house can only have one legal owner at a time.
```text
House --> Owner A
```

if ownership is tranferred:
```text
House --> Owner B
```

Owner A no longer owns the house.

The same principle applies to Rust.

when ownership moves from one variable to another, the previous variable becomes invalid.

This guarantees that there is always a single owner responsible for cleaning up the resource.

---

### Ownership Rules

Rust enforces three fundamental rules:

1. Each value has an owner.
```rust
let s = String::from("hello");

//  The variable `s` owns the string.
```
---

2. There can only be one owner at a time.
```rust
let s1 = String::from("hello");
let s2 = s1;

// Ownership moves from s1 to s2
```

After the move:
```text
s1 ❌ invalid
s2 ✅ valid owner
```

> [Note]
> Attempting to use `s1` results in a compiler error.

---

3. When the owner out of scope, the value is dropped.
```rust
{
  let s = String::from("Hello");
} // drop(s) automatically called here
```

Rust automatically frees the memory.

No explicit `free()` call is required.

---

## Syntax

### Ownership Through Functions
```rust
fn consume(s: String) {
  println!("{}", s);
}

fn main() {
  let s = String::from("hello");

  consume(s);

  // s is no longer valid, comment this line
  println!("{}", s); 
}
```

The ownership of the String move to the `consume`, and when call finish the value was dropped.

Once moved, Rust prevents accidental access through the original variable.

Without ownership tracking, bugs such as use-after-free could occur.

Rust prevents them during compiling.
