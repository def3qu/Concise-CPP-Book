<!-- nav -->
[Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Chapter 10 →](10-pointers.md)

# Chapter 9 - Inheritance

## 9.1 Introduction to Inheritance

One of the main features of object-oriented programming (OOP) is the ability to create new classes from existing classes. Inheritance defines an IS-A relationship between a parent class and a child class. These classes are also known as base and derived and superclass and subclass. So parent, base and superclass all describe the same thing.

Another way to think of inheritance is as specialization. We could create a base class for **Vehicles** and then specialize to derived classes for **Cars** and **Trucks**, each of which has attributes and methods that extend vehicle.

## 9.2 Basics of Inheritance in C++

- The class keyword is used for the definition of both base and derived classes. The base class is derived as any standalone class would be. A derived class starts off like a regular class, but adds a colon, an access specifier (public, protected, or private) and the name of the base class.
- Public, protected, and private inheritance. The table below summarizes the difference:

| Inheritance Type | Public Members of Base | Protected Members of Base | Private Members of Base |
| --- | --- | --- | --- |
| Public | Stay public in derived | Stay protected in derived | Inaccessible |
| Protected | Become protected in derived | Stay protected in derived | Inaccessible |
| Private | Become private in derived | Become private in derived. | Inaccessible |

Choose public inheritance when you want the derived class to be treated as the base class. Choose protected when you want the derived class to have access to public and protected members of the base class, but block those from external access. Use private inheritance when you only need implementation details from the base class without exposing them.

## 9.3 Creating and Using Derived Classes

### 9.3.1 Defining a base class

Let's say we define a base Animal class, which has a private attribute **weight** and a public method called **eat.** Here is a look at that class:

```cpp
class Animal
{
    private:
      double weight;
    public:
      void eat()
      {
        cout << "Eating" << endl;
      }
};
```

Now that we have our animal class, we can define specific animals, like a dog or a cat. This is inheritance since a Dog IS-A Animal. We could create a new Dog class, but we want to be able to take advantage of the functionality already coded in Animal. There will be more benefits that we will discuss later.

Here is the syntax for a simple Dog class:

```cpp
class Dog : public Animal
{
 private:
  string breed;
 public:
  void bark()
  {
    cout << "Barking...\n";
  }
};
```

Since a Dog object IS-A Animal, it has all of the **public** attributes and methods of the Animal class. If there were any **private** attributes, they would exist in the derived class, but objects of the derived class would not have direct access.

Here is an example of using Animal and Dog in a program.

```cpp
int main()
{
    Dog d;
    Animal a;
    d.eat();  // Inherited from Animal
    d.bark(); // Defined in Dog
    a.bark(); // Will not work since defined in subclass
    return 0;
}
```

## 9.4 Constructors in Inheritance

In the above example, we used default constructors, so no parameters were passed. If we want a more useful constructor that assigns initial values to objects of the base and derived class, we are going to have to call the base class constructor from the derived class constructor. Here is an example:

```cpp
// Base class with a parameterized constructor
class Base
{
  private:
    int baseValue;
  public:
    // Base class constructor
    Base(int v)
    {
        baseValue = v;
        cout << "Base class constructor called with value: "
            << baseValue << endl;
    }
};
// Derived class
class Derived : public Base
{
  private:
    int derivedValue;
  public:
    // Derived class constructor that calls Base constructor
    Derived(int baseV, int derivedV) : Base(baseV)
    //       both values come in        to base
    {
        derivedValue = derivedV;
        cout << "Derived class constructor called with value: "
            << derivedValue << endl;
    }
};
```

So, when we go to create an object of type Derived, we need to pass along both baseValue and derivedValue. The baseValue is passed to the base class constructor, while the derivedValue is set in the derived class constructor.

## 9.5 Function Overriding and Pure Virtual Functions

- Member methods that are defined in the base class can be overridden in the derived class. This allows for the specialization of the subclass.

Example:

Let's go back to the base Animal class and the derived Dog class from above. Let's say that the Animal class has the following public method:

```cpp
void makeSound()
{
    cout << "Animal makes a sound!" << endl;
}
```

We can override this function in the Dog class as follows:

```cpp
void makeSound()
{
    cout << "Woof! Woof!" << endl;
}
```

How does the program know which makeSound() to call? It is based on the object type of the calling object. Example code snippet:

```cpp
Animal a;
Dog d;
a.makeSound();
d.makeSound();
```

will have the following output:

```text
Animal makes a sound!
Woof! Woof!
```

### 9.5.1 Pure Virtual Functions

There are times that we want to make a generic class that we will never make objects from. It is created to form a template for other subclasses that we do make objects from. We call these **abstract** classes as compared to **concrete** classes. To make a class abstract, we add a **pure virtual** function.

To create a pure virtual function, we start by prepending the keyword **virtual**, and then put the function signature, then end with =0; So, to create the makeSound() function of the Animal class above a pure virtual, we would do the following:

```cpp
virtual void makeSound() = 0;
```

We do not define the function at all, just give the signature.

Pure virtual functions create an obligation in any class derived from the abstract class. So, with the change above, we cannot make any objects of type Animal. When we make a Dog class that inherits from Animal, it MUST have a **void makeSound()** method, or it will cause an error.

Here is a full example of an Abstract Animal Class with two derived classes, Dog and Cat.

```cpp
class Animal
{
  private:
    string name;
  public:
    virtual void makeSound()=0;
    Animal(string n)
    {
        name = n;
    }
};
class Dog : public Animal
{
  private:
    string breed;
  public:
    void makeSound()
    {
        cout << "Woof! Woof!" << endl;
    }
    Dog(string n, string b) : Animal(n)
    {
        breed = b;
    }
};

class Cat : public Animal
{
  private:
     double weight;
  public:
    void makeSound()
    {
        cout << "Meow! Meow!" << endl;
    }
    Cat(string n, double w) : Animal(n)
    {
        weight = w;
    }
};
```

Here is a test program using the above classes:

```cpp
int main()
{
    // Animal a("Bob"); We can't do this since Animal is abstract
    Dog d("Alice", "Corgie");
    d.makeSound();
    Cat c("Fluffy", 7.89);
    c.makeSound();
    return 0;
}
```

Note that when I try to initialize an Animal object, I get the following error: "cannot declare variable ‘a’ to be of abstract type ‘Animal’"

<h1>This chapter is unfinished</h1>
---

[← Previous: Chapter 8](08-structs-and-classes.md) | [Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Chapter 10 →](10-pointers.md)
