### Lists
Lists are containers, that behave similar to STL's vectors.
We will use our Node class for that, and store edge nodes in the list class.

---
### Types of lists
There are 3 types:

- Single linked list
- Double linked list
- Circular list

We are going to create only first two of those lists, since circular list is not helpful in any way when we use our Node class.

---
### Single linked list
This list contains head node, that points to next one, etc. It uses only pointer to its child, so parent is not needed.
It does not have to remember how many values are inside, but it can be useful is some cases.

---
### Creating the structure
This example will use templates to make it usable with any type. However, templated class cannot be split into more files, so declaration aswell as definition must be done in ```.h``` file.
```cpp
template <class T> class SingleLinkedList {
  private:
    Node<T> *head;
  public:
    SingleLinkedList () {
      this->head = NULL; /// Set our head node to nothing
    }
};
```
The class now contains contructor, that will only set our head node to ```NULL```. Since we use Nodes, we are not limited by size of array or anything else.

---
### Adding values
Now it's time to add method for adding values inside. 

- Adding values to the front

We will create new Node, set its child to ```head``` and set that node as our new head node. This requires minimum amount of time.
```cpp
void push_front (const T& value) {
  Node<T>* parent = new Node<T>(value); /// Create new head node
  if (this->head == NULL) {
    this->head = parent; /// If head node is not set, set it to our new node
  } else {
    parent->setChild(this->head); /// Set current head node as child to our new head node
    this->head = parent;
  }
}
```

- Adding values to the back

We will set pointer ```selected``` to our head node and then set the pointer to it's own child until the child will be ```NULL```.
This can cause problems with delays, when we have lot of values stored inside, since we need to go through all our nodes.
That problem is solved by double-linked list.
```cpp
void push_back (const T& value) {
  Node<T>* child = new Node<T>(value); /// Create new node with our value
  if (this->head == NULL) {
    this->head = child; 
  } else {
    Node<T>* selected = this->head; /// Mark our head node as selected
    while (selected->hasChildren()) { 
      selected = selected->getChild(); /// Until node has no child, mark child of our marked node
    }
    selected->setChild(child); /// Add our child to last node
  }
}
```

---
### Removing values
Again, it is possible to remove values from list from both sides.

- Removing from the front

Removing from the front is as easy as adding. However, we need to make sure that we have something to delete.
```cpp
T pop_front () {
  if (this->head != NULL) {
    Node<T>* child = this->head->getChild();
    T value = this->head->getValue();
    delete this->head;
    this->head = child;
    return value;
  }
}
```

- Removing from the back

Again, as with adding, first we need to go through all nodes, until we find the second bottom one, and remove its child.
```cpp
T pop_back () {
  if (this->head != NULL) {
    T value;
    if (this->head->getChild() == NULL) { /// If head has no child, remove head
      value = this->head->getValue();
      delete this->head;
      this->head = NULL;
      return value;
    } else {
      Node<T>* selected = this->head;
      while (selected->getChild()->getChild() != NULL) { /// Step in, while selected node has child without child
        selected = this->selected->getChild();
      }
      value = selected->getChildren()->getValue(); /// Get value of last node, then remove it
      selected->removeChildren();
      return value;
    }
  }
}
```

---
### Double linked list
As with single linked list, we will need to have a head node. But because we will use parents and children of nodes, we will have also pointer to the end of our list. This will help adding and removing elements from the end much quicker.

---
### Creating the structure
We are going to use templated class again.
```cpp
template <class T> class DoubleLinkedList {
  private:
    Node<T>* head, * tail;
  public:
    DoubleLinkedList () {
      this->head = NULL;
      this->tail = this->head;
    }
};
```

---
### Adding values
We are going to add values from both sides.

1) Adding values to the front

```cpp
void push_front (const T& value) {
  Node<T>* parent = new Node<T>(value); /// Create new head node
  if (this->head == NULL) {
    this->head = parent;
  } else {
    parent->setChild(this->head); /// Set head as parent's child
    this->head->setParent(parent);
    this->head = parent;
  }
}
```

2) Adding values to the back

This is much easier, since we can update the parent node while creating our child node.
```cpp
void push_back (const T& value) {
  this->tail = new Node<T>(value, this->tail, true); /// Set tail to new Node, which parent is old tail
}
```

---
### Removing values

This process is very similar to adding. We have to make sure to delete our Node, not call ```destroy``` on it, otherwise it will remove our whole list.

1) Removing values from the front

It looks a bit complicated, it's because we need to update node's child and parent at once. If we do not do that, we would get memory errors.
```cpp
T pop_front () {
  if (this->head != NULL) {
    Node<T>* child = this->head->getChild();
    child->setParent(NULL); /// Set parent of child to NULL (since it's being removed)
    T value = this->head->getValue();
    delete this->head;
    this->head = child;
    return value;
  }
}
```

2) Removing values from the back

Similar to removing from front, we simply delete the tail and replace it will its parent node.
```cpp
T pop_back () {
  if (this->tail != NULL) {
    Node<T>* parent = this->tail->getParent();
    T value = this->tail->getValue();
    this->parent->removeChildren(); /// Delete child of our parent node
    this->tail = parent;
    return value;
  }
}
```







