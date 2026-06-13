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
