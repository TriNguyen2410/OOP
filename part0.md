### Raw pointers
Pointers are variables, that hold adress (position in memory) to another variable. They can also point nowhere (null pointers).
It is declared using this syntax: ```pointer_type* pointer_name;```

### Raw pointer to object
Using address-of operator ```&```, you can get adress of any object and store it in pointer .
```cpp
int n;
int *pointer_to_n = &n;
```
This will create a pointer ```pointer_to_n``` to our integer variable ```n```. Now we have two different variables, that are essentialy using the same data. That means we can dereference ```pointer_to_n``` by using dereference operator ```*``` and gain access to n.

### Keyword ```new``` and ```new []```
This keyword is used to allocate new memory for single variable, like this:
```cpp
int* pointer = new int;
```
Or for allocating an array, which size can be specified while the program is running:
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
