# C++ Pointers - Fundamental Concepts

## What is a Pointer?
A pointer is a variable that stores the **memory address** of another variable, rather than storing a value directly.

```cpp
int x = 42;        // x is a regular variable containing the value 42
int* ptr = &x;     // ptr is a pointer containing the ADDRESS where x is stored

// Think of it like this:
// Memory Address:  Value:
// 0x1000          42        <- x lives here
// 0x2000          0x1000    <- ptr lives here, contains address of x
```

## Basic Pointer Operations

### 1. Declaration
```cpp
int* ptr;           // Declares a pointer to int
double* dPtr;       // Declares a pointer to double
char* cPtr;         // Declares a pointer to char

// Alternative syntax (same meaning):
int *ptr;           // * can be next to type or variable name
```

### 2. Address-of Operator (&)
```cpp
int x = 42;
int* ptr = &x;      // & gets the ADDRESS of x

cout << x;          // Prints 42 (the value)
cout << &x;         // Prints something like 0x7fff5fbff61c (the address)
cout << ptr;        // Prints the same address as &x
```

### 3. Dereference Operator (*)
```cpp
int x = 42;
int* ptr = &x;

cout << *ptr;       // * dereferences ptr, prints 42 (the value at that address)
*ptr = 100;         // Changes the value at the address (changes x to 100)
cout << x;          // Now prints 100
```

## Visual Memory Model
```cpp
int a = 10;
int b = 20;
int* p1 = &a;
int* p2 = &b;

// Memory layout:
// Address   Variable   Value
// 0x1000    a          10
// 0x1004    b          20  
// 0x2000    p1         0x1000  (points to a)
// 0x2004    p2         0x1004  (points to b)

cout << a;          // 10
cout << &a;         // 0x1000
cout << p1;         // 0x1000
cout << *p1;        // 10

*p1 = 50;           // Changes a to 50
cout << a;          // 50
```

## Pointer Arithmetic
```cpp
int arr[] = {10, 20, 30, 40, 50};
int* ptr = arr;     // Points to first element (same as &arr[0])

cout << *ptr;       // 10 (first element)
ptr++;              // Move to next element
cout << *ptr;       // 20 (second element)
ptr += 2;           // Move 2 elements forward
cout << *ptr;       // 40 (fourth element)

// Array access using pointers:
cout << *(ptr + 1); // 50 (fifth element)
cout << ptr[1];     // 50 (same as above, array notation)
```

## Dynamic Memory Allocation
```cpp
// Allocate memory on the heap
int* ptr = new int(42);     // Allocate one int, initialize to 42
int* arr = new int[5];      // Allocate array of 5 ints

cout << *ptr;               // 42
arr[0] = 10;               // Set first element
*(arr + 1) = 20;           // Set second element using pointer arithmetic

// IMPORTANT: Must free memory manually
delete ptr;                // Free single object
delete[] arr;              // Free array

// After delete, ptr and arr become "dangling pointers"
// Don't use them again!
```

## Common Pointer Operations
```cpp
// 1. Check if pointer is null
int* ptr = nullptr;
if (ptr == nullptr) {
    cout << "Pointer is null" << endl;
}

// 2. Pointer assignment
int x = 10, y = 20;
int* p = &x;        // p points to x
p = &y;             // Now p points to y

// 3. Pointer comparison
int* p1 = &x;
int* p2 = &x;
if (p1 == p2) {     // true - both point to same address
    cout << "Same address" << endl;
}

// 4. Copying through pointers
int a = 5, b = 10;
int* pa = &a;
int* pb = &b;
*pa = *pb;          // Copies value from b to a (a becomes 10)
```

## Pointers in Function Parameters
```cpp
// Pass by value (copies the value)
void func1(int x) {
    x = 100;        // Only changes local copy
}

// Pass by reference using pointers
void func2(int* x) {
    *x = 100;       // Changes the original variable
}

// Pass by reference using C++ references
void func3(int& x) {
    x = 100;        // Changes the original variable (cleaner syntax)
}

int main() {
    int num = 42;
    
    func1(num);     // num is still 42
    func2(&num);    // num becomes 100
    func3(num);     // num becomes 100 (no & needed in call)
}
```

## Pointers vs References
```cpp
int x = 10, y = 20;

// Pointer:
int* ptr = &x;      // Can point to different variables
ptr = &y;           // OK - now points to y
*ptr = 30;          // OK - changes y to 30

// Reference:
int& ref = x;       // Must be initialized, cannot be reassigned
// ref = &y;        // ERROR - cannot reassign reference
ref = 30;           // OK - changes x to 30 (no * needed)
```

## Linked List Example Explained
```cpp
struct LinkedListNode {
    int val;
    LinkedListNode* next;   // Pointer to next node
    
    // Constructor with initializer list:
    LinkedListNode(int val): val(val), next(nullptr) {}
    //        ^         ^      ^           ^         ^
    //        |         |      |           |         |
    //   Constructor  Parameter |       Initialize  Empty
    //      name               |       next to      body
    //                   Initialize     nullptr
    //                   this->val 
    //                   to parameter val
};

// Creating nodes:
LinkedListNode* one = new LinkedListNode(1);    // Calls constructor with val=1
LinkedListNode* two = new LinkedListNode(2);    // Calls constructor with val=2
LinkedListNode* three = new LinkedListNode(3);  // Calls constructor with val=3

// Linking nodes:
one->next = two;        // one's next pointer points to two's address
two->next = three;      // two's next pointer points to three's address

// The -> operator is shorthand for (*ptr).member
one->next;              // Same as (*one).next
one->next->val;         // Same as (*((*one).next)).val or (*two).val
```

## Constructor and Initializer List Explained
```cpp
// Breaking down: LinkedListNode(int val): val(val), next(nullptr) {}

// Method 1: Using initializer list (RECOMMENDED)
LinkedListNode(int val): val(val), next(nullptr) {
    // Constructor body is empty - initialization happens before this
}

// Method 2: Assignment in constructor body (less efficient)
LinkedListNode(int val) {
    this->val = val;        // Assignment (not initialization)
    this->next = nullptr;   // Assignment (not initialization)
}

// Method 3: More explicit initializer list syntax
LinkedListNode(int inputVal): val(inputVal), next(nullptr) {
    // Different parameter name to avoid confusion
}

// What happens when you call: new LinkedListNode(42)
// 1. Memory is allocated for the new node
// 2. Constructor is called with parameter val = 42
// 3. Member variables are initialized:
//    - this->val gets set to 42 (from parameter)
//    - this->next gets set to nullptr
// 4. Constructor body executes (empty in this case)
// 5. Pointer to the new node is returned
```

## Why Use Initializer Lists?
```cpp
struct Example {
    const int id;           // const member must be initialized
    int& ref;              // reference member must be initialized
    string name;           // object member
    
    // MUST use initializer list for const and reference members
    Example(int i, int& r, string n): id(i), ref(r), name(n) {
        // id = i;         // ERROR! Cannot assign to const after construction
        // ref = r;        // ERROR! Cannot reassign reference after construction
    }
    
    // Benefits of initializer list:
    // 1. More efficient (direct initialization vs assignment)
    // 2. Required for const/reference members
    // 3. Cleaner code
    // 4. Initialization order follows declaration order
};
```

## Common Constructor Patterns
```cpp
struct Node {
    int data;
    Node* next;
    
    // Default constructor
    Node(): data(0), next(nullptr) {}
    
    // Parameterized constructor
    Node(int val): data(val), next(nullptr) {}
    
    // Constructor with both parameters
    Node(int val, Node* nextNode): data(val), next(nextNode) {}
};

// Usage:
Node* n1 = new Node();          // Uses default constructor: data=0, next=nullptr
Node* n2 = new Node(42);        // Uses parameterized: data=42, next=nullptr
Node* n3 = new Node(10, n2);    // Uses both params: data=10, next=n2
```

## Common Pointer Mistakes
```cpp
// 1. Uninitialized pointer
int* ptr;           // Contains garbage address
*ptr = 5;           // CRASH! Accessing random memory

// 2. Dangling pointer
int* ptr = new int(42);
delete ptr;
*ptr = 10;          // CRASH! Using deleted memory

// 3. Memory leak
int* ptr = new int(42);
ptr = new int(100); // Lost reference to first allocation - memory leak!

// 4. Double delete
int* ptr = new int(42);
delete ptr;
delete ptr;         // CRASH! Deleting already deleted memory
```

## Best Practices
```cpp
// 1. Initialize pointers
int* ptr = nullptr;     // Always initialize

// 2. Check before dereferencing
if (ptr != nullptr) {
    *ptr = 42;
}

// 3. Set to nullptr after delete
delete ptr;
ptr = nullptr;

// 4. Use smart pointers (C++11+) for automatic memory management
#include <memory>
std::unique_ptr<int> smartPtr = std::make_unique<int>(42);
// Automatically deletes memory when smartPtr goes out of scope
```

## Memory Layout Summary
```
Stack (automatic variables):
┌─────────────┐
│ int x = 10  │ ← Local variables, function parameters
│ int* ptr    │ ← Pointer variable (stores address)
└─────────────┘

Heap (dynamic allocation):
┌─────────────┐
│ new int(42) │ ← Dynamically allocated memory
│ new Node()  │ ← Must be manually deleted
└─────────────┘
``` 