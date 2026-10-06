<!-- nav -->
[Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Chapter 7 →](07-strings.md)

# Chapter 6 - Arrays

## 6.1 Introduction to Arrays

### 6.1.1 Definition of an Array

An array is a linear data structure. It allows you to store, and easily access, multiple items of the same data type. They have to be initialized to one type and to a size. It is similar, in some ways, to lists in Python. The biggest difference is the need for all items to be of the same type, and the inability of arrays to grow. We will cover **vectors** (part of the Standard Template Library) later. They are the closest equivalent to Python lists.

### 6.1.2 Why use arrays?

Arrays allow us to create a series of variables for easy access. For example, let’s say we are creating a program to store temperature data from a sensor. With what we know so far, we would have to initialize a variable for each reading.

```text
int temp1, temp2, temp3, temp4, temp5, ...
```

This quickly gets laborious. If we want to access each temp, we have to address it individually.

```cpp
cout << temp1 << " " << temp2 << " " << temp3 ..
```

If we store the temps in an array, we only need to initialize one. We can also easily loop through the items.

## 6.2 Initializing Arrays

Arrays have to be initialized before they can be used. In most cases, they need to have some default value set to avoid unpredictable data in them.

### 6.2.1 Declaring arrays

To declare an array, you need to specify the type of data the array will hold, and how many items can be stored. The line below also sets each element in the array to 0.

```cpp
int arr[5] = {0};
```

When this statement is executed, the OS will reserve enough contiguous memory to hold five integers. The name arr is really just the starting memory address. If we do the following:

```cpp
cout << arr << endl;
```

…we will get a memory address such as *0x7ffc852d5430*.

When you declare an array without giving it values, the elements contain whatever happened to be in that memory. The values are unpredictable, so if you want them to be something in particular, you need to set the initial values explicitly.

### 6.2.2 Initializing arrays

We can add values to our array when initialized.

```cpp
int arr[5] = {1, 2, 3, 4, 5};
```

We can also do a partial initialization:

```cpp
int a[5] = {1, 2} // others default to 0
```

## 6.3 Accessing and Modifying Array Elements

### 6.3.1 Using Index Notation

We can access an element of an array by using its index number. The index number is the position of the element in the array. Index numbers start at 0, which seems an odd choice. The reason lies in the fact that the array name points to a memory address.

The index refers to the offset of the element within the contiguous block of memory where the array is stored. The first element is stored at that memory address, so we don’t need to add anything. Hence, the first element in the array `arr` initialized above would be accessed using `arr[0]`.

To get to the second element of `arr`, we would use `arr[1]`. This tells the computer to go to the starting memory address of `arr`, and then to move over the size of an int, which for us is 4 Bytes or 32 Bits. So, if the memory address of our array is *0x7ffc650e04f0*, the memory address of the second element is *0x7ffc650e04f4*, which is 4 more. We can address each of the elements in the array in this fashion.

### 6.3.2 Input and Output of array elements

It is important to remember that the element of an array is just a variable of the type of the array. If we use the typeid function, arr is reported to be an array of ints, while arr\[1\] is reported to be an int. So, we can use arr\[1\] wherever we would use an int. We could ask the user to input a value to be stored in an array just as we would with an int.

```cpp
cout << "Enter an Integer: ";
cin >> arr[1];
```

We can also output the value of an array in a similar fashion:

```cpp
cout << "You entered " << arr[1];
```

### 6.3.3 Common Errors - out of bounds

An important difference from Python is that C++ does not enforce array limits. In a Python List with three elements, an attempt to access a fourth element will result in a “List Index Out of Bounds” error. C++ has no such guardrails. It is up to the programmer to make sure they do not access items that are out of bounds.

In the array `arr` defined above, if we attempt to print out `arr[5]`, it will not trigger a warning. It will instead print out whatever is located in the memory next to where the array ends. Let’s take a look at the memory layout of `arr`:

arr\[0\]         arr\[1\]        arr\[2\]         arr\[3\]       arr\[4\]

|  |  | 0 | 0 | 0 | 0 | 0 |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

Displaying `arr[0]` through `arr[4]` will print out the 0 we expect. Displaying `arr[5]` will display whatever is randomly stored in the memory next to where arr happens to be stored.

```cpp
int arr[5] = {0};  // set all array values to 0
cout << arr[0] << endl;
cout << arr[4] << endl;
cout << arr[5] << endl;
```

Output:

```text
0
0
-915409408
```

## 6.4 Iterating through Arrays

### 6.4.1 Using loops

One of the biggest advantages of using arrays is that we can easily iterate through them with a for loop. We can do this when entering the array, when displaying the array, or when changing the elements of the array.

Allow user to enter the elements of the array:

```cpp
int arr[5];
for (int i=0; i<5; i++)
{
    cout << "Enter an Integer: ";
    cin >> arr[i];
}
```

Display elements of the array `arr:`

```cpp
for (int i=0; i<5; i++)
{
    cout << cin arr[i] << '\t';
}
cout << endl;
```

Let's say we want to double each of the elements the user enters. Here is the complete program to initialize an array, ask the user to enter some values, double each value, then print out the array:

```cpp
#include <iostream>
using namespace std;

int main()
{
  int arr[5];
  // have the users input elements
  for (int i=0; i<5; i++)
    {
      cout << "Enter an Integer: ";
      cin >> arr[i];
    }
  // double the elements one by one
  for (int i=0; i<5; i++)
    {
      arr[i]=arr[i]*2;
    }
  // Display the elements tab separated
  for (int i=0; i<5; i++)
    {
      cout << arr[i] << '\t';;
    }
  cout << endl;

  return 0;
}
```

### 6.4.2 Tracking the Size of the Array or Using sizeof()

It is standard programming practice in C++ to create a constant variable to hold the size of any array. We can use this to avoid the out-of-bounds condition described above. Here is an example:

```cpp
const int arrSIZE=5;
int arr[arrSIZE];

for (int i=0; i<arrSIZE; i++)
    {
    cout << arr[i] << '\t';;
    }
cout << endl;
```

Note the naming convention used for the size of the array. If you have a way to consistently name things, you never have to work to remember what things are called. In this case, I am using the name of the array followed by SIZE. SIZE is in all caps since it is a constant. Using this method, if I need to know the size of an array called grades, I will know it will be GRADES\_SIZE.

We can also use the **sizeof()** function to determine the array size on the fly. This is not the preferred way to do things, but can be used in a pinch. This can ONLY be done where the array has not been passed into a function as a parameter. The sizeof() function returns how much space something takes up. In the above example, the sizeof(arr) is 20. Each int is 4 Bytes and there are 5 of them. The sizeof(arr\[0\]) returns the size of an individual element in arr. Since arr is composed of ints, the size of each element will be 4 Bytes. If we divide the size of arr by the size of arr\[0\], we get 5, which is the size of the array. We could use it as follows:

```cpp
int arr[5];

for (int i=0; i<sizeof(arr)/sizeof(arr[0]); i++)
    {
    cout << arr[i] << '\t';;
    }
cout << endl;
```

### 6.4.3 Traversing in Reverse Order

By adjusting the for loop, we can print out arrays in reverse order. To do so, we start our loop at the last element, which will be the size of the array minus 1. We then count backwards until we reach 0, which is the first element.

```cpp
const int arrSIZE=5;
int arr[5]={1, 2, 3, 4, 5};

for (int i=arrSIZE-1; i>=0; i--)
    {
    cout << i << '\t' << arr[i] << '\t' << endl;
    }
cout << endl;
```

Note: the index i was added to the cout to make sure that it is working correctly.

Note: the '\\t' prints out a horizontal tab.

## 6.5 Multidimensional Arrays

### 6.5.1 Declaring and Initializing 2D Arrays

As we have seen, arrays can hold any data type. Arrays can even hold other arrays. This is the idea behind the somewhat strange syntax of 2-dimensional arrays in C++. Let's say we want to create a 3 x 3 array that contains the numbers 1-9 in order. First, we will create arrays for each row:

```text
row1 = [ 1, 2, 3]
row2 = [ 4, 5, 6]
row3 = [ 7, 8, 9]
```

So conceptually, our 2D array will be an array of these three arrays or \[row1, row2, row3\]. That is not quite the syntax we use. Instead, we will create our array this way.

```cpp
int ar[3][3] = {{1, 2, 3},
                 {4, 5, 6},
                 {7, 8, 9} };
```

Note that we have to specify the dimension of our array, in this case a 3 x 3. The spacing here is not required. It could be on one line such as:

```cpp
int ar[3][3] = {{1, 2, 3}, {4, 5, 6},{7, 8, 9} };
```

but most use the first version, as it gives a better idea of what you are creating.

Unlike Python, we cannot have jagged arrays. Each row in our 2D array must have the same number of elements.

### 6.5.2 Accessing Elements in 2D Arrays

We access items in our 2D array by specifying the row, then column that we want, each enclosed in their own square brackets. As with all arrays, we start counting with 0. So, to access the 1 in the ar array defined above, we would type ar\[0\]\[0\].

So, for this array, the valid values for both Row and Column are 0, 1 and 2. As always with arrays in C++, if we try to access a number larger than this, we don't get an error. C++ will just return whatever is randomly located in the memory of the address that we specify. Therefore, just as before, we need to keep track of the size. In this case, it is common practice to track constant integers for both Row and Columns. Here we redo our array from above using constants:

```cpp
int ROWS = 3;
int COLS = 3;

int ar[ROWS][COLS] = {{1, 2, 3},
                       {4, 5, 6},
                       {7, 8, 9} };
```

It is important to keep in mind that **ar** is a 2D array. **ar\[0\]** is incorrectly treating it as a 1D array.

### 6.5.3 Nested Loops for Traversing 2D Arrays

In order to traverse a 2D array, we are going to need nested **for** loops. The outer **for** loop will iterate through each **row**, while the inner for loop will go through each item in that row. For an example, let's create a new array that is 3 x 4 and then print out each item.

```cpp
int ROWS = 3;
int COLS = 4;

int ar2[ROWS][COLS] = {{1, 2, 3, 4},
                        {5, 6, 7, 8},
                        {9, 10, 11, 12} };
```

The outer for loop will be:

```cpp
for(int r = 0; r < ROWS; r++)
```

The inner loop will be:

```cpp
for(int c = 0; c < COLS; c++)
```

Putting this all together we get the following:

```cpp
for(int r = 0; r < ROWS; r++)
{
    for(int c = 0; c < COLS; c++)
    {
        cout << ar2[r][c] << endl;
    }
}
```

Note that this prints the entire first row before moving to the second. So the inner loop iterates 3 times for every 1 time of the outer loop.

We don't always want to initialize the array up front. Here is an example where we ask the user to enter values in a 2 x 3 array of doubles. We start by declaring the array, but not adding any values:

```cpp
int ROWS = 2;
int COLS = 3;
int temps[ROWS][COLS] = {0};
```

The ={0} will set all values of the array to 0.

Now we can go through and ask the user to enter each of the 6 values.

```cpp
for(int r=0; r<ROWS; r++)
    {
    for(int c=0; c<COLS; c++)
            {
                cout << "Enter next value: ";
            cin >> temps[r][c];
            }
    }
```

Now let's print out those new values:

```cpp
for(int r=0; r<ROWS; r++)
    {
    for(int c=0; c<COLS; c++)
            {
                cout << temps[r][c] << " ";
            }
    cout << endl;
    }
```

In this print, there will be a space between the values in each row, and a new line only at the end of the row.

## 6.6 Passing Arrays to Functions

### 6.6.1 Passing an Array to a Function

Arrays can be very large, so copying one every time we call a function would be wasteful. C++ avoids this. The array name is really the memory address of the first element, so when we pass the array, only that address is copied. The function then works on the original elements, which has the same effect as passing by reference, meaning that changes made in the function remain in the calling function. No & is needed.

We can test this by printing the array name without any indices:

```cpp
const int SIZE=3;
int ar[SIZE] = {1, 2, 3};
cout << ar << endl;
```

`…`will return something like:

```text
0x7ffd374e4b1c
```

Let's write a function that takes an array and its size and prints it out.

```cpp
void printArray(int arr[], int size)
{
    for (int i=0; i<size; i++)
    {
        cout << arr[i] << " ";
    }
cout << endl;
}
```

### 6.6.2 Changing an Array in a Function

Since arrays are passed by reference (at least they act this way) to functions in C++, we are free to make modifications to the items in the array. Here is an example of a function that doubles each element in the array sent in as a parameter:

```cpp
void doubleArray(int arr[], int size)
{
  for (int i=0; i<size; i++)
    {
      arr[i] = arr[i]*2;
    }
  cout << endl;
}
```

### 6.6.3 Using const to Prevent Modifications

We may not want to allow changes to our array. To avoid this, we have to add the const keyword to any parameter we don't want to change. Here is the printArray function with this modification:

```cpp
void printArray(const int arr[], int size)
    {
        for (int i=0; i<size; i++)
        {
            cout << arr[i] << " ";
        }
    cout << endl;
    }
```

Now, if the function tries to modify any element of the array, the program will not compile, and an "assignment of read-only location" error will occur.

## 6.7 Common Operations on Arrays

There are common operations we perform on arrays. This section will give some examples.

### 6.7.1 Finding the Largest/Smallest Element

We often want to know the largest or smallest element in an array. This example will cover the largest, but it should be easy enough for the reader to change it to the smallest. This example works for 1D arrays, but it could be modified for more dimensions if needed.

The array will be passed in as a constant, as there is no reason for this function to change any elements of the array.

We will use what I call the "King of the hill" algorithm, named for the childhood game, not the animated TV series. In this algorithm, we set the **`largest`** variable to the first element of the array. We then check each element of the array against the first, and update **`largest`** if needed.

```cpp
int largestElement(const int arr[], int size)
{
    int largest = arr[0]; // set largest to first item in array
    for (int i=1; i<size;i++)
    {
        if (arr[i]>largest)
        {
            largest = arr[i];
        }
    }
    return largest;
}
```

### 6.7.2 Calculating Sum and Average

Knowing the sum or average of an array of numbers is often needed in programs. We will create the sumArray function below. The average function can be created by the reader without too much further effort.

Again, the array will be passed in as a constant to prevent unwanted changes. We will return the sum. This program assumes the array is of doubles.

```cpp
double sumArray(const double arr[], int size)
{
    double sum=0; //    set initial value of sum to 0
    for (int i=0;   i<size;i++)
        {
        sum = sum + arr[i];
        }
  return sum;
}
```

### 6.7.3 Searching for an Element

Our last example will be a function that searches through an array to see if an element is located there. As before, we will be dealing with a 1-dimensional array, though it could be easily extended to multiple dimensions. We have options here: we could make a function that returns the index of the first instance of the search term. If the item is not found, we usually return a value such as -99. Or, as we will do here, we could make a Boolean function that returns true if the item is found and false otherwise.

We will again use a single-dimension array for the example. Traditionally the variable k is used for search terms, and we will follow that practice.

```cpp
bool search(int k, const int arr[], int size)
{
  for(int i=0; i<size; i++)
    {
     (arr[i] == k)
        {
          return true;
        }
    }
  return false;
}
```

We can test this function with the rather crude code below:

```cpp
const int SIZE=3;
int ar[SIZE] = {13, 22, 3};

int n=13;

if (search(n, ar, SIZE))
  {
    cout << n << " Found" << endl;
  }
else
  {
    cout << n << " Not Found" << endl;
  }

n=33;
if (search(n, ar, SIZE))
  {
    cout << n << " Found" << endl;
  }
else
  {
    cout << n << " Not Found" << endl;
  }
```

Which correctly prints that 13 is found while 33 is not.

---

[← Previous: Chapter 5](05-functions.md) | [Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Chapter 7 →](07-strings.md)
