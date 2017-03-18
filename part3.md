### Objectives of this class
This class will serve as base for improved Queues, Stacks and Lists. And a modification of this class will be used later aswell. 
We want to hold these value inside:
1) Value of the node itself
2) Pointer to its parent
3) Pointer to its children

Because we want it to be used in more places, we need to have the option to set parent and chilren nodes.
Everything will be made with pointers, because we don't want to unnecessary copy our nodes all over.
The only required value we need whilst creating the node is the value itself, the rest can stay set to ```NULL```.
We will also make all variables inside private, and use functions as interface with the node.

---
### Templates
Because we want to use any type of value, we are going to need to use templates. You create templated class like this:
```cpp
template <class T> class myObject { /// T behaves like #DEFINE, however you can specify it for each instance created
  public:
    myObject () {
    }
};
```
And then you use it like this: ```myObject<int> my_object;```

---
### Body of the Node
Now we will create body for the class, which will for now contain only variables that we want.
Because we do not know how many children we want, we need to use pointer-to-pointer.
```cpp
template <class T> class Node {
  private:
    Node<T> **children_;
    Node<T> *parent_;
    T value_;
    int children_count_;
  public:
};
```
This will prepare every possible variable we will need inside of our node. Variable ```children_count_``` represents size of our ```children_``` array.

---
### Constructor
Because we want optional parameters, we will use default values. You can use overloads, but it's unnecessary.
Also, we will add option to call function on parent, that will set the node as its child. We will create that function later.
```cpp
Node (const T& value, Node<T> *parent = NULL, bool update_parent = false) {
  this->value_ = value;
  this->children_ = NULL;
  this->children_count_ = 0;
  this->parent_ = parent;
  if (this->parent_ != NULL && update_parent) {
    this->parent_->addChild(this); /// If parent is not blank and should be updated, then set it's child to this node
  }
}
```

---
### Destructor
After we are finished with the node, we will want to delete itself aswell as all children nodes.
However, putting this into destructor could cause some problems, so we will put it into separate function, that we will call after we will not want the Node to exist anymore.
```cpp
void destroy () {
  for (int i = 0; i < this->children_count_; i++) {
    this->children_[i]->destroy(); /// Call this function on every child node in the tree
  }
  delete this;
}
```

---
### Get\Set Methods
Now that we have our Node prepared, we need to have a way how to access all those internal variables.
We will do that by writing few functions, that will do what we need.
1) Get\Set the value
```cpp
T getValue () {
  return this->value_;
}

void setValue (const T& value) {
  this->value_ = value;
}
```
2) Get\Set\Has\Count all children at once
```cpp
Node<T>** getChildren () {
  return this->children_
}

void setChildren (Node<T>** children, int count) { /// Amount is required
  this->children_ = children;
  this->children_count_ = (children == NULL ? 0 : count); /// Set to 0, if no children are present
}

bool hasChildren () {
  return this->children_count_ > 0;
}

int nChildren () {
  return this->children_count_;
}
```
3) Get\Set\Has parent node
```cpp
Node<T>* getParent () {
  return this->parent_;
}

void setParent (Node<T>* parent) {
  this->parent_ = parent;
}

bool hasParent () {
  return (this->parent != NULL);
}
```
4) Get child variants

Now we will need 2 functions, where we will return child on exact position in array, or at position 0.
```cpp
Node<T>* getChild () {
  if (this->child_count_ > 0) { /// If there is one or more children, return first child
    return this->children_[0];
  } else {
    return NULL;
  }
}

Node<T>* getChildAt (int position) {
  if (this->child_count_ > position && position >= 0) { /// Position needs to be between the number of children and 0
    this->children_[position];
  } else {
    return NULL;
  }
}
```
5) Fill function

This function increases or decreases the size of array of children. This will in the end reduce a bit of code in next functions.
If we want to resize the children array to ```0```, we need to deallocate the array and set its size to 0.
Otherwise, if the size is different than current size, then we will allocate new array of that size.
Next step is to copy all data from the old array and fill the rest with ```NULL```.
In the end we will delete the old array and replace with new one.
```cpp
void fill (int count) {
  if (count == 0) {
    if (this->children_count_ != 0) {
      delete[] this->children_;
      this->children_count_ = 0;
    }
    this->children_ = NULL;
  } else if (count > 0 && count != this->children_count_) {
    Node<T>** array = new Node<T>*[count];
    for (int i = 0; i < this->children_count_; i++) {
      array[i] = this->children_[i];
    }
    for (int i = this->children_count_; i < count; i++) {
      array[i] = NULL;
    }
    delete[] this->children_;
    this->children_ = array;
    this->children_count_ = count;
  }
}
```
6) Set child variants

Again, two functions will be created. One for setting the child to position 0 in the array, and the other to specified position.
```cpp
void setChild (Node<T>* child) {
  setChildAt(child, 0);
}

void setChildAt (Node<T>* child, int position) {
  if (position >= 0) {
    if (position >= this->children_count_) fill(position + 1); /// If there is no space, then expand the array as required
    this->children_[position] = child;
  }
}
```
7) Add child

Another useful variant of ```setChild```. Increases child array by one and adds the child there.
```cpp
void addChild (Node<T>* child) {
  fill(this->children_count_ + 1);
  this->children_[this->children_count_ - 1] = child;
}
```






