<!-- nav -->
[Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Chapter 3 →](03-control-structures.md)

# Chapter 2 - Variables and Data Types, Constants and Input/Output

## 2.1 Variables

As in Python, variables in C++ are not the same as variables in mathematics. In math, a variable is an unknown. In C++, a variable is a named location in memory that can be used to store data. When you use a variable name, C++ knows to look in the memory address pointed to by the name. Unlike in Python, variables in C++ have to be declared and be of a consistent type.

In Python, the following is legal:

```text
x = 24
x = "Bob"
```

In C++, once x is initialized as an int, it must remain an int:

```cpp
int x = 5;
```

`x = "Bob";` would give an **invalid conversion** error.

The operating system allocates a chunk of memory to each running program. When we initialize a variable in C++, the OS will reserve an area of memory to hold data. The size of this area will depend on the type of variable declared, and to some degree on the operating system you are using. On ludwig, any variable declared as an **int** (integer) will take up 4 bytes (32 bits). The range of integers is thus -2,147,483,648 to 2,147,483,647.

We declare an int with the following command:

```cpp
int x = 5;
```

This command creates a 4-byte area in the program's memory, assigns the name x to that location, and places the number 5 in that area. Let’s say the memory address happens to be 0x1000. When we execute a line such as:

```cpp
cout <<  x  << endl;
```

the program goes to location 0x1000 in memory, sees that there is a 5 stored there, prints 5 to the screen, and then goes to the next line.

It is possible to declare a variable without giving it a value, but do not use it until you have assigned one. A local variable that is declared but not given a value holds whatever happened to be in that memory before, and reading it is a bug. On some systems, the leftover value happens to be zero, which makes the bug easy to miss. Do not rely on it. Give every variable a starting value when you declare it, such as `int x = 0`.

## 2.2 Data Types

| Type | Min | Max | Example |
| --- | --- | --- | --- |
| int | -2,147,483,648 | 2,147,483,647 | int miles=245; |
| float | -3.40282 x 10<sup>38</sup> | 3.40282 x 10<sup>38</sup> | float pi = 3.14; |
| double | -1.79769 x 10<sup>308</sup> | 1.79769 x 10<sup>308</sup> | double r = 23.456; |
| char | -128 | 127 | char c = 'A'; |
| bool | false (0) | true (1) | bool isReady = true; |

Notes on data types:

If you move past the range of **int**, you will wrap around to the smallest number. Note that this is technically undefined behavior, but it is what actually happens.

If **doubles** or **floats** exceed their limits, they return a positive or negative infinity.

If **doubles** or **floats** get too small, they change to 0.

Due to precision limits, not all real numbers can be represented as **doubles** or **floats.**

**chars** usually store the ASCII value of characters. For example, ‘A’ is 65 in ASCII or 01000001

in Binary.

**bool** variables must be either true or false.

There are also strings that act superficially like strings in Python. This will be covered more later. Example:

```cpp
string name = "Bob";
```

## 2.3 Typecasting

C++ gives you the ability, with restrictions, to convert a variable from one data type to another. This is called **typecasting**, of which there are two types.

### 2.3.1 Implicit Typecasting

Also called **type promotion**, this happens when the compiler automatically converts the type of a variable. If the conversion broadens the variable (i.e. int to double) there is no risk of data loss. It is a more concerning case if the typecasting narrows the variable (i.e. double to int). This occurs most frequently when doing arithmetic operations with ints and floats.

Example 1

```cpp
int x = 5;
float y = x;           // automatic type promotion from 5 to 5.0
```

Example 2

```cpp
int x = 5;
float y = 2.4
float prod = x * y;        // automatic type conversion of the int 5 to
                             //    5.0 before multiplication
```

### 2.3.2 Explicit Typecasting

This occurs when the programmer specifies the conversion in code. C++ offers a couple of options on how to do explicit typecasting. The most common is a static\_cast, which allows for conversions between compatible types such as ints and floats.

Example

```cpp
int x = 5;
float y = static_cast<float>(x);
```

## 2.4 Constants

Constants are variables whose value does not change. By tradition, we name constants with capital letters. The C++ compiler enforces immutability on constants. Python does not have the ability to enforce constants.

You declare a constant with the `const` keyword before a variable declaration. You must give it a value when you declare it. Example:

```cpp
const int MAX_STUDENTS = 50;
```

You use constants to ensure that a value of a variable is not changed during the execution of a program. Constants are often used to avoid “magic numbers,” which are numeric literals that are placed in code with no explanation.

Example:

In a program you see the following code:

```cpp
pv = futureValue / pow(1 + .25, periods);
```

What does the .25 represent? There is not much context to know. In this case, .25 is an interest rate. The code would be more readable if modified to this:

```cpp
const double interestRate = .25;
pv = futureValue / pow(1 + interestRate, periods);
```

Another advantage to using constants instead of literals is easier maintenance. Let’s say the interest rate of .25 is used dozens of times in your code as numeric literals. One day, the interest rate changes to .35. Now you have to go through and find each instance of the interest rate and change it one by one. If instead you had declared a const double at the beginning of your code, and used that constant in all calculations, then you only need to change the line where *interestRate* is declared.

## 2.5 Input (from keyboard) and Output (to Screen)

Input and output in C++ are based on the idea of streams. The streams model data flow from a source to a destination. We have already seen an example of output to the screen. Let’s look at that in more detail.

Output Stream

**cout** is the standard output. In our case this will be the screen. We use the output **stream operator <<** to place things on the output stream to end up on the screen.

```cpp
string name = "Bob";
cout << "Hello " << name << endl;
```

First, “Hello” is added to the stream. Then the “Bob”, which is the value of name. Then an endl which flushes the output buffer and creates a new line.

If we reverse the stream operators from << to >>, we get hundreds of lines of errors.

Input Stream

**cin** represents the standard input, which is the keyboard. We use the **input operator >>** to move things from the keyboard to a variable. This allows us to ask the user for information.

Example:

```cpp
int age;
cout << "Enter your age ==> ";
cin >> age;
```

This combination of **cout** then **cin** allows us to ask for the age, and then capture what the user types to the variable age. Note that we are not doing any error checking here. If the user enters an invalid response, unpredictable results may occur. We will learn to check for these kinds of issues later.

We can also get a series of values from the user.

Example:

```cpp
int age1, age2, age3, age4, age5;
cout << "Enter 5 ages separated by a space ==> " ;
cin >> age1 >> age2 >> age3 >> age4 >> age5;
```

Even if the user presses enter, the program will not move forward until 5 values have been input.

Please note that cin can behave unexpectedly when strings with spaces are entered.

## 2.6 Arithmetic

The mechanics of arithmetic are basically the same in C++ as in Python. There are some significant differences at a deeper level, which we will discuss below.

### 2.6.1 Basic Operations

| Operator | Operation | Example |
| --- | --- | --- |
| + | Addition | `int a = 5;`<br>`int b = 2;`<br>`int x = a + b;`<br>`// result is 7` |
| - | Subtraction | `int a = 5;`<br>`int b = 2;`<br>`int x = a - b;`<br>`// result is 3` |
| \* | Multiplication | `int a = 5;`<br>`int b = 2;`<br>`int s = a * b;`<br>`// result is 10` |
| / | Division | `int a = 5;`<br>`int b = 2;`<br>`int s = a / b;`<br>`// result is 2`<br>`float a = 5;`<br>`float b = 2;`<br>`float s = a/b;`<br>`// result is 2.5` |
| % | Modulus | `int a = 5;`<br>`int b = 2;`<br>`int s = a % b;`<br>`// returns the remainder`<br>`// of a/b`<br>`// result is 1` |

Mixed type operations are allowed.

Example:

```cpp
int x = 9;
float y = 2.5;
cout << x + y << endl;

// result will be 11.5. x will be implicitly typecast to 9.0
```

### 2.6.2 Differences from Python

Since C++ is a statically typed language and Python is dynamically typed, there are some differences in how arithmetic is handled. The biggest issue is overflow. In Python, a variable can grow as large as the memory allows. In C++, if you go past the maximum value for the type, you will wrap around to the minimum value and get incorrect results.

If we were to create a program that implements the factorial operation on ints in C++, we could only calculate up through 12! successfully. When we try with any number bigger than 12, we get incorrect results. The same program in Python would successfully calculate factorials in excess of 500 with no issues.

### 2.6.3 Order of Operations

The order of operations lets us know the order that arithmetic operations will be performed when there are multiple operations in a single expression. C++ follows Python, and indeed most programming languages in implementing the mnemonic device PEMDAS. When interpreting this, we need to realize that some of these are on the exact same level. Let’s go through the letters.

P - Parentheses - These have the highest priority and are always done first.

E - Exponents - Exponents are done next. (using pow(), introduced in Chapter 5)

MD - Multiplication and Division - these are at the same level.

AS - Addition and Subtraction - also at the same level.

If there is more than one operation at the same level of priority, they are done from left to right.

You often see folks arguing online about what the “real” answer is when doing multiple things in one line. One such program that causes arguments is the following:

6 ÷ 2(1 + 2)

Start by converting this into C++ form:

```text
6 / 2 * (1 + 2)
```

The highest priority is the parentheses, so we do 1 + 2 first. This gives us:

```text
6 / 2 * 3
```

This is where the arguments start. Do we do the multiplication or the division first? The correct answer is that since they are at the same level of priority, we do the division then the multiplication (left to right). This gives us:

```text
3 * 3
```

= 9

We can write a quick program to verify this.

```cpp
// Program to test PEMDAS
#include <iostream>
using namespace std;

int main()
{
    // Initialize an integer n to use in calculations
    int n;

    // Save the results of our calculation to n
    n = 6 / 2 * (1 + 2);

    // Print out the results
    cout << n << endl;

    return 0;
}
```

When we run this program, it prints out 9.

### 2.6.4 Converting Formulas to C++ Form

If you have a formula in mathematical format, you will have to make some changes to implement it in C++ code. Here are some examples.

**Example 1**. Determining the slope of a line between point 1 (x1, y1) and point 2 (x2, y2).

Formula in mathematics notation:

We start our conversion by putting some parentheses for the numerator and denominator with the division sign in between.

```text
(numerator) / (denominator)
```

We now add in the specifics:

```text
m = (y2 - y1) / (x2 - x1)
```

Sample program to calculate the slope given two points.

```cpp
#include <iostream>
using namespace std;

int main()
{
  // Initialize variables
  int m, x1, y1, x2, y2;

  // Ask users for x and y values of the two points
  // Note: we are not checking for valid values
  cout << "Enter the x and y coordinates for point 1 separated by a space: ";
  cin >> x1 >> y1;

  cout << "Enter the x and y coordinates for point 2 separated by a space: ";
  cin >> x2 >> y2;

  // calculate the slope
  m = (y2 - y1) / (x2 - x1);

  // print out the results
  cout << "The slope is " << m << endl;

  return 0;
}
```

**Example 2:** Ideal Gas Law

Using the technique above, we start with:

```text
( ) / ( )
```

Adding in the specifics we get:

```cpp
P = (n * R * T)/(V);
```

Here is a program to implement this formula:

```cpp
#include <iostream>
using namespace std;

int main()
{
  // Variables
  double n, R, T, V, P;

  // Input values
  cout << "Enter the number of moles of gas (n): ";
  cin >> n;
  cout << "Enter the ideal gas constant (R): ";
  cin >> R;
  cout << "Enter the temperature (T in Kelvin): ";
  cin >> T;
  cout << "Enter the volume (V in liters): ";
  cin >> V;

  // Calculate pressure
  P = (n * R * T) / (V);

  // Output the result
  cout << "The pressure of the gas is: " << P << " atm" << endl;

  return 0;
}
```

When a program has a formula, you must make sure that the results given are correct. To do this, you need to work out some test cases by hand.

1. Write a program that declares variables of all the basic data types (int, float, double, char, bool) and assigns them values. Then print the values to the screen using cout.
2. Write a program that performs arithmetic between an int and a float. Print the result and observe the type conversion. Then, use static\_cast to explicitly cast the float to an int before the operation and observe the difference.
3. Write a program that calculates the area of a circle using a const double PI = 3.14159. Ask the user for the radius, then compute and print the area.
4. Create a program that asks the user for their name, age, and favorite number. Then, display a personalized greeting incorporating all the input.
5. Write a program that asks the user to input three integers. Calculate their average and print the result.
6. Write a program that takes two numbers as input and performs addition, subtraction, multiplication, division, and modulus operations. Print the results.

---

[← Previous: Chapter 1](01-introduction-to-cpp.md) | [Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Chapter 3 →](03-control-structures.md)
