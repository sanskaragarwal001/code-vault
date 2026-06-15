# Structs

## Why This Exists

Programs often need to work with multiple pieces of related data.

Consider a user in an application.

Without a struct, we might store the information in separate variables.

```rust
let username = String::from("alice");
let age = 25;
let active = true;
```

This works for a small example.

However, as programs grow, keeping related values in separate variables becomes difficult.

```rust
let username = String::from("alice");
let age = 25;
let active = true;

let username2 = String::from("bob");
let age2 = 30;
let active2 = false;
```

It quickly becomes unclear which values belong together.

One possible solution is a tuple.

```rust
let user = (String::from("alice"), 25, true);
```

While this groups data together, it introduces another problem.

```rust
println!("{}", user.0);
println!("{}", user.1);
println!("{}", user.2);
```

The meaning of each position is not immediately obvious.

Rust needed a way to:

* Group related data
* Give each piece of data a meaningful name
* Create reusable custom types
* Improve code readability

This solution is called a struct.

A struct allows multiple related values to be stored together under a single type.

---

## Mental Model

Think of a form used to register users.

```text
User Form

Username: ______
Age: ______
Active: ______
```

Each field describes a specific piece of information.

Together, the fields represent a single user.

A Rust struct works the same way.

```text
User
├── username
├── age
└── active
```

Instead of managing several independent variables, we create a single value that contains all related data.

---

## Struct Rules

Rust structs follow several important principles.

---

### Rule 1

A struct groups related data.

```rust
struct User {
    username: String,
    age: u32,
    active: bool,
}
```

The struct defines a blueprint.

It describes what information every `User` should contain.

---

### Rule 2

A struct becomes a new type.

```rust
struct User {
    username: String,
    age: u32,
    active: bool,
}
```

After defining the struct, Rust treats `User` as a completely new type.

```rust
let user: User;
```

Just like:

```rust
let age: u32;
```

---

### Rule 3

Each field has its own type.

```rust
struct User {
    username: String,
    age: u32,
    active: bool,
}
```

Rust validates every field independently.

Attempting to assign an incorrect type results in a compilation error.

---

## Syntax

### Defining a Struct

```rust
struct User {
    username: String,
    age: u32,
    active: bool,
}
```

This creates a new type named `User`.

---

### Creating an Instance

```rust
struct User {
    username: String,
    age: u32,
    active: bool,
}

fn main() {
    let user = User {
        username: String::from("alice"),
        age: 25,
        active: true,
    };
}
```

This creates a value of type `User`.

---

### Accessing Fields

```rust
struct User {
    username: String,
    age: u32,
    active: bool,
}

fn main() {
    let user = User {
        username: String::from("alice"),
        age: 25,
        active: true,
    };

    println!("{}", user.username);
}
```

Output:

```text
alice
```

Field access uses dot notation.

---

### Mutable Structs

A struct instance can be made mutable.

```rust
struct User {
    username: String,
    age: u32,
    active: bool,
}

fn main() {
    let mut user = User {
        username: String::from("alice"),
        age: 25,
        active: true,
    };

    user.age = 26;
}
```

Because the entire instance is mutable, its fields can be modified.

---

## Ownership and Structs

Structs follow Rust's ownership rules.

```rust
struct User {
    username: String,
}

fn main() {
    let user1 = User {
        username: String::from("alice"),
    };

    let user2 = user1;
}
```

Ownership moves from `user1` to `user2`.

After the move:

```text
user1 ❌ invalid
user2 ✅ owner
```

A struct is not special.

Its fields participate in ownership exactly like any other value.

---

## Struct Update Syntax

Rust provides a convenient way to create a new struct using values from an existing one.

```rust
struct User {
    username: String,
    age: u32,
    active: bool,
}

fn main() {
    let user1 = User {
        username: String::from("alice"),
        age: 25,
        active: true,
    };

    let user2 = User {
        username: String::from("bob"),
        ..user1
    };
}
```

The remaining fields are copied or moved from `user1`.

This avoids repeating field values.

---

## Why Structs Matter

Structs are one of Rust's primary tools for modeling data.

They allow developers to:

* Group related values
* Create meaningful custom types
* Improve readability
* Represent real-world entities
* Build larger abstractions

Many Rust programs are built around structs.

They form the foundation for concepts such as methods, traits, and enums that appear later in the language.
