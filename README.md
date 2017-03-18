### Object Oriented Programming
OOP is a way of programming based around objects which contain own data and methods.
### Creating an object
Objects are created with keyword ```class``` or ```struct``` like this:
```cpp
class myObject {
};

struct myObject2 {
};
```
The difference between ```class``` and ```struct``` is default visibility of it's content, for example:
```cpp
class myObject {
  int f; /// This variable can be used only here (in declaration/definition of the class).
};

struct myObject2 {
  int f; /// This variable can be used from anywhere.
};
```
### Visibility
By default, visibility depends on keyword used when creating the class. However, this can be changed with keywords ```public```, ```proctected``` and ```private```. This example will use ```class``` keyword, to make visibility private, by default.
```cpp 
class myObject {
  int a; /// The same as with private.
  private:
    int b; /// Can be used only here.
  protected:
    int c; /// Similar to private, but can be used also from objects that extend this object.
  public:
    int d; /// Can be used from anywhere.
};
```
### Constructor
Constructor is special function in each object. It can be overloaded as any other function.
Return type is not used, since the function returns the object itself (creates it).
```cpp
class myObject {
  public:
    myObject () {
    }
    myObject (int a) { /// overloaded constructor
    }
};
```
Also, contructor can contain so-called defaults, which are default values for variables.
Then the value does not have to be specified and the default value is then used.
However, default values can be used only at the end of parameter list, with required parameters first.
```cpp
class myObject {
  public:
    myObject (int a = 0) {
    }
};
```
Constructor created like that can be called in two ways: ```myObject()``` aswell as ```myObject(2)```. The first way will function in this case like ```myObject(0)```.
### Destructor
This function is called when object goes out-of-scope. For example when static object is created and then the program ends.
It is very handy when you use pointers and ```new```, to call ```delete``` on the pointer for example.
```cpp
class myObject {
  public:
    myObject () {
    }
    ~myObject () {
    }
};
```












