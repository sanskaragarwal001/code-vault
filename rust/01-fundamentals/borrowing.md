# Borrowing

## Why This Exists

Ownership guarantees memory safety by ensuring that every value has a single owner.

However, ownership alone is too restrictive.

Consider the following example:

```rust
fn say_hello(message: String) -> {
  println!("{}", message);
}

fn main() {
  let msg = String::from("hello, rust!");

  say_hello(msg);

  println!("{}", msg);
}
```

The code fails to compile.

When `msg` is passed to `say_hello`, ownership moves into the function, After the move, `msg` is no longer valid.

While ownership prevents memory-related bugs, constantly transferring ownership would make programs difficult to write.

One possbile solutin is cloning:
```rust
let copy = msg.clone();
```

However, cloning allocate new memory and can become expensive.

Rust needed a mechanism that would:
- Allow access to data
- Avoid transferring ownership
- Avoid unnecessary copying
- Maintain memory safety

This mechanism is called borrowing.

Borrowing allows temporary access to a value without becoming its owner.

---

Mental Model

Think of ownership as owning a house.

```text
House --> Owner
```

The owner has full control over the property. The can live in it, renovate it, sell it, or transfer ownership to someone else.

Now suppose someone wants to use the house temporarily.

Instead of selling the house, the owner rents it out.
```text
House --> Owner

Visitor --> Renter (Temporary access)
```

The renter can use the house, but they do not own it.

The important point is that ownership never changes.

The owner remains responsible for the property, while the renter only receives temporary access.

Borrowing in rust works the same way.

The owner of a value can temporarily lend access to other parts of the program without tranferring ownership.

----

## Borrowing Rules

Rust enforces three borrowing rules.

### Rule 1
You can have multiple immutable borrows.

```rust
fn main() {
  let name = String::from("Rust");

  let a = &name;
  let b = &name;
  let c = &name;
}
```

All three borrows can co-exists because none of them can modify the data.

---

### Rule 2

You can have exactly one mutable borrow.

```rust
fn main() {
  let mut name = String::from("Rust");

  let borrow = &mut name;
}
```

The mutable borrow gains exclusive access.
No other borrows may exists simultaneoulsy.

---

### Rule 3

You cannot mix mutable and immutble borrows.

```rust
fn main() {
  let mut name = String::from("Rust");

  let a = &mut name; // Mutable borrow
  let b = &name; // Immutable borrow
}
```

This is not allowed.

If mutation is possible while immutable readers exists, data consistency can no longer be guaranteed.

Rust prevents this situation during compilation.

---

## Why Mutable Borrowing Is Restricted.

Imagine multiple parts of a program accessing the same value.
```text
Reader A
Reader B
Writer
```

If the writer modifies the value while readers are using it, the readers may observe inconsistent data.

Many programming languages allow this situation and rely on the programmer to avoid mistakes.

Rust prevents it entirely.

Its borrowing rules ensure that:
```text
Many Readers
OR
One Writer
```

but never both at the same time.

This principle is one of the foundations of Rust's memory and concurrency safety.

---
