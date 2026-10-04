# Chapter 10 - Pointers

## C++ and Memory

One of the defining features of C++ as compared to other programming languages is its ability to directly access and manipulate the computer's physical memory. Pointers are one of the ways that we can do this. At its most basic, a **pointer** is a variable that holds a memory address.

When the Operating System runs a program, it assigns it a block of memory, usually at least 3GB. C++ organized the memory into the following sections:

- Code Segment - compiled machine instructions are held here.
- Static/Global Data Segment - global and static variables and constants
- Heap - Used for dynamically allocated memory. This is requested via the **new** keyword. This memory is managed by the programmer.
- Stack - Used for local variables and to keep track of function calls and return addresses. This is allocated on a last in / last out structure.

Examples:

1. if we create a normal variable in our program with a statement such as:

```cpp
int x = 12;
```

4 Bytes of contiguous memory are reserved from  the top of the stack and used to hold 12. The beginning memory address is saved as being for x. x points to the value being stored at address x. To print it out, just do:

```cpp
cout << x;
```

2. if we dynamically allocate a variable with a statement such as:  
   `int* x = new int(12);`

4 Bytes of contiguous are reserved from the heap and used to hold 12. In this case, x holds the memory address of x, so it is a pointer. We will talk more about pointers in the rest of this chapter.

No matter if you allocate a variable (or array) to the stack, the operating system will automatically release the memory when the variable goes out of scope. If we dynamically allocate a variable, it is the programmers responsibility to free it using the **delete** keyword. Programs that do not delete dynamic memory are said to contain a **memory leak**, which means that memory is allocated but never released. This memory cannot be reused for the entire run of the program.

## Introduction to Pointers

How we handle pointers depends on whether the variables are declared normally in our code and stored in the stack, or declared dynamically and stored in the heap. We will deal with each case separately.

### Variables stored in the Stack

Let's start by declaring two ints, x and y, and giving them the values 10 and 20 respectively. Assume that the variable x is stored at memory address 100, which would make y stored in memory address 104.

```cpp
int x = 10;
int y = 20;
```

We use x and y to refer to the values 10 and 20. If we want to get the memory address that x is stored in, we can use the & operator. So, in our case, &x = 100 and &y =104.

We can also create a pointer to defer to the address of x and y. To do this, we first have to create two pointers to ints. A pointer can only point to the type it was initialized to. We can now declare our pointers and point them to the address that x and y are stored at as follows:

```cpp
int *px = &x;
int *py = &y;
```

So, px is equal to the memory address 100 and py is equal to the memory address 104.

## 1. Introduction to Pointers

A pointer is a variable that holds a memory address. Typically we use pointers to point to the memory address of something, be it a variable or an array. We say that the pointer is a "reference" to a variable.

Why do we use pointers?

One of the most powerful features of C++ is its ability to manage memory directly. Pointers are the way we can do that. We use pointers in the following situations:

- Dynamic memory management with the **new** and **delete** commands
- Passing references to a function
- To create data structures link **linked lists**

We can think of memory as a series of cells as in a spreadsheet. Each cell represents 1 byte. Every time we run a program, the Operating System gives us a certain number of cells to work with. The program code is loaded in one part of this memory. Our variables are loaded in another. There is also room left for dynamic memory, or memory that is allocated during the running of a program.

When we create a new variable, the system will allocate a portion of that memory to store the variable in. The amount of space it needs depends on the type of variable involved. On most systems, ints and floats take up 4 bytes, while doubles take up 8 bytes. Remember, a byte is 8 bits, and a bit is either a 1 or a 0. So if we initialize a new int variable x, the system will find a 4-byte contiguous block of memory and mark it as reserved for our new variable x. Note that this will be different every time we run the program.

So, how do we create a pointer to x, or more precisely, to the memory address where x currently lives? We do this using two special operators.

| Symbol | Meaning |
| --- | --- |
| \* | Dereference Operator - returns the value at that address |
| & | Address-of Operator - returns the memory address of a variable |
| int\* p | creates a pointer to a int called p |

Note that pointers are typed just as variables are. So if you want to have a pointer to remember the address of an **int**, you need to create an **int** pointer.

---

## 2. Declaring and Using Pointers

Let's go into more detail on the two operators. Here is an example of creating some pointer variables and assigning them to specific variables.

```cpp
// Declare some variables
int x;
double y;

// Declare some pointers and point them to the address of the variables
int *ptrX = &x;
double *ptrY = &y;
```

In the first pointer declaration, we are really saying "create a pointer to an int called ptrX and assign it the value of the memory address where the variable x lives."

- Declaring pointer variables
- The address-of operator (&)
- The dereference operator (\*)
- Basic pointer assignment and dereferencing
- Pointer vs variable: key differences

---

## 3. Pointers and Functions

- Passing arguments by pointer
- Modifying values in the caller
- Comparison with pass-by-reference
- Pointers as return values

---

## 4. Pointers and Arrays

- Relationship between arrays and pointers
- Accessing array elements using pointers
- Pointer arithmetic (incrementing, indexing)
- Common mistakes: out-of-bounds, uninitialized pointers

---

## 5. Dynamic Memory Management

- The new and delete operators
- Allocating single variables and arrays
- Memory leaks and how to avoid them
- nullptr and checking for null pointers

---

## 6. Pointers to Structures and Classes

- Declaring and accessing members using pointers
- The arrow operator (->)
- Pointers to objects vs object instances

---

## 7. Pointers and Const

- const pointers vs pointers to const
- When and why to use const with pointers

---

## 8. Common Pointer Pitfalls

- Dangling pointers
- Memory leaks
- Double deletes
- Wild/uninitialized pointers

---

## 9. Smart Pointers (Preview for Advanced Topics)

- Introduction to std::unique\_ptr and std::shared\_ptr
- Why raw pointers are discouraged in modern C++
- Transitioning to RAII (Resource Acquisition Is Initialization)

---

## 10. Summary and Best Practices

- When to use raw pointers
- Alternatives (references, smart pointers, containers)
- Key takeaways

---

## 11. Exercises and Labs

- Trace pointer values and memory locations
- Write a swap function using pointers
- Dynamically allocate and deallocate memory
- Create and manipulate arrays using pointers
- Build a small linked list using pointers (advanced)

---

## 📎 Appendices (optional)

- Visual diagrams of memory layout
- Debugging tips for pointer-related bugs
- FAQ: segmentation faults, runtime errors, etc.

---
