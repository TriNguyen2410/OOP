### Introduction to Stack and Queue
Stack and queue are container objects (store values), that are defined by their input and output methods.
That means that the inner workings of those containers do not matter.
This part of the tutorial will not use any Node class, instead dynamically alocated array will be used.

---
### About stack
This container contains two methods: ```push``` for input and ```pop``` for output.
When function ```push``` is called, then a value is inserted to the top (newest value) of the stack. 
Function ```pop``` then returns and removes the newest value on the stack.

For example, if you insert string like this: ```A B C D E F```, then the output pattern will look like this: ```F E D C B A```

---
### Creating Stack object
For storage, we will use array, which size of will be specified by constructor paramater. Also, size of array must be at least 2.
We will want to create destructor aswell, where we will deallocate our array.
Now we need to track position of the newest value, so we will use ```int postion``` for that, and set to 0 at start-
```cpp
class Stack {
  private:
    int* array;
    int size;
    int position;
  public:
    Stack (int size) {
      this->position = 0;
      this->size = (size >= 2 ? size : 2); /// Make size equal to 2 if less than 2
      array = new int[this->size]; /// Allocate our array
    }
    ~Stack () {
      delete[] this->array; /// Delete array after goind out-of-scope
    }
};
```

---
### Push
When we have the stack itself prepared, it's time to create method to insert a value into it. 
We will use our array to store our values at ```position```, because since it will be always increased after calling this method, it will always point to blank space in the array.
In case that the position will be out of range, then the method will return ```false```, otherwise ```true``` value.
```cpp
bool push (int value) {
  if (this->position < this->size) {
    this->array[this->position++] = value; /// Insert value into place and move to next free space
    return true;
  } else return false;
}
```
Now that we have a way how to insert values into the stack, we need a way to remove them.

---
### Pop
While ```position``` marks the next free place in array, ```position - 1``` marks the newest value added.
Now we will return that value while decrementing position to mark newest value as free spot again.
Also we need to protect the array against underflow (position must be greater than zero).
```cpp
int pop () {
  if (this->position > 0) {
    return this->array[--this->position]; /// Decrement the position and return the value
  }
}
```

---
### Helpful functions:
Peek method is usefull, if you only want to return the value without removing it from the stack.
It's almost identical to ```pop()```, but with ```this->position - 1``` instead of decrementing the actual value.
```cpp
int peek () {
  if (this->position > 0) {
    return this->array[this->position - 1]; /// Only return the value
  }
}
```
If you want to know, if there are still values inside the stack, you can use this short function:
```cpp
bool hasContent () {
  return this->position > 0; /// If position is greater than 0, then value is stored inside
}
```

---
### Example of usage
Here we are using Stack for storing 5 values and then printing them out.
```cpp
int main () {
  Stack my_stack(5); /// Create new Stack with space for 5 values
  for (int i = 0; i < 6; i++) { /// Insert 6 values, 5 will be used, 6th would be thrown away
    my_stack.push(i);
  }
  while (my_stack.hasContent()) { /// Remove and print values from Stack while it contains more values
    cout << my_stack.pop();
  }
  
  return 0;
}
```

---
### About queue
This container contains as with queue, two methods: ```put``` for input and ```get``` for output. When function 
```push``` is called, then value is inserted to the front of queue. When function ```get``` is called, the oldest value will
be removed and returned from queue.

For example, if you insert string like this: ```A B C D E F```, then the output pattern will look like this: ```A B C D E F```

---
### Creating Queue object
Inside of the queue will be very similar to the stack. Newest value will be added to the end of the array, and removed from the front. Then the entire array will be shifted to left, to fill the blank space. 
That makes it very time consuming when a lot of values is being stored inside. This problem can be solved by using Node class, which will be explained in later part.
Minimal size of array in the stack will be 2 again and the rest would be exactly the same as with stack.
```cpp
class Queue {
  private:
    int* array;
    int size;
    int position;
  public:
    Queue (int size) {
      this->position = 0;
      this->size = (size >= 2 ? size : 2); /// Make size equal to 2 if less than 2
      array = new int[this->size]; /// Allocate our array
    }
    ~Queue () {
      delete[] this->array; /// Delete array after goind out-of-scope
    }
};
```

---
### Put
After creating our queue, we will need to create a way how to insert a value into it. We will do exactly the same as we did with stack, since the process is the same for both.
```cpp
bool put (int value) {
  if (this->position < this->size) {
    this->array[this->position++] = value; /// Insert value and move position to next free
  } else return false;
}
```

---
### Get
This function is where most of the time is consumed, and the delay increases with more value added.
First, the value at position ```0``` is stored, and then the whole array is shifted to the left (in a cycle).
Underflow protection is required aswell, otherwise memory error will happen.
```cpp
int get ()  {
  if (this->position > 0) {
    int value = this->array[0]; /// Store first value in array (oldest)
    for (int i = 0; i < this->position; i++) { /// Shift array to the left
      this->array[i] = this->array[i + 1];
    }
    this->position--; /// Decrement position
    return value;
  }
}
```

---
### Helpful functions:
Again, these functions are identical to stack ones.
Peek method is usefull, if you only want to return the value without removing it from the stack.
It's almost identical to ```pop()```, but with ```this->position - 1``` instead of decrementing the actual value.
```cpp
int peek () {
  if (this->position > 0) {
    return this->array[this->position - 1]; /// Only return the value
  }
}
```
If you want to know, if there are still values inside the queue, you can use this short function:
```cpp
bool hasContent () {
  return this->position > 0; /// If position is greater than 0, then value is stored inside
}
```

---
### Example of usage
Now we can use Queue to store 20 numbers, and print them then out again.
```cpp
int main () {
  Queue my_queue(20);
  for (int i = 0; i < 20; i++) {
    my_queue.put(i);
  }
  while (my_queue.hasContent()) {
    cout << my_queue.get();
  }
  
  return 0;
}
```
