### Pointers
Pointers are variables that hold adress in memory as value. You can declare the pointer as ```type* pointer```.
As value, you can store adress of already existing static variable by using ```pointer = &variable``` or create new dynamic variable.

### Keyword ```new``` and ```new []```
This keyword is used to allocate new memory for single variable, like this:
```cpp
int* pointer = new int;
```
Or for allocating an array, which size can be specified while running:
```cpp
int size = 5;
int* pointer = new int[size]; /// Creates array of size 5
```

### Keyword ```delete``` and ```delete []```
After you stop using the variable, you need to deallocate it. If you do not, then it will remain in memory after you exit the application.
It's required to pair ```new``` \ ```delete``` and ```new []``` \ ```delete []```. If you use ```delete``` with ```new []```, then only the first element. Using ```delete []``` and ```new``` produces memory error.
```cpp
int* dynamic_variable = new int;
int* dynamic_array = new int[5];

delete dynamic_variable;
delete[] dynamic_array;
```

### Pointers to pointer
You can also use pointers to point to another pointer. For example, when you are creating an array or arrays. 
However, if you do so, you need to allocate (or deallocate) every 'step' on it's own. In case of array of arrays like this:
```cpp
int size = 10;

/// Allocation
int** array = new int*[size];
for (int i = 0; i < size; i++) array[i] = new int[size]; 

/// Deallocation
for (int i = 0; i < size; i++) delete[] array[i];
delete[] array;
```
