<!-- nav -->
[Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Chapter 12 →](12-exception-handling.md)

# Chapter 11 - Using Files

Up until now, all of the information used by our programs has been hardcoded into the program, or typed in by the user. If they run the program again, they need to enter all the data again. This is clearly not efficient. In this chapter, we will cover how to input information stored in files, and the create new files with our program created data.

## Stream Model

In C++, file input and output is implemented as streams. We have already seen streams in our use of cin and cout. While using cout seems less intuitive than using print in Python, that extra work is going to pay off now. We interact with files using the same techniques we use for standard input and output.

Here is the general model:

```cpp
data input source (keyboard or file) << program
program >> data output (screen or file)
```

### Example using Standard I/O

```cpp
int age;
cout << "Enter your age: ";
cin >> age;
```

We will use this same mental model to read and write to files.

## Libraries

The tools we need for file manipulation are found in the \<fstream> library, so we must include it in any program that accesses files.

The \<fsteam> library has the following streams we can use (similar to cin and cout)

- ifstream - input stream. Used for reading information from files
- ofstream - output stream. Used for writing information to files
- fstream - bidirectional stream. Can be used for both reading and writing

### Inheritance Hierarchy for C++ Streams

```cpp
ios
 └── istream  ←── cin
 │    └── ifstream
 └── ostream  ←── cout
 │    └── ofstream
 └── iostream
      └── fstream
```

## Opening Files for Input and Output

### Input

We can create an instance of the **ifstream** operator and open a file in one step using the constructor.

```cpp
ifstream infile("data.txt")
```

We now have an **ifstream** object called **infile**, and it points to the open  **data.txt** file in the current directory. **infile** is also referred to as a file pointer. We use **infile** as the name by tradition, but any valid name will work.

You should always verify that the file opened successfully before reading from it. The following code will accomplish this:

```cpp
ifstream inFile("data.txt");
if (!inFile)
{
    cout << "Error opening file." << endl;
    return 1;      // flag that something went wrong
}
```

### Output

We proceed in a similar way to open a file for output, this time using the **ofstream** class**.**

```cpp
ofstream outFile("output.txt");  // creates or overwrites
if (!outFile)
{
    cout << "Error opening file." << endl;
    return 1;
}
```

We have options when opening a file for output. By default, we just overwrite whatever was in the file before. To open a file for appending, adding on to the end of the file, use the **`ios::app`** flag as follows:

```cpp
ofstream outFile("output.txt", ios:: app);
```

### Closing our file

Whether using an input or output file, you need to close it when done. Use the **.close()** function.

## Reading from Files

Once you have an input file pointer, you can start reading from the file. You have options on how you go about doing this.

| **Function** | **Unit Read** | **Whitespaces** | **Best For** |
| --- | --- | --- | --- |
| >> | Token - stops on whitespace | Skipped | Numbers, single words |
| getline() | Full line | Preserved | Text lines, CSV, names with spaces |
| get() | One character | Preserved | Character-level processing |
| peek() | One character (no consume) | Preserved | Lookahead, conditional reading |

### Using the >>

```cpp
ifstream inFile("data.txt");
int x;
string word;

inFile >> x;        // reads one integer
inFile >> word;     // reads one word (stops at whitespace)
```

\>> skips whitespaces, so a name like Billy Bob would be read as two separate tokens.

### Using getline()

```cpp
string line;
while (getline(inFile, line))
{
    cout << line << endl;
}
```

This example will read and print out the entire file.

## Common Errors with files

### Using get() and peek()

get() reads a character and moves the file pointer. peek() gets a character and leaves the file pointer where it is.

```cpp
char ch;
while (inFile.get(ch)) {   // reads one character including whitespace
    cout << ch;
}

if (inFile.peek() == '\n') {
    // next character is a newline — act accordingly
}
```

### Using >> and getline() together

When `>>` reads a value it leaves the newline character sitting in the buffer. A subsequent `getline()` immediately consumes that leftover newline and returns an empty string:

```cpp
int age;
string name;

inFile >> age;          // reads the number, leaves '\n' in buffer
getline(inFile, name);  // immediately hits '\n' — reads empty string!
```

The fix is to consume the leftover newline using **.ignore()**  before calling `getline()`:

```cpp
inFile >> age;
inFile.ignore();           // discards one character (the '\n')
getline(inFile, name);     // now works correctly
```

### Incorrect Loop patterns

```cpp
while (inFile >> x) {           // loop exits cleanly when read fails
    // process x
}

while (getline(inFile, line)) { // same idea for lines
    // process line
}
```

Problematic pattern. Checking eof() explicitly:

```cpp
while (!inFile.eof()) {      // classic bug — processes last item twice
    inFile >> x;
    // process x
}
```

## Final Example

We will close this chapter with a solution to a common problem, transferring information from one file to another.

```cpp
ifstream inFile("students.txt");
ofstream outFile("results.txt");

if (!inFile || !outFile) {   // tests that both files opened correctly
    cout << "Error opening one or more files." << endl;
    return 1;
}

string name;
double gpa;
while (inFile >> name >> gpa) {  // gets the name and gpa tokens
    if (gpa >= 3.5) {
        outFile << name << " made the honor roll." << endl;
    }
}

inFile.close();
outFile.close();
```

Note in this example the name cannot contain any spaces.

---

[← Previous: Chapter 10](10-pointers.md) | [Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Chapter 12 →](12-exception-handling.md)
