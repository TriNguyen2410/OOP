### class [Node](https://github.com/HafisCZ/VSB-PRG1/blob/master/algorithms2/Node.h)
This class is represents an object which stores these parameters:

**Type** | **Variable name** | **Meaning**
--- | --- | ---
C | value | Actual value of node itself
Node<C>** | subordinates | Children nodes (are located UNDER the node)
Node<C>* | superior | Parent of node (is located ABOVE the node)
int | subordinate_count | N of children nodes
bool | set | True if value is set

### Constructor
```cpp
Node(const C& value, Node<C> *superior = NULL, bool update_superior = false) {
	this->subordinate_ = NULL;
	this->subordinate_count_ = 0;
	this->superior_ = superior;
	if (this->superior_ != NULL && update_superior) this->superior_->addSubordinate(this);
	this->value_ = value;
	this->set_ = true;
}
```
This class constains one constructor, with 3 parameters:

```const C& value``` - Value of new node, required parameter

```Node<C> *superior``` - Parent of new node

```update_superior``` - Whether parent node should be updated with child node

Constructor sets parameters of node to default / function parameter values. If ```update_superior``` is specified, then calls ```addSubordinate()``` with itself as parameter on parent node.

### Get / Set methods
```cpp
inline Node** getSubordinates() {
	return this->subordinate_;
}
inline Node* getSuperior() {
	return this->superior_;
}
inline Node* getSubordinate() {
	return (this->subordinate_count_ > 0 ? this->subordinate_[0] : NULL);
}
inline Node* getSubordinateAt(int index) {
  return (index >= 0 && index < this->subordinate_count_ ? this->subordinate_[index] : NULL);
}
inline C getValue() {
	return this->value_;
}
inline int nSubordinates() {
	return this->subordinate_count_;
}
inline bool isSet() {
	return this->set_;
}
inline bool hasSubordinate() {
	return (this->subordinate_count_ > 0);
}
inline bool hasSuperior() {
	return (this->subordinate_ != NULL);
}
inline void setValue(const C& value) {
	this->value_ = value;
}
```
These methods are pretty basic, either set value or return value.

### fill()
```cpp
void fill(int count) {
	if (count == 0) this->subordinate_ = NULL;
	else if (count > 0 && count != this->subordinate_count_) {
		Node<C> **temporary = new Node<C>*[count];
		for (int i = 0; i < this->subordinate_count_; i++) temporary[i] = this->subordinate_[i];
		for (int i = this->subordinate_count_; i < count; i++) temporary[i] = NULL;
		delete[] this->subordinate_;
		this->subordinate_ = temporary;
		this->subordinate_count_ = count;
	}
}
```
This function takes number of children as parameter, and if it's greater than 0 and not matching current number of children, then expands array of child nodes inside of node.
First, new array of child nodes is created at size of ```count``` parameter. Then, existing values in old array are copied into the new one and the rest is set to ```NULL```. In the end, the old array is deleted and the pointer is set to the new array of children. Then the total child count is updated.
