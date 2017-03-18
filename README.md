## Object Oriented Programming
OOP is a way of programming based around objects which contain own data and methods.

---
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
---
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
---
### Variables and ```this``` keyword
Keyword ```this``` represents pointer to the object itself.

There are 2 ways, how to access variables inside of the object:
```cpp
variableA = 5;
this->variableA = 5;
```
Both of them are the same, where all represent the same variable:
```cpp
variableA
this->variableA
(*this).variableA;
```
---
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
Constructor created like that can be called in two ways: ```myObject()``` aswell as ```myObject(2)```. 
The first way will function in this case like ```myObject(0)```.

---
### Calling constructor
This depends on the actual constructor. If constructor doesn't use any parameters (given that no overloaded constructors are used), then new instance of object is created like this:
```cpp
myObject obj;
```
In case that object requires / has default parameters, the constructor is required to be called like this:
```cpp
myObject obj(); /// When default value is used
myObject obj2(5); /// When value is required
```
---
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
However, this can be dangerous, if you decide to use ```delete``` while using the same adress in multiple places.
---
### Methods
You can use any function inside of object, like you would use it outside. Remember to use ```public``` visibility to make sure that the function can be used outside of the object. 
```cpp
class myObject {
  public:
    myObject () {
    }
    ~myObject () {
    }
    void myFunction () {
    }
};
```
Function written like that would be then used like this:
```cpp
myObject obj; /// Create new instance of object
obj.myFunction(); /// Call function on myObject obj
```
---
### Get\Set methods
This is not completely neccesary step, however, it's better and more secure to use these methods. This can prove useful, when you does not want the user to directly use variables stored inside the object. Simply, use ```private``` keyword for that variable and create a pair of functions, where one of them would return the value of the variable, and another will accept value and then set the variable inside the object to that value.
```cpp
class myObject {
  private:
    int a;
  public:
    void setA (int new_a) {
      a = new_a;
    }
    int getA () {
      return a;
    }
};
```
---
### Example
Here you'll see how a real life scenario can be turned into few objects:
> Imagine you have container with liquid, and you want to know, how much does the container weight with any liquid. Use kilograms and cubic meters as units.
It does sound slightly complicated, but let's break it into few steps.
- Liquid

First step in recreating this scenario is to create an object that will represent liquid. We also want to know what is the density of the liquid. Let's do this then:
```cpp
class Liquid {
  private:
    double density; /// Store density of liquid as double
  public:
    Liquid (double liquid_density) { /// Create constructor with density as required parameter
      density = liquid_density;
    }
    double getDensity () {
      return density;
    }
};
```
Now we created new object that represents a liquid and one get method, so we will know how dense the liquid is. 
- Container

Lets say, that the container is weight-less. We will want to know, what volume the container has. Also, we want to know, what is the weight of the liquid inside.
```cpp
class Container {
  private:
    double volume;
    Liquid* liquid;
   public:
    Container (double vol) { /// Volume of container is required when the object is created
      volume = vol;
    }
    void fill (Liquid* liq) { /// 'Fills' container with certain liquid (by pointer)
      liquid = liq;
    }
    double getWeight () { /// Returns weight of the container
      if (liquid != NULL)
        return volume * liquid->getDensity();
      else
        return 0;
    }
};
```
This is basically everything that we need for our small example. Now we can use it in our ```main``` method:
```cpp
int main () {
  Liquid water(1000); /// Create liquid with density of 1 ton per cubic meter
  Container bucket(0.1); /// Make container with volume 0.1 m^3
  bucket.fill(&water);
  cout << bucket.getWeight(); /// Print out the weight into console output (std)
  return 0;
}
```















