<!-- nav -->
[Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Appendix A →](appendix-a-connecting-to-the-server.md)

# Chapter 12 - Exception Handling

## Motivation

A well written program should be able to handle any input a user throws at it without cracking. It is never acceptable for a user to see a **segmentation fault** or other error. You may leave the user unable to save information or to be unable to restart the program. In addition to being unprofessional, having a program break unexpectedly could lead to security vulnerabilities.

An **exception** is an unexpected condition that occurs during the execution of a program that disrupts the normal execution of the program and cannot be handled at the point where it occurs. If it is an error that users are likely to make, such as entering letters when a number is expected, it should be handled by input validation after input. An exception is of a larger scope. Examples include file system errors, such as File not Found and Math errors such as Division by Zero.

## The Try-Catch Block

A **try** block is placed around any code where an exception could occur. A **catch** block follows. It is responsible for dealing with the exceptions that occur only in the connected try block. The catch block will allow your program to deal with the exception in a controlled way.

### Throwing errors

When an error is detected, usually using a conditional statement, you can throw an error which will be caught by the catch block. The throw allows you to send a value that can be used in catch to determine exactly how to respond. You can throw primitives (such an int), standard exceptions or custom exceptions.

Throwing a primitive is rarely very informative. It can be used to signal a specific error, but is not done much in practice. Here is a simple example:

```cpp
int divide (int x, int y)
{
    if (y == 0)
    {
        throw -1;
    }
    return x/y;
}

int main()
{
    try
    {
        int z = divide(35,0);
    }
    catch (int errorCode)
    {
        cout << "Error code " << errorCode << endl;
    }
    return 0;
}
```

In this example, the program would just print out an error code of -1, which would not really mean anything to the user.

### Using Standard Exceptions

Here are the Standard Errors defined by c++ in std::exception:

```text
std::exception
├── std::logic_error
│ ├── std::invalid_argument
│ ├── std::domain_error
│ ├── std::length_error
│ └── std::out_of_range
├── std::runtime_error
│ ├── std::range_error
│ ├── std::overflow_error
│ └── std::underflow_error
├── std::bad_alloc
├── std::bad_cast
└── std::bad_exception
```

These standard exceptions are thrown by various operations. For example, the vector at() function will throw an out\_of\_range exception. Here is an example of an uncaught out of range error.

```cpp
int main() {
    vector<int> numbers = {10, 20, 30};

    cout << "Accessing element..." << endl;
    cout << numbers.at(10) << endl;       // index 10 doesn't exist
    cout << "Program continues..." << endl;

    return 0;
}
```

This program will compile with no issues, but when you run it, you will get the following error:

```text
Accessing element...
terminate called after throwing an instance of 'std::out_of_range'
  what():  vector::_M_range_check: __n (which is 10) >= this->size() (which is 3)
Aborted (core dumped)
```

This is exactly what you don't want to happen. The only upside is that we know exactly what exception was thrown, so we can catch it to deal with it more gracefully. Here is the corrected code:

```cpp
int main() {
    vector<int> numbers = {10, 20, 30};

    try {
        cout << "Accessing element..." << endl;
        cout << numbers.at(10) << endl;       // index 10 doesn't exist
        cout << "Program continues..." << endl;
    }
    catch (out_of_range& e) {
        cout << "Error: " << e.what() << endl;
        cout << "Valid indexes are 0 through " << numbers.size() - 1 << endl;
    }

    cout << "Program ends gracefully." << endl;

    return 0;
}
```

This provides a much nicer user experience. Here is the output:

```text
Accessing element...
Error: vector::_M_range_check: __n (which is 10) >= this->size() (which is 3)
Valid indexes are 0 through 2
Program ends gracefully.
```

All of the Error: line is produced by the e.what() function. It provides the user much more readable and useful information. Note that throwing and catching an error does not return from a function. The program does not terminate. You could ask the user for more input, for example, and try again.

Here is a larger example that demonstrates a real life use of Exception Handling. In this example, we are going to ask the user for a filename, and then display the file contents. We will open the file in a function to separate that process from main().

```cpp
#include <iostream>
#include <fstream>
#include <stdexcept>
using namespace std;

ifstream openFile(string filename) {
    ifstream inFile(filename);
    if (!inFile)
        throw runtime_error("File not found: " + filename);
    return inFile;
}

int main() {
    string filename;
    ifstream inFile;
    bool fileOpened = false;

    while (!fileOpened) {
        cout << "Enter filename: ";
        cin >> filename;

        try {
            inFile = openFile(filename);
            fileOpened = true;              // only reached if no throw
            cout << "File opened successfully." << endl;
        }
        catch (runtime_error& e) {
            cout << "Error: " << e.what() << endl;
            cout << "Please try again." << endl;
        }
    }

    // read and display the file contents
    string line;
    while (getline(inFile, line)) {
        cout << line << endl;
    }
    inFile.close();

    return 0;
}
```

Notes:

- We need to include \<stdexcept> to have access to the standard exceptions.
- The throw is done in the function if the file does not exist, i.e., if !infile is true.
- The function is called inside the try block in main. If an exception is thrown, program control moves to the catch block. The fileOpen - true will not execute if there is an exception.
- Once inside the catch block. e.what is used to print out an error message.
- Control now flows back to the while loop. If there was an exception, the value fileOpened is still false, so we ask the user again.

Here is the out of running the program with a file that does not exist, followed by one that does.

```text
Enter filename: Test.txt
Error: File not found: Test.txt
Please try again.
Enter filename: test.txt
File opened successfully.
This is a test file.
```

- **Catching by reference** — why `catch(const std::exception& e)` is preferred
- **Exception safety** — basic vs. strong vs. no-throw guarantees (conceptual overview)
- **Stack unwinding** — what happens to local variables when an exception propagates
- **When NOT to use exceptions** — exceptions vs. error codes, performance considerations, expected vs. exceptional conditions

---

[← Previous: Chapter 11](11-using-files.md) | [Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Appendix A →](appendix-a-connecting-to-the-server.md)
