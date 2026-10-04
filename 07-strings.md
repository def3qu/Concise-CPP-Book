<!-- nav -->
[Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Chapter 8 →](08-structs-and-classes.md)

# Chapter 7 - Strings

## 7.1 Introduction to Strings

We have seen strings before, used as a data type as in the following code:

```cpp
string name = "Bob";
```

There is more to strings than just being a data type. They are also a linear data structure like the arrays that we just covered. This means we can use them to store and organize data.

In particular, strings in C++ store a sequence of characters, enclosed in double quotes. Inside of the double quotes we can have any letters, numbers, punctuation or spaces that we want.  The word sequence is important. Strings are stored in order. The string "hello" is not the same as the string "olleh".

The strings we are speaking of here refer specifically to what is known as a std::string. C, which was the predecessor of C++, did not have strings and instead used arrays of chars. We will talk more about C-style strings later in this chapter. Unless otherwise specified, string will mean std::string.

### 7.1.1 Differences between strings and chars

C++ has a data type `char` that stores only a single character. Chars are stored in single quotes. Internally, chars are integers that store the numerical ASCII value of a character. For example, the following code:

```cpp
char ch = 'A';
```

would store the number 65, which is the ASCII value of a capital A.

### 7.1.2 Characteristics of Strings

- **Dynamic Size** - Strings can shrink and grow dynamically as needed
- **Character Access** - we can access the individual chars that make up a string using bracket notation

```cpp
string s = "hello";
char c = s[0];    // c contains 'h'
```

- **Mutability** - individual parts of a string can be changed, unlike in other language such as Python and Java. So the following is permissible:

```cpp
string s = "hello";
s[0] = 'H';
```

- **Comparison operators** - we can use >, <, >=, <=, ==, and != with strings. They will be compared in **lexicographical** or dictionary order.  So, "Apple" is less than "Banana" since 'A' evaluates to 65 and 'B' evaluates to 66. Note that there is an order to punctuation marks, but it is not self-evident what that order is. See  ASCII Table in Appendix E..
- **Built-in Copy Constructor** - we can easily copy strings using the assignment operator (=):

```cpp
string name1 = "Bob";
string name2 = name1;
```

- **Can use cin and cout just like any other data type -**

```cpp
string name;
cout << "What is your name? ";
cin >> name;
```

### 7.1.3 Iterating Through a String

Just like with arrays, we can use a **for loop** to iterate through the characters of a string one by one.

To do this, we need to know the length of the string. Unlike arrays, strings know how long they currently are.  We have two options for this, .length() or .size(). They function identically.  Here is an example using the length in a for loop.

```cpp
string major = "Computer Science";
for(int i=0; i<major.size(); i++)
{
    cout << major[i] << endl;
}
```

This code will print out each character of the string on its own line.

There is a more modern style of **for loops** that looks a little more Python like:

```cpp
for (char ch : major)
{
        cout << ch << endl;
}
```

### 7.1.4 String Concatenations

We can make a new string by combining or **concatenating** two or more strings together. For example, we can create a new string from two existing strings:

```cpp
string c = "cats";
string d = "dogs";
string s = c + d;
```

String s would now have the value "catsdogs", so what we probably wanted to do was:

```cpp
string s = c + " " + d;
```

Which gives us "cats dogs".

We can also create a new string from an existing one by adding new characters to either the beginning or end.

```cpp
string name = "Bob";
name = name + " Smith";
```

name would now hold "Bob Smith". Note that we could have written that last line as:

```cpp
name += " Smith";
```

It turns out that using += instead of + is more efficient, as it requires the CPU to make fewer temporary objects.

## 7.2 C-strings

C, from which C++ developed, did not have strings. To represent a sequence of characters, you make an array of chars. C strings are terminated by a null character '\\0'.

There are two ways to declare c-strings in C++.

```cpp
char str1[6] = {'H', 'e', 'l', 'l', 'o', '\0'}; // Null char at end
char str2[] = "Hello";     // Implicit null termination
```

We can use `cin` and `cout` on C strings. Here is an example:

```cpp
char city[26];    // we can store 25 chars, as the \0 takes up 1
cout << "Enter a City: ";
cin >> city;     // goes until it hits a whitespace, so no St. Louis
cout << "Welcome to " << city << endl;
```

In the above example, cin stops at a space. If we want to allow spaces, we need to use a `cin.getline()` instead. Here is the new version:

```cpp
char city[26];    // we can store 25 chars, as the \0 takes up 1                     cout << "Enter a City: ";
cin.getline(city, 26);     // allows whitespace
cout << "Welcome to " << city << endl;
```

C-strings lack any built-in methods, so we have to include the library \<cstring> to get some functionality.

Here are some common \<cstring> functions:

| Function | Purpose |
| --- | --- |
| `strcpy(from, to)` | Copies from one string to another<br>DO NOT USE. It does not limit the size copied, and can result in a Buffer Overflow (see example below) |
| `strncpy(from, to, size)` | Safely copies from one string to another by limiting the size to be copied |
| `strlen(string)` | Returns the length of a string (not including the Null Terminator) |
| `strcat(orig, to add)` | Concatenates a string to another string |
| `strcmp(str1, str2)` | Compares two strings. 0 means equal, 1 means unequal |

Example program with cstring functions:

```cpp
int main()
{
    char name1[10] = "Bob";
    char name2[10];
    strncpy(name2, name1, 10);  // Copy name1 to name2                                cout << "Copied string: " << name2 << endl;
    cout << "Length of name1: " << strlen(name1) << endl;  // String length
    strcat(name1, " Smith");  // Concatenate " Smith" to name1
    cout << "Concatenated string: " << name1 << endl;
    cout << "Comparison (Bob vs Bob): " << strcmp(name2, "Bob") << endl;
    cout << "Comparison (Bob vs Alice): " << strcmp(name2, "Alice") << endl;
    return 0
}
```

A **buffer** is a fixed-size block of memory that is used to hold data. An array is an example. A **buffer overflow** occurs when more data is written to the buffer than it can hold. The extra data gets placed in the memory right after the buffer with unpredictable results.

Buffer overflow example with strcpy:

```cpp
#include <iostream>
#include <cstring>
using namespace std;
int main() {
        char smallBuffer[5];  // Can hold only 4 characters + '\0'
    strcpy(smallBuffer, "Hello");  // Buffer Overflow (5 chars + '\0')
     cout << smallBuffer << endl;
        return 0;
}
```

## 7.3 Common String Operations with std::string

### 7.3.1 Substrings

You can create substrings from an existing string by using the .substr function. This function is appended to a string variable and takes two int parameters. The first parameter is the position to start the substring (where 0 represents the first character.) The second parameter gives the length of the substring to be extracted. Here are some examples:

```cpp
string str1 = "Hello There";
string str2 = str1.substr(0, 5);   // str2 is "Hello"
string str3 = str1.substr(6, 5);   // str3 is "There"
```

### 7.3.2 Searching for Substrings within a String

We can see if a substring is contained in a string using the **.find()** function. This function is appended to a string variable and takes as a parameter, the substring you are looking for. If found, the function returns a **size\_t** that shows the position in the string that the substring starts. A **size\_t** is a special kind of unsigned int (no negative numbers) that is used to represent indices. If not found, a special character, string::npos is returned. string::npos is the largest possible **size\_t** number and is used to represent when a string is not found..

Here are some examples:

```cpp
string str = "apple";
size_t pos = str.find('p');
cout << "First occurrence of 'p' is at index: " << pos << endl;
```

`…`would return: `First occurrence of 'p' is at index: 1`

```cpp
string str = "hello";
size_t pos = str.find("xyz");
if (pos == string::npos)
{
    cout << "Substring not found!" << endl;
}
```

`…`would return: `Substring not found!`

The .find() function can take an optional second parameter that allows you to set the starting point for your search.

Example using the optional second parameter:

```cpp
string str = "banana";
size_t pos = str.find("na", 3); // Starts searching from index 3
```

There is also a .rfind() function that works in the same way as .find(), but starts from the right of the string instead of the left.

Example program using both .find() and .rfind():

```cpp
#include <iostream>
#include <string>
using namespace std;
int main()
{
    string str = "banana";
    // Find the first occurrence of "na"
    size_t first_pos = str.find("na");
    cout << "First occurrence of 'na' is at index: " << first_pos << endl;
        // Find the last occurrence of "na"
    size_t last_pos = str.rfind("na");
    cout << "Last occurrence of 'na' is at index: " << last_pos << endl;
        return 0;
}
```

### 7.3.3 Modifying strings

The following are functions to modify strings:

| Function | Purpose | Example |
| --- | --- | --- |
| .append() | Add to the end of a string | str1.append("Hello") |
| .insert() | Add at a specific point in a string | str1.insert(4, "star") |
| .replace(start, len, " ") | Replace part of a string | str1.replace(5,3,"ball") |
| .erase(start, len) | Deletes part of a string | str1.erase(3, 2) |

### 7.3.4 Conversion between std::string and C-style strings

You can convert a std:string to a c-string using the  **.c\_str()**. Here is an example:

```cpp
string str = "Hello, World!";
const char* c_str = str.c_str();
```

## 7.4 Example Program

Reversing a string:

```cpp
#include <iostream>
#include <string>
using namespace std;
void reverseString(string& str)
{
    int start = 0;
    int end = str.length() - 1;
  // Swap characters from both ends towards the center
     while (start < end)
        {
        // Swap characters
        char temp = str[start];
        str[start] = str[end];
        str[end] = temp;
      // Move the indices towards the center
      start++;
    end--;
    }
}
int main()
{
  // Input string
    string str;
    cout << "Enter a string: ";
    getline(cin, str);
  // Reverse the string
    reverseString(str);
// Output the reversed string
    cout << "Reversed string: " << str << endl;

  return 0;
}
```

---

[← Previous: Chapter 6](06-arrays.md) | [Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Chapter 8 →](08-structs-and-classes.md)
