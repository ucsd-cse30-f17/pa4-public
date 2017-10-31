# PA4: Binary Search Tree (in C and Assembly)
In this assignment, you will write several C functions and two ARM assembly functions. To implement your binary search tree, you will need to use dynamic memory allocation.
### 0. Getting Started
The Github Classroom link for your starter code is here:
[insert link here](http://)

### 1. Background
Reading
### 2. Functions to implement in C
You will be implementing the following functions in the file named bst.c (we will provide a header file, bst.h, which contains the method signatures that you need).
#### 2.1 `struct BSTNode* bst_makeNode(char* key, struct BSTNode* left, struct BSTNode* right)`
#### 2.2 `void bst_add(struct BST* bst, char* key)`
#### 2.3 `void bst_remove(struct BST* bst, char* key)`
#### 2.4 `int bst_contains(struct BST* bst, char* key)`
#### 2.5 `char* bst_max(struct BST* bst)`
#### 2.6 `char* bst_min(struct BST* bst)`
#### 2.7 `int bst_count(struct BST* bst)`
#### 2.8 `int bst_totalLength(struct BST* bst)`
#### 2.9 `void bst_deleteTree(struct BST* bst)`

### 3. Functions to implement in Assembly
You will be writing helper functions in ARM assembly to help you implement bst_count and bst_totalLength.
#### 3.1 `int totalLength(struct BSTNode* node)`
#### 3.2 `void count(struct BSTNode* node)`

### 4. Testing your functions

### 5. README
###### 1. We've provided the `.o` files for ten different implementations of the BST. Nine of these are incorrect implementations, and one is correct. Below are ten short descriptions of the errors.
A. NO ERRORS.
B. bst_add() is incorrect.
C. bst_add() is incorrect.
D. bst_remove() is incorrect.
E. bst_remove() is incorrect.
F. bst_count() is incorrect.
G. bst's structure is incorrect.
H. bst_max() and bst_min() are incorrect.
I. bst_contains() is incorrect.
J. bst_totalLength() is incorrect.
Match each of the descriptions to one of the bst#.o files given to you in bad_impls/ directory.
Format your answer as a comma-separated list of the letters (i.e. "B, C, E, D, ...").
2. Choose two of the above "incorrect implementations" and explain the error in detail, and how you could fix it.
