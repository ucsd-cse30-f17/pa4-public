# PA4: Binary Search Tree (in C and Assembly)
In this assignment, you will write several C functions and two ARM assembly functions. The goal of this assignment is for you to implement Binary Search Tree class using dynamic memory allocation.

### 0. Getting Started
The Github Classroom link for your starter code is here:
[insert link here](http://)

### 1. Background
#### 1.1 Binary Search Trees: A Review from CSE 12
Let's review! A binary search tree (BST) is a data structure that's good for relatively fast search, insertion, and deletion. The nodes of a BST are connected via child and parent pointers (in this assignment, you will only have left and right pointers, no parent pointers).
##### Properties of a BST
In a BST, the node with no parent is called the root node. Each node has either 0, 1, or 2 children. A node is larger than its left child but smaller than its right child. (Left child's key < node's key < right child's key.) If any of these conditions are violated, then your tree is not a BST. 

#### 1.2 Structs
For this assignment, you will be implementing a BST "class". But wait! C is not an object-oriented language and does not have "classes" like Java or C++ - so how can we implement a data structure like a BST? Luckily, C does have a useful way to declare data types: using structs!

A struct is essentially a data type (much like `int` or `float` or `char *`), which is defined by you. But C doesn't have some built in data type for a BST or a BSTNode; you'll have to do it. (Actually, we did it for you in `bst.h`, which is provided in the starter code. Note: do not modify `bst.h`!)

A struct, in short, lets you as the programmer define a custom "data type" consisting of certain member variables, each of their own types (for example, `char *` or `BSTNode *`). Be careful! Just like any other variable declared in C, these member variables will not be initialized to 0 (unless you do so yourself, i.e. in bst_makeNode (a function you'll be writing), or with calloc(), which will be explained in section 1.3 of this writeup).

Below are the structs we've declared for you in `bst.h`.

```C
    struct BST {
       // member variables
       struct BSTNode* root;
    }
    struct BSTNode {
       // member variables
       char* key;
       BSTNode* left;
       BSTNode* right;
    }
    
    // Declaring a pointer to a BST
    struct BST* bst;
    // Declaring a pointer to a BSTNode
    struct BSTNode* node;
```
Note that when you declare a variable of type struct, you'll need to declare it as `struct BST* bst` - if you forget to include `struct` in your variable declaration, for example, if you try to declare a variable as `BST* bst`, you'll get an error.

To access the member of a struct, use the arrow operator `->`. For example, assuming you've declared a variable `struct BST* bst`, then you might access that BST's root like this: `bst->root`.

#### 1.3 Dynamic Memory Allocation
Recommended reading: C.8 in the textbook, and also refer to [TutorialsPoint](https://www.tutorialspoint.com/cprogramming/c_memory_management.htm). If you need an idea of how dynamic memory allocation for a struct might work, refer to `TestManualMallocAndFree()` in `test.c` in the pa4 starter code that is provided for you.

#### 1.4 Valgrind

### 2. Functions to implement in C
You will be implementing the following functions in the file named `bst.c`. We will provide a header file, `bst.h`, which contains the method signatures that you need to implement the BST. Please **do not modify** the signatures of any of the 9 functions listed below, and **do not modify** the `bst.h` file.

#### 2.1 `struct BSTNode* bst_makeNode(char* key, struct BSTNode* left, struct BSTNode* right)`
This function should use dynamic memory allocation to initialize a Node. Use the parameters (key, left, and right) to initialize the members of the struct BSTNode*.
#### 2.2 `void bst_add(struct BST* bst, char* key)`
This function should add a BSTNode* with the specified key to bst. Do nothing if a node with the same key already exists in the tree.
#### 2.3 `void bst_remove(struct BST* bst, char* key)`
This function should remove the BSTNode* with the specified key from bst. Do nothing if a node with this key does not exist in the tree.
#### 2.4 `int bst_contains(struct BST* bst, char* key)`
This function should search the tree for a node with the specified key. Return 1 if the node is found; otherwise, return 0.
#### 2.5 `char* bst_max(struct BST* bst)`
This function should return the key of the largest node in the tree (that is, the node with the largest key). This function is already implemented (correctly) for you in `bst.c`, which is provided along with `bst.h` in the starter code.
#### 2.6 `char* bst_min(struct BST* bst)`
This function should return the key of the smallest node in the tree (that is, the node with the smallest key).
#### 2.7 `int bst_count(struct BST* bst)`
This function should return the number of nodes in the tree.
#### 2.8 `int bst_totalLength(struct BST* bst)`
This function should sum the lengths of all the keys in bst, and return this value (the "total length").
#### 2.9 `void bst_deleteTree(struct BST* bst)`
This function deletes the whole bst tree. Think about what was dynamically allocated in the tree, and be sure to deallocate it. You can use valgrind to confirm that you have no memory leaks.

### IMPORTANT NOTE:
Again, **do not** modify the signatures of any of the methods declared in `bst.h`.

However, you will have some freedom with choosing the signatures of helper methods. Some examples of valid helper method signatures are commented out at the top of `bst.c` - feel free to use these. For example, a helper method for bst_add or bst_remove might be helpful, and you can change the parameters / return type of these helper methods if you'd like.

In `bst.c`, we've provided the (correct) implementation of bst_max for you. The rest is up to you!

### 3. Functions to implement in Assembly
You will be writing helper functions in ARM assembly, to help you implement bst_count and bst_totalLength.
#### 3.1 `int totalLength(struct BSTNode* node)`
#### 3.2 `void count(struct BSTNode* node)`

### 4. Testing your functions

### 5. README
#### 1. We've provided the `.o` files for ten different implementations of `bst.c`. 9 of the 10 are incorrect implementations, and 1 is correct. Below are ten short descriptions of the errors, in no particular order.
A. NO ERRORS.

B. `bst_add()` is incorrect.

C. `bst_add()` is incorrect (and violates order BST property).

D. `bst_remove()` is incorrect (removes incorrect node(s)).

E. `bst_remove()` is incorrect (does not always remove node).

F. `bst_count()` is incorrect.

G. The bst's structure is incorrect.

H. `bst_max()` and `bst_min()` are incorrect.

I. `bst_contains()` is incorrect.

J. `bst_totalLength()` is incorrect.

Match each of the descriptions to one of the bst#.o files given to you in bad_impls/ directory. You should write good tests in part 4, and use those tests to figure out which files match up to which errors.

Format your answer as a comma-separated list of the letters (i.e. "B, C, E, D, ...").
#### 2. Choose two of the above "incorrect implementations." Give a detailed explanation of the error that is made in each one. Also describe how you could fix it.

### 6. Commenting and style guide

### 7. Handin
