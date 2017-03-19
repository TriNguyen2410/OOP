### Difference in using nodes instead of arrays
The main difference is in how quickly you can add or remove elements from those structures.
However, this approach consumes more memory, since each node needs to know address of its child.

Both stack and queue will in the end have unlimited size.

---
### Stack
Here, we will use similar approach like in single linked list. 
The only changes is, that we will be able to add / remove only the top-most node (last inserted, youngest).

We are going to use templates, since our Node class is templated aswell.

---
### Body with constructor & destructor
We are going to use simple constructor, just to set our head node to ```NULL```. Also, since we want to dispose of all remaining nodes after our stack goes out-of-scope, we will use destructor, that will call ```destroy``` on the head node. Resulting into deleting of all nodes.
```cpp
template <class T> class NodeStack {
  private:
    Node<T> *head_;
  public:
    NodeStack () {
      this->head_ = NULL;
    }
    
    ~NodeStack () {
      if (this->head_ != NULL) {
        this->head_->destroy();
      }
    }
};
```

---
### Adding elements
This is done by creating a new Node, setting the current head as child to it and then setting our new node as head.
```cpp
void push (const T& value) {
  Node<T> *parent = new Node<T>(value); /// Create new Node
  if (this->head_ != NULL) {
    parent->setChild(this->head_); /// If there is existing node, set it as child to our new one
  }
  this->head_ = parent; /// Set new node as parent node
}
```

---
### Removing elements
If we want to remove element from our stack, we are going to need to get child of our head node. If there is no child, then head is set to ```NULL``` (because all node's children are set by default to ```NULL```).
```cpp
T pop () {
  Node<T> *child = this->head_->getChild(); /// Get child of our parent
  T value = this->head_->getValue();
  delete this->head_;
  this->head_ = child;
  return value;
}
```

---
### Helpful methods
These methods can be useful in some cases. This function will allow us to see value on top of the stack without removing it.
```cpp
T peek () {
  return this->head_->getValue();
}
```
And this function will allow us to know if there are any remaining values inside of our stack. It can be used for example when you plan to use infinite loops to remove all values from the stack.
```cpp
bool hasElements () {
  return this->head_ != NULL;
}
```

---
### Queue
This is slightly more tricky than stack, since we are adding to the front and removing from the end.
However, this time we are going to use two pointers instead:

- ```oldest``` - Oldest value in the queue, the same as head node from before.
- ```youngest``` - Youngest value, but it is the last child of our nodes in the queue (oldest will point in the youngest direction).

It can be done in opposite direction aswell, using only parent nodes. That is completely irrelevant to the functioning of this queue.

---
### Body with constructor & destructor
We want to set all pointers to ```NULL``` again, to make sure that we have all time valid starting point in the queue.

And we are using templated class here once again. 
```cpp
template <class T> class NodeQueue {
  private:
    Node<T> *youngest_, *oldest_;
  public:
    NodeQueue () {
      this->youngest_ = NULL;
      this->oldest_ = NULL;
    }
    
    ~NodeQueue () {
      if (this->oldest_ != NULL) {
        this->oldest_->destroy(); /// We need to call this method on our oldest node, since it's the head node
      }
    }
};
```

---
### Adding elements
First, we need to create our new Node. In case that there are no nodes present, we will set it as ```youngest_``` and point ```oldest_``` to ```youngest_```. This process is required, because we are using ```youngest_``` solely for adding and ```oldest_``` for removing elements from the queue.
```cpp
void put (const T& value) {
  Node<T> *child = new Node<T>(value);
  if (this->oldest_ == NULL || this->youngest_ == NULL) { /// If there are no nodes present, we need to point oldest to youngest, to maintain the connection between them
    this->youngest_ = child;
    this->oldest_ = this->youngest_;
  } else {
    this->youngest_->setChild(child); /// We need to set our old youngest as child to our new node
    this->youngest_ = child;
  }
}
```

---
### Removing elements
This method is identical to the Stack's one.
```cpp
T get () {
	Node<T> *child = this->oldest_->getSubordinate();
	T value = this->oldest_->getValue();
	delete this->oldest_;
	this->oldest_ = child;
  return value;
}
```


---
### Helpful methods
The same methods can be used as with Stack.
This function will allow us to see value in the end of the queue without removing it.
```cpp
T peek () {
  return this->oldest_->getValue();
}
```
And this function will allow us to know if there are any remaining values inside of our queue. It can be used for example when you plan to use infinite loops to remove all values from the queue.
```cpp
bool hasElements () {
  return this->oldest_ != NULL;
}
```

---
### Practical usage & Example
These classes can be used for example in Breadth-First-Search or Depth-First_Search algorithms.

```cpp
NodeStack node_stack;
for (int i = 0; i < 10; i++) {
  node_stack.push(i);
}
while (node_stack.hasElements()) {
  cout << node_stack.pop();
}

NodeQueue node_queue;
for (int i = 0; i < 10; i++) {
  node_queue.put(i);
}
while (node_queue.hasElements()) {
  cout << node_queue.get();
}
```
