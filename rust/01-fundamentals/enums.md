# Enums

## Why This Exists

In the previous section, we learned that structs allow us to group related data together.

```rust
struct User {
    username: String,
    age: u32,
}
```

A struct works well when every instance has the same fields.

However, many real-world concepts can exist in multiple forms.

Consider a network message.

A message might:

* Request data
* Send text
* Disconnect from the server

These are related concepts, but they do not necessarily contain the same data.

One possible solution is a struct with many optional fields.

```rust
struct Message {
    text: Option<String>,
    id: Option<u32>,
    disconnected: bool,
}
```

This works, but it has a problem.

Most fields are irrelevant for most messages.

For example:

```text
Send Text
├── text = Some(...)
├── id = None
└── disconnected = false
```

The structure does not clearly communicate which data belongs to which kind of message.

Rust needed a way to:

* Represent a value that can take multiple forms
* Associate data with specific forms
* Express intent clearly
* Maintain type safety

This solution is called an enum.

An enum allows a value to be one of several possible variants.

---

## Mental Model

Think of a traffic light.

At any moment, the light can only be in one state.

```text
Traffic Light

Red
Yellow
Green
```

It cannot be both red and green simultaneously.

It must be exactly one of the available choices.

Enums work the same way.

```text
Message
├── Quit
├── Write
└── Move
```

A value of type `Message` must be exactly one variant at a time.

---

## Enum Rules

Rust enums follow several important principles.

---

### Rule 1

An enum defines a fixed set of variants.

```rust
enum Direction {
    North,
    South,
    East,
    West,
}
```

A value can only be one of these variants.

```rust
let direction = Direction::North;
```

Any value outside this set is invalid.

---

### Rule 2

A value can only be one variant at a time.

```rust
let status = Status::Online;
```

```text
Status
└── Online
```

The value cannot simultaneously be:

```text
Online
Offline
```

It must be exactly one.

---

### Rule 3

Variants can store data.

Unlike enums in many languages, Rust variants may contain values.

```rust
enum Message {
    Write(String),
}
```

Now a variant can carry additional information.

```rust
let msg = Message::Write(String::from("Hello"));
```

The data exists only when that specific variant is active.

---

### Rule 4

Different variants may store different types of data.

```rust
enum Message {
    Quit,
    Write(String),
    Move { x: i32, y: i32 },
}
```

Each variant can define its own structure.

This makes enums extremely flexible.

---

## Syntax

### Defining an Enum

```rust
enum Direction {
    North,
    South,
    East,
    West,
}
```

This creates a new type named `Direction`.

---

### Creating Values

```rust
fn main() {
    let north = Direction::North;
    let south = Direction::South;
}
```

Each value is one variant of the enum.

---

### Enum Variants with Data

```rust
enum Message {
    Write(String),
}
```

Creating a value:

```rust
let msg = Message::Write(String::from("Hello"));
```

The string becomes part of the variant.

---

### Variants with Named Fields

```rust
enum Message {
    Move {
        x: i32,
        y: i32,
    },
}
```

Creating a value:

```rust
let msg = Message::Move {
    x: 10,
    y: 20,
};
```

This variant behaves similarly to a struct.

---

## Enums vs Structs

Structs and enums solve different problems.

### Struct

A struct represents a single shape of data.

```rust
struct User {
    username: String,
    age: u32,
}
```

Every `User` contains:

```text
username
age
```

Always.

---

### Enum

An enum represents multiple possible forms.

```rust
enum Status {
    Online,
    Offline,
}
```

A value is:

```text
Online
OR
Offline
```

but never both.

---

### Comparison

```text
Struct
├── One shape
└── Same fields for every instance

Enum
├── Multiple possible shapes
└── One variant active at a time
```

---

## Ownership and Enums

Enums follow the same ownership rules as any other Rust type.

```rust
enum Message {
    Write(String),
}

fn main() {
    let msg1 = Message::Write(String::from("hello"));

    let msg2 = msg1;
}
```

Ownership moves:

```text
msg1 ❌ invalid
msg2 ✅ owner
```

The data stored inside the enum participates in ownership normally.

---

## Why Enums Matter

Enums are one of Rust's most powerful features.

They allow developers to:

* Represent states
* Model choices
* Associate data with specific cases
* Eliminate invalid states
* Express intent clearly

Many of Rust's most important types are enums.

For example:

```rust
enum Option<T> {
    Some(T),
    None,
}
```

and

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Understanding enums is essential because they form the foundation of pattern matching, error handling, and many APIs throughout the Rust ecosystem.
