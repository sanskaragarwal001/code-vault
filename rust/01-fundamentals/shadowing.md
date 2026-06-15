# Shadowing

## Why This Exists

Variables are often transformed throughout a program.

Consider the following example:
```rust
let input = "42";
```

Suppose we want to convert the string into a number.

One approach is to create a new variables.
```rust
let input = "42";
let parsed_input = input.parse::<i32>().unwrap();
```

This works, but as programs grow, developers often end up calling many temporary variables.
```rust
let input = "42";
let parsed_input = input.parse::<i32>().unwrap();
let doubled_input = parsed_input * 2;
```

The intent is simple:

1. Start with a string.
2. Convert it into a number.
3. Transform the number.

However, multiple variable names can make the code harder to follow.

Rust provides a feature called shadowing.

Shadowing allows a new variable to reuse the name of existing variable.
```rust
let input = "42";
let input = input.parse::<i32>().unwrap();
let input = input * 2;
```

Each `input` is a complete new variable.
The previous variable becomes inaccessible.

This allow transformatations to be expressed naturally while keeping the code concise.

---

## Menatal Model

Think of a whiteboard in an office.

Initially the board contains:
```text
text = "25"
```

Later, someone erases the contents and writes

```text
Age = 25
```
The board still has the same label:
```text
Age
```

But the value written on it has changed.

In Rust, shadowing works similarly,
```rust
let age = "25";
let age = age.parse::<i32>().unwrap();
```

The second `age` shadows the first one.
The original variable still existed, but it is no longer accessible through the name `age`.

---

## Shadowing Rule

Rust follows several important rules regarding shadowing.

---

### Rule 1

Shadowing creates a new variable.

```rust
let x = 5;

let x = 10;
```

The second `x` is not modifying the first variable.

It creates an entirely new variable with the same name.

```text
x = 5
  ↓
shadowed
  ↓
x = 10
```

---

### Rule 2

Shadowing does not require mutability.

```rust
let x = 5;

let x = x + 1;
```

Notice that `mut` is not used.

This is valid because a new variable is being created.

No mutation occurs.

---

### Rule 3

Shadowing can change the type.

```rust
let value = "42";

let value = value.parse::<i32>().unwrap();
```

Before shadowing:

```text
value -> &str
```

After shadowing:

```text
value -> i32
```

This is allowed because the second variable is completely new.

---

## Shadowing vs Mutability

Shadowing and mutability are often confused.

Although they may appear similar, they solve different problems.

### Mutable Variable

```rust
let mut count = 0;

count += 1;
```

The same variable is modified.

```text
count
  0
  ↓
  1
```

The type must remain the same.

---

### Shadowing

```rust
let count = "0";

let count = count.parse::<u32>().unwrap();
```

A new variable is created.

```text
count (&str)
      ↓
shadowed
      ↓
count (u32)
```

The type may change.

---

## Syntax

### Basic Shadowing

```rust
fn main() {
    let language = "Rust";

    let language = "Rust Programming Language";

    println!("{}", language);
}
```

Output:

```text
Rust Programming Language
```

The second variable shadows the first one.

---

### Transforming Data

```rust
fn main() {
    let number = "10";

    let number = number.parse::<i32>().unwrap();

    let number = number * 2;

    println!("{}", number);
}
```

Output:

```text
20
```

Each step creates a new variable while preserving the same name.

---

### Shadowing in Inner Scopes

```rust
fn main() {
    let x = 5;

    {
        let x = 10;

        println!("{}", x);
    }

    println!("{}", x);
}
```

Output:

```text
10
5
```

The inner `x` only exists inside the nested scope.

Once that scope ends, the outer `x` becomes visible again.

---

## Why Shadowing Matters

Shadowing allows developers to:

* Reuse meaningful variable names
* Express transformations clearly
* Avoid unnecessary temporary variables
* Change a variable's type safely
* Preserve Rust's preference for immutability

Rather than mutating existing values, Rust encourages creating new values when data changes.

Shadowing provides an ergonomic way to do exactly that.
