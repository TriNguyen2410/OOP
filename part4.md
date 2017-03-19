### Lists
Lists are containers, that behave similar to STL's vectors.
We will use our Node class for that, and store edge nodes in the list class.
List made like this has one slight problem, and thats time that it takes when we want to access / remove value that is not on the edge.

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
Now it's time to add method for adding values inside. We will set pointer ```current``` to our head node and then set the pointer to it's own child until the child will be ```NULL```.
This can cause problems with delays, when we have lot of values stored inside.
```cpp
void push_back (const T& value) {
  Node<T>* child = new Node<T>(value); /// Create new node with our value
  if (this->head == NULL) {
    this->head = child; /// If head node is not set, set it to our new node
  } else {
    Node<T>* selected = this->head; /// Mark our head node as selected
    while (selected->hasChildren()) { 
      selected = selected->getChild(); /// Until node has no childs, mark child of our marked node
    }
    selected->setChild(child); /// Add our child to last node
  }
}
```





