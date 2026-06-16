# Option & Result

## Why This Exists

Many programming languages have two common problems:
1. **Missing values** (`null`, `nil`, `undefined`)
2. **Error Handling** (exceptions, error codes)

These approaches often lead to bugs:
```javascript
user.name.toUpperCase();
// Crash if user is null
```
Or:
```java
String name = getUser().getName();
// NullPointerException
```

In 2009, Tony Hoare, who introduced the null reference, called it:
> "My billion-dollar mistake."

The idea was simple: allow a reference to point to nothing.

The problem is that programmers often forget to check for null values, leading to crashes and security vulnerabilities.

Rust solves this by making absence and errors explicit through the type system.

---

## Mental Model

Think of Rust as forcing you to answer two questions:

### Option
> "What if there is no value?"

### Result
> "What if the operation fails?"

Instead of hiding these possibilities, Rust referes you to handle them.

---

## Option<T>
### Why Rust Uses Option Instead of Null

In Rust, a value can either:
- Exist (Some)
- Not exist (None)

This is represented using the `Option<T>` enum.

```rust
enum Option<T> {
  Some(T),
  None,
}
```
There is no hidden null value.

The compiler knows that a value might be missing and forces you to deal with it.

--- 

### Syntax

```rust
let username = Some("alice");
let missing_user: Option<&str> = None;
```

Example

#### Without Option (Hypothetical)
```text
User
 └─ name = null
```

#### With Option
```rust
let username = Some("alice");
let username: Option<&str> = None;
```

The compiler knows both cases are possible.

---


# Result <T, E>

## Why This Exists
Some operation can fail.

Examples:
- Reading a file
- Parsing a number
- Making a network request
- Connecting to a database

Many languages use:
- Exceptions
- Error codes
- Special return values

Rust treats failure as ordinary values.

## Error As Values
In rust:

```rust
enum Result<T, E> {
  Ok(T),
  Err(E),
}
```

An error is not something magical. It is simply another value.

## Mental Model

Instead of:
```text
Do operation
↓
Maybe throw exception
↓
Program jumps elsewhere
```

Rust does:
```text
Do operation
↓
Return success OR failure
↓
Caller decides what to do
```
Control flow remain explicit.

---

## Syntax

```rust
let result: Result<i32, String> = Ok(42);
```

or 

```rust
let result: Result<i32, String> = Err(String::from("Something failed"));
```

---

## Example

Parsing a number:

```rust
let number: i32 = "42".parse()?;
```

Type:

```rust
Result<i32, ParseIntError>
```
