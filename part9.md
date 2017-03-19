### About binary trees
Binary tree is special case of tree, where each node can contain two children. It is used to sort values while they are being added.
Optimalization of these trees depends on how well is the tree balanced. For example, if we enter ```0``` as first value, and then add ```1``` and ```2```, we will get unbalanced tree.
This can in the end result in much longer time needed to reach some values. 
Problems like this can be fixed using balancing trees.

---
### Binary node
Even that we created our Node class before, lets make it less complicated and create new class, that will have only the required variables and methods. We will not use templates for now.
```cpp
class BNode {
  private:
    BNode *left_, *right_;
    int value_;
  public:
    BNode (int value) {
      this->left_ = NULL;
      this->right_ = NULL;
      this->value_ = value;
    }
    
    void destroy () {
      if (this->left_ != NULL) {
        this->left_->destroy();
      }
      if (this->right_ != NULL) {
        this->right_->destroy();
      }
      delete this;
    }
};
```
This is everything that we need to create and destroy our BNode. You can see that we made also ```destroy``` function, that will destroy all its children.
Now we need some Get\Set methods, to allow our tree to work with these nodes.
```cpp
int getValue () {
  return this->value_;
}

void setValue(int value) {
  this->value_ = value;
}

BNode* getLeft () {
  return this->left_;
}

BNode* getRight () {
  return this->right_;
}

void setLeft (BNode* left) {
  this->left_ = left;
}

void setRight (BNode* right) {
  this->right_ = right;
}
```
Because we will want to use only one function for Get\Set of ```right_``` and ```left_```, we can short them using bool parameter like this:
```cpp
void setSide (BNode* node, bool right) {
  if (right) {
    this->right_ = node;
  } else {
    this->left_ = node;
  }
}

BNode* getSide (bool right) {
  if (right) {
    return this->right_;
  } else {
    return this->left_;
  }
}
```

---
### Creating body for our tree
Now that we have our BNode fully prepared, we can begin to create body for our new class. We will need to know only the head node.
```cpp
class BinaryTree {
  private:
    BNode* head_;
  public:
    BinaryTree () {
      this->head_ = NULL;
    }
    
    ~BinaryTree () {
      if (this->head_ != NULL) {
        this->head_->destroy();
        }
    }
};
```
Our constructor will only set our head node to ```NULL``` when we create our tree. 
Because we are planning to use the tree as a static object, we can use destructor like that.

---
### Adding values inside
This is slightly more complicated part of our class. 
First, we need to make sure that we have head node present, otherwise we are going to set it to our new node with value.
We will use pointer ```selected``` to mark BNode we are currently working on, and ```target``` to mark our next BNode that we will look at.
We are going to use while loop to check all available nodes until ```selected``` will be NULL. That means that we reached the end.
Each cycle we will set ```target``` to child node, depending if the current value is greater or less than we are inserting. That will end up in stepping into left node in case our value is lower than current node's value.
If target is ```NULL``` (free place in right range was found), then we will place our new BNode to this place.
```cpp
void add(int value) {
  if (this->head_ == NULL) {
    this->head_ = new BNode(value);
  } else {
    BNode* selected = this->head_;
    BNode* target = NULL;
    while (selected != NULL) {
      if (selected->getValue() >= value) {
        target = selected->getLeft();
      } else {
        target = selected->getRight();
      }
      if (target == NULL) {
        selected->setSide(value, selected->getValue() < value);
      }
      selected = target;
    }
  }
}
```


