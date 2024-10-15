# Memory addressing problem
A pointer is a variable that stores the memory address of another variable.

# Theory:
Pointers are crucial in low-level programming because they allow direct manipulation of memory. A pointer points to a memory location and can be used to access the value stored at that location.

### Declaration:
To declare a pointer to an integer:

```c
int *ptr;
int x = 10;
ptr = &x;  // pointer points to the address of x
```

### Dereferencing
Dereferencing a pointer means accessing the value stored at the memory address it points to:
```c
int y = *ptr;  // y now holds the value of x (10)`
```

# Advantages
- **Efficiency**: Pointers allow direct memory access, which is faster for certain operations.
- **Memory Management**: Pointers are used in dynamic memory allocation (e.g., `malloc` in C).
- **Function Arguments**: Passing pointers allows functions to modify variables outside their local scope (by reference).