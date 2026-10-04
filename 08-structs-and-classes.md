# Chapter 8 - Structs and Classes

## 8.1 Introduction to Object-Oriented Programming (OOP)

### 8.1.1 What is OOP?

Object-Oriented Programming (OOP) is a programming paradigm that organizes programs into objects that can be used to represent real life objects. Each object will combine the **data(attributes)** that an instance of the object needs to remember and the **behavior(methods)** which are what an instance of the object can do with the data it holds. For example, we can create a Car class. Attributes could be make, model. color. mpg. Methods could be move(), turn() or stop().

### 8.1.2  Key OOP Principles

- **Encapsulation:** Grouping related data and functions together
- **Abstraction:** Hiding implementation details, exposing only necessary features
- **Inheritance:** The ability to create new classes from existing ones
- **Polymorphism:** Allowing different classes to use the same interface

### 8.1.3 Comparing OOP in Python vs. C++

We saw classes in Python. One of the major differences in C++, is that all attributes and methods need to be explicitly typed rather than in Python where they are dynamic. In C++ we also need to manage the memory in our programs. Also, Python classes are a bit more flexible.

## 8.2 Structs in C++

We will start our discussion of OOP in C++ with **structs**. While in reality structs can function exactly like classes, the traditional way to use them is as classes with only attributes. So **structs** are a way to collect data together.

### 8.2.1 Creating a Struct

It may help to start with an example. Let's say we are going to create a gradebook program, and we need to track some data about students. We need to track the name, class, and gpa for each student. To avoid many, many variables that we can't iterate through, we will create a struct to hold the data

```cpp
struct Student
{
    string name;
    string gradeLevel;
    double gpa;
};
```

**Creating an instance of Student**

We can create students in the following way. Let's say we want a student Bob who is a Sophomore with a gpa of 2.8 and Alice who is a Junior with a gpa of 3.6. The struct name Student is really a custom data type, so we will use it as we would a data type.

```cpp
Student s1 = {"Bob", "Sophomore", 2.8};
Student s2 = {"Alice", "Junior", 3.6};
```

**Accessing Struct Members**  
To access the member data of an instance of Student, we use dot notation. The name of the instance goes before the . and the attribute name goes after. So, to print out the student names we could do the following:

```cpp
cout << s1.name << " and " << s2.name << " are in class" << endl;
```

We could print out all we know about a student as follows:

```cpp
cout << "\tName: " << s2.name << endl;
cout << "\tClass: " << s2.gradeLevel << endl;
cout << "\tGPA: " << s2.gpa << endl;
```

We can change the value of any objects attributes at any time as follows:

```cpp
s1.gpa = 2.9;
```

**Passing Structs to Functions**

By default, Structs are passed to functions by value, which means changes made in the function are not reflected when we return to the calling function. You can also pass structs by reference by appending a & to the struct name in the function signature. Passing by reference is preferred when dealing with a large struct, as it may take up a lot of memory that would need to be duplicated with pass by value. Remember, to add the **const** keyword if the function should not modify the attributes of the struct.

Here is an example program that creates a struct to represent a geometric point:

```cpp
#include <iostream>
using namespace std;
// Define a struct to represent a 2D point
struct Point
{
    int x;
    int y;
};
// Function to display the coordinates of the point
void displayPoint(const Point& p)
{
    cout << "Point coordinates: (" << p.x << ", " << p.y << ")\n";
}
// Function to move the point by a given amount
void movePoint(Point& p, int dx, int dy)
{
    p.x += dx;
    p.y += dy;
}
int main()
{
    // Create a point and initialize its coordinates
    Point p1 = {3, 4};
        // Display the original point
    displayPoint(p1);
        // Move the point by (2, 3)
    movePoint(p1, 2, 3);
        // Display the updated point
    displayPoint(p1);
        return 0;
}
```

## 3. Classes in C++

### 3.1 Difference Between Structs and Classes

In theory, the only real difference between structs and classes is that structs default to public access while classes default to private access. Public here means that we can access the information outside of the struct or class. So in the above example, when we cout p.x, we can do this because x has the default public access. We will talk about public and private more below.

In practice, we create structs with data only, no methods. This comes in part from structs in C, which cannot have methods. We will stay with this practice.

### 3.2 Introduction to Classes

Classes allow us to combine data the same as structs. In addition, we are able to add methods to manipulate the data. This allows for the encapsulation of OOP where classes combine the data and the operations that act on that data.

One of the other goals in OOP is information hiding, where we use methods to control how the attributes are used. Let's say we have a class that represents a Savings account. This class has an attribute for the Interest Rate. We would not want this Interest Rate to be changed beyond certain bounds, so we can restrict any code not in the class definition from changing this rate. We may also not even want this rate to be displayed. We do this by defining the Interest Rate variable as private. Since classes are Private by default, there will need to be some Public functions. Therefore, we need both Public and Private sections of our class definition.

If we have a private attribute, then no outside code can interact with it. To solve this, we usually create functions called Accessors and Mutators, aka getters and setters. These functions, which are defined inside the class, allow viewing (Accessors) and changing under controlled conditions (Mutators). Unless there is a compelling reason, all attributes should be made Private. You can then decide what Accessors or Mutators are needed.

Accessors follow a formula. They are named getAttributeName(). The return type will be the type of the attribute being accessed. There are no parameters. The only statement in the function will be to return the attribute. So, if we had an int attribute called age, our accessor would be:

```cpp
int getAge()
{
    return age;
}
```

Mutators also follow a formula. They are named setAttributeName(). The return type is always void, and they take a single parameter of the same type as the attribute. At the least, the mutator function will have a single line that sets the attribute to the parameter value. More often, there will be some logic to make sure that the value being passed is of a valid type. Here is what the mutator for age could look like:

```cpp
void setAge(int a)
{
    if (a > 0 && a<120)
    {
        age = a;
    }
    // no change if age is not in range. you could give a message
}
```

We now can define a whole class. Typically, we will save class definitions in their own file, and then include that file in any program that needs to use them. Here is an example of a class definition for a Car class:

```cpp
class Car
{
private:
    string make;
    int year;

public:
    // Accessors
    string getMake()
    {
        return make;
    }
    int getYear()
    {
        return year;
    }

    // Mutators
    void setMake(string m)
    {
        make = m;
    }

    void setYear(int y)
    {
        if (y > 1900 && y < 2026)
        {
            year = y;
        }
    }
};
```

### Creating and Using Classes

To use a class, we have to make an instance of the class. We call these objects. We can think of Classes as Cookie Cutters and Objects as Cookies. Just as you can't eat a cookie cutter, you can't directly use a Class. You have to instantiate an object of the Class type. Let's create and instance of our Car class above:

```cpp
Car myCar;
```

We can now use the accessors and mutators on myCar, not on Car.

```cpp
myCar.setMake("Ford");
myCar.setYear(1999);
```

To actually test this, we would need to create a file and include the Car.cpp. Here is a carTest.cpp program that includes and tests our class.

```cpp
#include<iostream>
#include "Car.cpp"

using namespace std;

int main()
{
  Car myCar;
  myCar.setMake("Ford");
  myCar.setYear(1999);
  cout << myCar.getYear() << "  " << myCar.getMake() << endl;
  return 0;
}
```

## 4. Constructors and Destructors

Constructors and Destructors are special functions that are called when objects are created and destroyed.

### 4.1 Constructors

Constructors are called when an object is instantiated. Constructors have the same name as the class, and have no return type.

Here is an example of a constructor for the Car class we define above.

```cpp
class Car
{
private:
    string make;
    int year;
public:
    Car(string m, int y)
    {
        make = m;
        year = y;
    }
}
```

To use this constructor, we could type:

```cpp
Car myCar("Ford", 2022);
```

If any of the attributes of your class could have default values, you can make a default constructor. We can overload the constructor to have a version that takes no parameters. The constructor would then add default values to the object. Here is a default constructor for the Car class:

```cpp
public:
Car()
{
    make = "Ford";
    year = 1999;
}
```

Now if you instantiate an object without parameters, you will not get an error and the object will have the default values. So the statement:

```cpp
Car myCar();
```

would create an object called myCar with make="Ford" and year = 1999.

### 4.2 Destructors

Destructors are called when the object is deleted. Their main use is to free up dynamically allocated memory. We have not done that, so for now we will just say that destructors have the same name as the class, but with a \~ prepended. So the destructor for the **Car** class would be **\~Car**. It has no return type, and takes no parameters.

## 5. Access Specifiers: Public, Private, and Protected

We have already seen **public** and **private** used in code examples, but what do they really mean? These are known as Access Specifiers and denote where the attributes and methods of a class can be accessed. There is also a **protected** specifier that we have not seen yet.

- **Public:** Accessible from anywhere. There are no restrictions on use.
- **Private:** Accessible only from methods defined within the class. Not available outside.
- **Protected:** Accessible in derived classes (used in inheritance which we will cover later)

### 5.1 Example of Public and Private

```cpp
class ClassA
{
private:
    int x;
    void displayX()
    {
        cout << x << endl;
    }
public:
    int y;
    void displayY()
    {
        cout << y << endl;
    }
};

int main()
{
    ClassA myClass;
    myClass.x = 5;      // Gives an Error
    myClass.displayX(); // Gives an Error
    myClass.y = 15;
    myClass.displayY();
    return 0;
}
```

When we compile this, we get a "is private in this context" error when we try to directly change x or run the displayX() function. Since y and displayY() are public, they can be accessed from main().

As a general rule, all attributes should be private so you can control how they are updated. Most functions are public, unless there is a reason to restrict them to use only within the classes other attributes.

## 6. Accessors and Mutators to Access Private Attributes

When we place our attributes under the **private** access specifier, there is no way for code in other parts of your program to access them. To allow controlled access to these attributes, we need to add public functions.

### 6.2 Accessors or Getters

To allow access but not modification of private attributes, we create public **accessor** functions, also known as **getters**. These functions don't do anything except return the value of the attribute. This seems like a trivial thing, but because the attributes are private, we need a public function to allow other code to see them.

Let's do an example. Assume we have a Point class that has `private` int attributes for the **x** and **y** coordinates of a point on the Cartesian plane. As we saw above, if main() or any other function tries to directly print either **x** or **y**, we will get an error. So we need to create accessor functions for both **x** and **y**. By programmer tradition (which should be honored) we name these Accessor functions **getX()** and **getY()**. The return type for getters is the same as the type of the attribute, in this case both are `int`. Getters take no parameters. Here are **getX()** and **getY()**.

```cpp
int getX()
{
    return x;
}
int getY()
{
    return y;
}
```

All Accessor functions will look like this.

### 6.3 Mutators or Setters

If you want to allow the private attributes to be changed (following rules you define) add some public **mutator** functions also known as **setters**. Setters are `void` functions that take a parameter of the type of the attribute they are attached to. The functions check to see if the parameter value is acceptable, and if so, make the change to the attribute.

Let's return to the Point class above, with private attributes **x** and **y**. Assume that in this particular program. we need to restrict both **x** and **y** to values between -20 and 20. Our **setter** functions will need to enforce this. These programs should be named **setX()** and **setY()**. Both functions will be void, and will take an int as a parameter. Here are the functions.

```cpp
void setX(int a)
{
    if(a>=-20 and a<=20)
    {
        x = a;
    }
    else
    {
        cout << "Point coordinates must be between -20 and 20" << endl;
    }
}

void setY(int a)
{
    if(a>=-20 and a<=20)
    {
        y = a;
    }
    else
    {
        cout << "Point coordinates must be between -20 and 20" << endl;
    }
}
```

We can now modify **x** and **y** from anywhere in our code, as long as we follow the rules. Here is a fully class definition of the Point class with a sample use in main()

```cpp
#include <iostream>
using namespace std;

class Point
{
 private:
  int x;
  int y;

 public:
  // Accessors
  int getX()
  {
    return x;
  }

  int getY()
  {
    return y;
  }

  // Mutators
  void setX(int a)
  {
    if(a>=-20 and a<=20)
      {
        x = a;
      }
    else
      {
        cout << "Point coordinates must be between -20 and 20" << endl;
      }
  }

  void setY(int a)
  {
    if(a>=-20 and a<=20)
      {
        y = a;
      }
    else
      {
        cout << "Point coordinates must be between -20 and 20" << endl;
      }
  }

  // Default Constructor
  Point()
    {
      x = 0;
      y = 0;
    }
};

int main()
{
  // Create a point
  Point p1;

  // Display current value of x and y
  cout << p1.getX() << ", " << p1.getY() << endl;

  // Change coordinates to 5, -3
  p1.setX(5);
  p1.setY(-3);

  // Display new values
  cout << p1.getX() << ", " << p1.getY() << endl;

  // Try to change to illegal point 30, 30
  p1.setX(30);
  p1.setY(30);

  return 0;
}
```

This program gives the expected output:

```text
0, 0
5, -3
Point coordinates must be between -20 and 20
Point coordinates must be between -20 and 20
```
