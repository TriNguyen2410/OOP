## Using unique pointers
Since C++11, we can use several types of smart pointers - especially unique pointers. 
These pointers behave pretty much like raw pointers, but have direct ownership of whatever is stored in them.
That means that you can't just point two unique pointers to the same place.
After the unique pointer is destroyed, the object that it owns is destroyed aswell, 
so you do not have explicitly call ```delete``` after you stop using the object.

These pointers are included in library ```<memory>```!

## Creating unique pointers
To create a new pointer, we use this syntax:
```cpp
  std::unique_ptr<OBJECT_TYPE> ptr_name(OBJECT);
```
Where ```OBJECT_TYPE``` is type of object that we will be using this pointer for, and ```OBJECT``` is the object, that we will assign to our newly created pointer.
Note, that we will lose the object, if the unique pointer would be destroyed.

We can also create new pointer from object using function ```std::make_unique()``` in following syntax:
```cpp
   std::unique_ptr<OBJECT_TYPE> ptr_name = std::make_unique<OBJECT_TYPE>(OBJECT);
```

## Moving object from one pointer to another
We can do this using following function:
```cpp
  std::unique_ptr<OBJECT_TYPE> unique_ptr_name_A(OBJECT);
  std::unique_ptr<OBJECT_TYPE> unique_ptr_name_B;
  unique_ptr_name_B = std::move(unique_ptr_name_A);
```
Note that, using ```std::move``` will not set the previous pointer to ```nullptr```

## Getting raw pointer to object from unique pointer
Using this, we can get raw pointer to our object, that our pointer points to. But if the pointer would get deleted, we will have raw pointer that points into random place.
```cpp
  std::unique_ptr<OBJECT_TYPE> unique_ptr_name(OBJECT);
  OBJECT_TYPE *raw_ptr = unique_ptr_name.get();
```
