# PA4: Binary Search Tree (in C and Assembly)
In this assignment, you will write several C functions and two ARM assembly functions. The goal of this assignment is for you to implement and test a Binary Search Tree “class” using dynamic memory allocation.

## Table of Contents:
0. [Getting Started](https://github.com/ucsd-cse30-f17/pa4-public#0-getting-started)
1. [Background](https://github.com/ucsd-cse30-f17/pa4-public#1-background) ([BSTs](https://github.com/ucsd-cse30-f17/pa4-public#11-binary-search-trees-a-review-from-cse-12) | [Structs](https://github.com/ucsd-cse30-f17/pa4-public#12-structs) | [Dynamic Memory Allocation](https://github.com/ucsd-cse30-f17/pa4-public#13-dynamic-memory-allocation))
2. [Functions to implement in C](https://github.com/ucsd-cse30-f17/pa4-public#2-functions-to-implement-in-c)
3. [Functions to implement in Assembly](https://github.com/ucsd-cse30-f17/pa4-public#3-functions-to-implement-in-assembly)
4. [Testing your functions](https://github.com/ucsd-cse30-f17/pa4-public#4-testing-your-functions)
5. [Valgrind](https://github.com/ucsd-cse30-f17/pa4-public#5-valgrind)
6. [Compiling your code](https://github.com/ucsd-cse30-f17/pa4-public#6-compiling-your-code)
7. [README](https://github.com/ucsd-cse30-f17/pa4-public#7-readme)
8. [Commenting and style guide](https://github.com/ucsd-cse30-f17/pa4-public#8-commenting-and-style-guide)
9. [Handin](https://github.com/ucsd-cse30-f17/pa4-public#9-handin)
10. [Grading](https://github.com/ucsd-cse30-f17/pa4-public#10-grading)

### 0. Getting Started
The Github Classroom link for your starter code is here:
[insert link here](http://)

### 1. Background
#### 1.1 Binary Search Trees: A Review from CSE 12
Let's review! A binary search tree (BST) is a data structure that, hopefully, you've seen before. A BST has a pretty good average-case running time for search, insertion, and deletion (O(log(n)). The nodes of a BST are connected via child and parent pointers (in this assignment, you will only have left and right pointers, no parent pointers).
##### Properties of a BST
In a BST, the node with no parent is called the root node. An empty tree is the tree without any nodes (no root node). Each node in the tree has either 0, 1, or 2 children. A node's key must be larger than all the values in its left subtree and smaller than all the values in its right subtree. If any of these conditions are violated, then your tree is not a BST. 

##### This is a valid binary search tree (whose nodes have `int` type keys)
              100
           /        \
          50        200
         /  \     /     \
       20   52   190    210
       /   /  \   \     /  \
     10  51   53  191 205  300
 

##### This is a valid binary search tree (whose nodes have `char *` type keys, like the bst you'll be implementing)
              Hello
           /         \
        Dog          Joke
       /    \        /   \
     Cat  Elephant  Jest Mitochondria
     /                      \
    C                        No
                               \
                               Yes
#### 1.2 Structs
For this assignment, you will be implementing a BST "class". But wait! C does not have "classes" like Java or C++ - so how can we implement a data structure like the BST? Luckily, C does have a useful way to declare custom data types:  structs!

A struct is language feature for defining new, compound data types, where “compound” means that it allows us to combine multiple pieces of data together into one value. We've defined the struct types for BST (which is just a reference to a root node), and for BSTNode, which makes up the tree itself. Note that each struct definition comes with a new type we can use. In this case, we'll be using the type `struct BSTNode*` a lot – that is, an address in memory where the data for a BSTNode is stored.

Each field of the struct has its own types (for example, `char *` or `BSTNode *`). Be careful here! Just like any other variable declared in C, these fields will not be initialized to 0 or NULL by C. You'll be responsible for setting the values of these fields.

Below are the structs we've declared for you in `bst.h`.

```C
    struct BST {
       // fields
       struct BSTNode* root;
    }
    struct BSTNode {
       // fields
       char* key;
       BSTNode* left;
       BSTNode* right;
    }
```

Note that when you declare a variable of a struct type, you'll need to declare it as `struct BST* bst` - if you forget to include `struct` in your variable declaration, for example, if you try to declare a variable as `BST* bst`, you'll get an error.

```
    // Declaring a pointer to a BST
    struct BST* bst;
    // Declaring a pointer to a BSTNode
    struct BSTNode* node;
```

To access the member of a struct via a pointer, use the arrow operator `->`. For example, assuming you've declared a variable `struct BST* bst`, then you might access that BST's root like this: `bst->root`.

#### 1.3 Dynamic Memory Allocation
Recommended reading: Section [C.8](http://booksite.elsevier.com/9780128000564/content/APP0C_C_Programming.pdf) in the textbook. For extra help, you can also refer to [TutorialsPoint](https://www.tutorialspoint.com/cprogramming/c_memory_management.htm).

If you need an idea of how dynamic memory allocation for a struct might work, refer to `TestManualMallocAndFree()` in `test.c` in the provided starter code. The lectures from November 1 and 3 cover the use of dynamic allocation on structs in detail. For this programming assignment, you'll need to allocate everything on the Heap (using `malloc` or `calloc`), and then deallocate everything you allocated (using `free`).

Refer to [Section 5]() (on Valgrind) to read about testing yoru code for memory leaks.

### 2. Functions to implement in C
You will be implementing the following functions in the file named `bst.c`. We will provide a header file, `bst.h`, which contains the method signatures that you need to implement the BST. Please **do not modify** the signatures of any of the 9 functions listed below, and **do not modify** the `bst.h` file.

You'll want to start from `bst_makeNode`, as the rest of your BST will depend on this function. Using unit tests (testing each method individually) is a good idea. Once you’ve implemented `bst_makeNode`, test it! You should test each method you write as you go: refer to [section 4, Testing](https://github.com/ucsd-cse30-f17/pa4-public#4-testing-your-functions). 

After `bst_makeNode` is finished and tested, you should be able to test your other functions as you write them.

You can manually create a tree in your tester, without calling on any functions other than `bst_makeNode`. Doing this means that even if your `bst_add` is buggy, you'll still be able to test other methods. (However, if you're confident in your implementation of `bst_add`, feel free to use it when testing your other methods.)

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
This function should just call its recursive helper, `count`, implemented in Assembly (see section 3). The function should return the number of nodes in the tree.
#### 2.8 `int bst_totalLength(struct BST* bst)`
This function should just call its recursive helper, `totalLength`, implemented in Assembly (see section 3). The function should sum the lengths of all the keys in bst, and return this value (the "total length").
#### 2.9 `void bst_deleteTree(struct BST* bst)`
This function deletes the whole bst tree. Think about what was dynamically allocated in the tree, and be sure to deallocate it. **Make sure to deallocate the bst itself as well, which should have been dynamically allocated in your tester (or wherever the bst was created).** You can use valgrind to confirm that you have no memory leaks.

### IMPORTANT NOTE:
Again, **do not** modify the signatures of any of the methods declared in `bst.h`.

However, you will have some freedom with choosing the signatures of helper methods. Some examples of valid helper method signatures are commented out at the top of `bst.c` - feel free to use these. For example, a helper method for bst_add or bst_remove might be helpful, and you can change the parameters / return type of these helper methods if you'd like.

In `bst.c`, we've provided the (correct) implementation of `bst_max` for you. The rest is up to you!

### 3. Functions to implement in Assembly
You will be writing helper functions in ARM assembly, to help you implement `bst_count` and `bst_totalLength`.
#### 3.1 `int totalLength(struct BSTNode* node)`
#### 3.2 `int count(struct BSTNode* node)`

### 4. Testing your functions
It is really important to write good tests to detect errors in your code and make sure they work as expected.
You will also need your tests to help you figure out what is wrong in the buggy implementations later for the README question. 
#### You will be graded on your tests based on how many bad implementations your test can catch, described below
For this assignment, you need to use a simple framework for C called CuTest to write tests. We provide you the following:
```
cutest: This is the folder that contains all that you need to use CuTest. 
runtests.c: The main driver to run all tests. 
test.c: The file where all your tests should be in.
```
You only need to modify your test.c to add more tests.

Your test should have a signature like this: `void Test<Functiontested>_<TestInput>(CuTest *tc){}`
  
Here is the list of assert functions that CuTest provides:
```
    void CuAssert(CuTest* tc, char* message, int condition);
    void CuAssertTrue(CuTest* tc, int condition);
    void CuAssertStrEquals(CuTest* tc, char* expected, char* actual);
    void CuAssertIntEquals(CuTest* tc, int expected, int actual);
    void CuAssertPtrEquals(CuTest* tc, void* expected, void* actual);
    void CuAssertPtrNotNull(CuTest* tc, void* pointer);
```

To use these assert functions, just call them by passing in tc as the first argument and the other required arguments.
[More info about CuTest](https://github.com/ennorehling/cutest)

![Sample Output](https://raw.githubusercontent.com/ucsd-cse30-f17/pa4-support/master/testOutput.png?token=AXdWtOWxrRt6SMjwZ-gwcLvVV5cKpvMBks5aA0Y7wA%3D%3D)

An important note: If one assert fails, the rest in the same test function won't execute.

#### Thoroughly Testing

We've provided the `.o` files for ten different implementations of `bst.c` that we wrote ourselves. One of them is correct, and the other 9 have mistakes.

To make sure your tests are thorough, you will run your tests against these bad implementations to make sure your tests catch each of the bad implementations, and succeed on the correct one.

In order to run against a bad implementation

```
make testbst%  (% is 1-10)
```

Below are 10 descriptions of the implementations. These intentionally *do not* correspond to the numbers 1-10 for the .o files. Part of your task is to match them in the README below.

```
A. NO ERRORS.
B. Builds a BST with no right children.
C. Does not always add a new node.
D. Removes incorrect node(s).
E. Does not always remove node.
F. Structure is right but count is wrong.
G. Violates a fundamental property of BSTs (is backwards).
H. Incorrectly identifies smallest/largest nodes in the tree.
I. Search succeeds when it should not.
J. Total length doesn't sum the lengths of all the nodes' keys.
```
### 5. Valgrind
But how do you check to make sure that you've allocated and deallocated memory properly? That's where Valgrind comes in. After you've written `bst.c` and written some tests (see [section 4](https://github.com/ucsd-cse30-f17/pa4-public/blob/master/README.md#4-testing-your-functions) of this writeup), you can use the following command to run valgrind:

```
    make vtest
```
##### Here are some of the memory errors that you might see during this PA, which valgrind can help you catch:

1. Memory allocated but not freed

![needs_to_free](https://raw.githubusercontent.com/ucsd-cse30-f17/pa4-support/master/valgrind1.png?token=AXdWtGOEsclwwWBQl-nxSkPliZIhI3Otks5aArYAwA%3D%3D)
   * Pay attention to the HEAP SUMMARY: "in use at exit: 794 bytes in 94 blocks" and "total heap usage: 129 allocs, 35 frees, 9,682 bytes allocated". You want to see the same number of allocs as there are frees, and 0 bytes in use at exit.
   
   * Below the HEAP SUMMARY, you'll find the details on where exactly the memory leak occurred. In this screenshot, valgrind sees that memory was allocated for a string in strdup (in bst_makeNode), but was never freed (thus causing a memory leak to occur).
   
2. Deallocating memory after it has already been deallocated

![freed_too_many](https://raw.githubusercontent.com/ucsd-cse30-f17/pa4-support/master/valgrind2.png?token=AXdWtGoX6nELPCz_0dkv1bQiXs4dooexks5aArlhwA%3D%3D) 
   * Pay attention to the HEAP SUMMARY: "in use at exit: 0 bytes in 0 blocks" and "total heap usage: 44 allocs, 46 frees, 371 bytes allocated". You want to see the same number of allocs as there are frees.
   
   * Along with the HEAP SUMMARY, you'll find the details on where exactly the memory leak occurred. In this screenshot, valgrind sees that there was an "invalid free() / delete / delete[] / realloc()" line 140 in `main.c`.

3. Segfault (which happens when you try to access memory that's off-limits)
![segfault](https://raw.githubusercontent.com/ucsd-cse30-f17/pa4-support/master/segfault.png?token=AXdWtEnGdnPtZE1CnvRCcBTbiKgHQfFZks5aArzJwA%3D%3D)
   * valgrind tells you that in this case, an "Invalid read of size 4" caused the segfault.
   
   * It's useful to look at the valgrind output here, to figure out what line caused the segfault: valgrind notes that the segfault happened "at 0x10FC8: bst_contains (bst.c:143)," which was called by "0x10687: main (main.c:29)".
   
   * It also says that the "Address 0x8 is not stack'd, malloc'd or (recently) free'd".

   * For example, trying to dereference NULL (i.e. attempting `node->left` or `node->key` when `node == NULL`) will give you a segfault. Sometimes spotting exactly where a segfault occurred can be tricky; valgrind makes it easier to pinpoint the problematic line(s) of code.
   
### 6. The Makefile

We've provided a `Makefile` for you that should make the compilation process simple. Please do not modify the Makefile, but feel free to refer to it. The `test` and `vtest` commands above are one main way you'll interact with the assignment.

The Makefile is compiling your code into an object file called `bst.o`, which you can compile on its own with:

```
    make bst.o
```

It can occasionally be useful to run `make clean`, and then run `make test` or `make vtest` afresh, if the state in your working directory behaves oddly. `make clean` deletes any built files so that the program can be rebuilt from scratch.

### 7. README

#### 1. Understanding bad implementations

Match each of the descriptions to one of the bst#.o files given to you in bad_impls/ directory. You should write good tests in part 4, and use those tests to figure out which files match up to which errors.

Format your answer as a comma-separated list of the letters (i.e. "B, C, E, D, ..."), where the first corresponds to bst1, the second to bst2, and so on.


#### 2. Describing bad implementations

Choose two of the above "incorrect implementations." Speculate on the mistake in the code that could have led to the mistake in the implementation. How might it be fixed?


### 8. Commenting and style guide
For ARM Assembly code, please refer to PA3's commenting and style guide in the [PA3 Writeup](https://github.com/ucsd-cse30-f17/pa3-support/blob/master/description.pdf).

For C code, you should write function headers (see `bst_max` for an example) and comment roughly every block of code. Inline comments are also useful if you have a particularly tricky block of code.

If you want a style to go by, use `bst_max` and the provided test as examples. Describe each input and the result in a block comment. Indent nested blocks by two spaces each. We found the body of `bst_max` to be simple enough that we didn't need any comments, but found the block comment in the header quite useful.

### 9. Handin
Commit and push the _ files to the Github repository that was created for you by 11:59pm on Thursday, November 9. You can push up to one day late for a 20% penalty. After you push, make sure to check on Github that the files are actually there. We will mark all of the student repositories for grading a few minutes after midnight and grade precisely what is there.
Your handin should include:
* `bst.c` - which should contain your working implementation of the BST data structure
* `test.c` - which should contain a good set of test cases that catch (at least) all the errors in the bad implementations of a bst that we provided for you
* `totalLength.s` – which should contain your implementation in ARM assembly of `totalLength` as specified above
* `count.s` – which should contain your implementation in ARM assembly of `count` as specified above
* A single `README.txt` file - which should contain the answers to the README questions above

Note that you do not need to turn in `.o` files, and the `.gitignore` file makes it so they should be ignored in your submission.

### 10. Grading

- 25%: Quality of tests, determined by the # of bad implementations caught, and whether they pass on the correct implementation
- 20%: README
- 20%: Assembly functions `totalLength` and `count` (includes points for code style)
- 35%: Distributed among the BST functions (includes points for code style)

Both your tests and your BST functions can get deductions if Valgrind reports that memory is in use at the end of the program, or reports other memory-related errors.
