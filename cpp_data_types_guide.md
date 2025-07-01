# C++ Data Types Comprehensive Guide

## Built-in Data Types

### Fundamental Types
```cpp
// Integer types
int num = 42;                    // 32-bit signed integer
long long bigNum = 1234567890LL; // 64-bit signed integer
short smallNum = 100;            // 16-bit signed integer
char letter = 'A';               // 8-bit character/small integer

// Unsigned versions
unsigned int uNum = 42U;
unsigned long long uBigNum = 1234567890ULL;
unsigned short uSmallNum = 100;
unsigned char uLetter = 255;

// Floating point
float pi = 3.14f;               // 32-bit floating point
double precision = 3.141592653; // 64-bit floating point
long double extended = 3.14159265358979323846L; // Extended precision

// Boolean
bool flag = true;               // true or false
bool condition = false;

// Size information
cout << "int size: " << sizeof(int) << " bytes" << endl;
cout << "long long size: " << sizeof(long long) << " bytes" << endl;
cout << "double size: " << sizeof(double) << " bytes" << endl;
```

### Type Modifiers and Auto
```cpp
// Auto type deduction (C++11+)
auto x = 42;        // int
auto y = 3.14;      // double
auto z = 'A';       // char
auto w = true;      // bool

// Const and constexpr
const int CONSTANT = 100;           // Runtime constant
constexpr int COMPILE_TIME = 200;   // Compile-time constant

// References
int original = 10;
int& ref = original;    // Reference (alias) to original
ref = 20;              // Changes original to 20

// Pointers
int* ptr = &original;   // Pointer to original
*ptr = 30;             // Changes original to 30
```

## STL Sequential Containers

### 1. Array (Fixed Size)
```cpp
#include <array>

// C-style array
int cArray[5] = {1, 2, 3, 4, 5};

// STL array (recommended)
array<int, 5> arr = {1, 2, 3, 4, 5};
arr[0] = 10;                    // Access by index
arr.at(1) = 20;                 // Safe access (throws if out of bounds)
arr.front() = 1;                // First element
arr.back() = 5;                 // Last element
arr.size();                     // Always 5 (compile-time constant)
arr.empty();                    // false (never empty for non-zero size)

// Iteration
for (int& x : arr) {
    cout << x << " ";
}
```

### 2. Vector (Dynamic Array)
```cpp
#include <vector>

// Declaration and initialization
vector<int> v1;                     // Empty vector
vector<int> v2 = {1, 2, 3, 4, 5};  // Initializer list
vector<int> v3(10);                 // 10 elements, default value 0
vector<int> v4(10, 42);             // 10 elements, all set to 42
vector<int> v5(v2);                 // Copy constructor

// Common operations
v1.push_back(10);           // Add to end
v1.pop_back();              // Remove from end
v1.insert(v1.begin(), 5);   // Insert at beginning
v1.erase(v1.begin());       // Remove first element
v1.clear();                 // Remove all elements
v1.resize(20);              // Change size to 20
v1.reserve(100);            // Reserve capacity (no size change)

// Access
cout << v2[0];              // Direct access (no bounds check)
cout << v2.at(0);           // Safe access (throws if out of bounds)
cout << v2.front();         // First element
cout << v2.back();          // Last element

// Properties
cout << v2.size();          // Number of elements
cout << v2.capacity();      // Current capacity
cout << v2.empty();         // true if empty

// 2D vector
vector<vector<int>> matrix(3, vector<int>(4, 0)); // 3x4 matrix of zeros
matrix[1][2] = 42;
```

### 3. Deque (Double-Ended Queue)
```cpp
#include <deque>

deque<int> dq = {1, 2, 3, 4, 5};

// Unique operations (vs vector)
dq.push_front(0);           // Add to front (O(1))
dq.pop_front();             // Remove from front (O(1))
dq.push_back(6);            // Add to back (O(1))
dq.pop_back();              // Remove from back (O(1))

// Same as vector for most other operations
dq[0] = 10;
dq.insert(dq.begin() + 2, 42);
```

### 4. List (Doubly Linked List)
```cpp
#include <list>

list<int> lst = {1, 2, 3, 4, 5};

// Efficient insertion/deletion anywhere
lst.push_front(0);
lst.push_back(6);
lst.insert(next(lst.begin(), 2), 42); // Insert at 3rd position

// No random access (no operator[])
// Must use iterators
auto it = lst.begin();
advance(it, 2);             // Move iterator 2 positions
cout << *it;                // Access element

// List-specific operations
lst.sort();                 // Sort the list
lst.reverse();              // Reverse the list
lst.unique();               // Remove consecutive duplicates
```

### 5. Forward List (Singly Linked List)
```cpp
#include <forward_list>

forward_list<int> flst = {1, 2, 3, 4, 5};

// Only forward iteration
flst.push_front(0);         // Only front operations
// No push_back(), no size(), no back()

// Efficient insertion after a position
auto it = flst.begin();
flst.insert_after(it, 42);
```

## STL Associative Containers

### 1. Set (Ordered, Unique)
```cpp
#include <set>

set<int> s = {3, 1, 4, 1, 5}; // Becomes {1, 3, 4, 5} (sorted, unique)

s.insert(2);                // Insert element
s.erase(3);                 // Remove element
s.count(4);                 // Check existence (0 or 1)
s.find(4) != s.end();       // Alternative existence check

// Ordered operations
s.lower_bound(3);           // First element >= 3
s.upper_bound(3);           // First element > 3
```

### 2. Multiset (Ordered, Allows Duplicates)
```cpp
#include <set>

multiset<int> ms = {1, 1, 2, 2, 3, 3};

ms.insert(2);               // Now has 3 copies of 2
ms.count(2);                // Returns 3
ms.erase(ms.find(2));       // Remove only one copy of 2
```

### 3. Map (Key-Value Pairs, Ordered)
```cpp
#include <map>

map<string, int> ages;
ages["Alice"] = 25;
ages["Bob"] = 30;
ages.insert({"Charlie", 35});

// Access
cout << ages["Alice"];      // 25
cout << ages.at("Bob");     // 30 (throws if key doesn't exist)

// Check existence
if (ages.count("Alice")) {
    cout << "Alice exists";
}

// Iteration
for (const auto& [name, age] : ages) { // C++17 structured binding
    cout << name << ": " << age << endl;
}
```

### 4. Multimap (Multiple Values per Key)
```cpp
#include <map>

multimap<string, int> scores;
scores.insert({"Alice", 85});
scores.insert({"Alice", 92}); // Alice can have multiple scores

// Find all values for a key
auto range = scores.equal_range("Alice");
for (auto it = range.first; it != range.second; ++it) {
    cout << it->second << " ";
}
```

## STL Unordered Containers (Hash Tables)

### 1. Unordered Set
```cpp
#include <unordered_set>

unordered_set<int> us = {1, 2, 3, 4, 5};

// Average O(1) operations
us.insert(6);
us.erase(3);
us.count(4);                // Check existence

// No ordering guarantees
```

### 2. Unordered Map
```cpp
#include <unordered_map>

unordered_map<string, int> umap;
umap["key1"] = 100;
umap["key2"] = 200;

// Average O(1) access
cout << umap["key1"];

// Same interface as map, but no ordering
```

## STL Container Adapters

### 1. Stack (LIFO)
```cpp
#include <stack>

stack<int> st;
st.push(1);
st.push(2);
st.push(3);

cout << st.top();           // 3 (top element)
st.pop();                   // Remove top element
cout << st.size();          // 2
cout << st.empty();         // false
```

### 2. Queue (FIFO)
```cpp
#include <queue>

queue<int> q;
q.push(1);
q.push(2);
q.push(3);

cout << q.front();          // 1 (first element)
cout << q.back();           // 3 (last element)
q.pop();                    // Remove first element
```

### 3. Priority Queue (Heap)
```cpp
#include <queue>

// Max heap by default
priority_queue<int> maxHeap;
maxHeap.push(3);
maxHeap.push(1);
maxHeap.push(4);

cout << maxHeap.top();      // 4 (largest element)
maxHeap.pop();

// Min heap
priority_queue<int, vector<int>, greater<int>> minHeap;
minHeap.push(3);
minHeap.push(1);
minHeap.push(4);
cout << minHeap.top();      // 1 (smallest element)
```

## Utility Types

### 1. Pair
```cpp
#include <utility>

// Creation
pair<int, string> p1 = {1, "hello"};
pair<int, string> p2 = make_pair(2, "world");
auto p3 = make_pair(3, "auto");

// Access
cout << p1.first;           // 1
cout << p1.second;          // "hello"

// Common use with maps
map<string, int> m;
m.insert(make_pair("key", 42));

// Comparison (lexicographic)
pair<int, int> a = {1, 2};
pair<int, int> b = {1, 3};
cout << (a < b);            // true
```

### 2. Tuple (C++11+)
```cpp
#include <tuple>

// Creation
tuple<int, string, double> t1 = {1, "hello", 3.14};
tuple<int, string, double> t2 = make_tuple(2, "world", 2.71);
auto t3 = make_tuple(3, "auto", 1.41);

// Access
cout << get<0>(t1);         // 1
cout << get<1>(t1);         // "hello"
cout << get<2>(t1);         // 3.14

// C++17 structured binding
auto [num, str, flt] = t1;
cout << num << " " << str << " " << flt;

// Tuple size
constexpr size_t size = tuple_size_v<decltype(t1)>; // 3
```

## String Types

### 1. Basic String
```cpp
#include <string>

string s1 = "Hello";
string s2("World");
string s3(10, 'A');         // "AAAAAAAAAA"

// Concatenation
s1 += " ";
s1 += s2;                   // "Hello World"

// Access
cout << s1[0];              // 'H'
cout << s1.at(1);           // 'e'
s1.front() = 'h';           // "hello World"
s1.back() = '!';            // "hello Worl!"

// Substring
string sub = s1.substr(6, 5); // "Worl!"

// Find
size_t pos = s1.find("Worl"); // Position of "Worl"
if (pos != string::npos) {
    cout << "Found at position " << pos;
}

// Replace
s1.replace(0, 5, "Hi");     // "Hi Worl!"
```

### 2. Wide String
```cpp
#include <string>

wstring ws = L"Wide string";
u16string u16s = u"UTF-16 string";
u32string u32s = U"UTF-32 string";
```

## Smart Pointers (C++11+)

```cpp
#include <memory>

// Unique pointer (exclusive ownership)
unique_ptr<int> uptr = make_unique<int>(42);
cout << *uptr;              // 42
unique_ptr<int> uptr2 = std::move(uptr); // Transfer ownership

// Shared pointer (shared ownership)
shared_ptr<int> sptr1 = make_shared<int>(42);
shared_ptr<int> sptr2 = sptr1;  // Both point to same object
cout << sptr1.use_count();      // 2 (reference count)

// Weak pointer (no ownership)
weak_ptr<int> wptr = sptr1;
if (auto locked = wptr.lock()) {
    cout << *locked;            // Safe access
}
```

## Container Comparison Table

| Container | Ordered | Unique | Access | Insert/Delete | Use Case |
|-----------|---------|--------|--------|---------------|-----------|
| `array` | ✅ | ❌ | O(1) | ❌ | Fixed-size arrays |
| `vector` | ✅ | ❌ | O(1) | O(n) amortized | Dynamic arrays |
| `deque` | ✅ | ❌ | O(1) | O(1) at ends | Double-ended queue |
| `list` | ✅ | ❌ | O(n) | O(1) anywhere | Frequent insertion/deletion |
| `set` | ✅ | ✅ | O(log n) | O(log n) | Sorted unique elements |
| `multiset` | ✅ | ❌ | O(log n) | O(log n) | Sorted with duplicates |
| `map` | ✅ | ✅ keys | O(log n) | O(log n) | Key-value pairs, sorted |
| `unordered_set` | ❌ | ✅ | O(1) avg | O(1) avg | Fast unique elements |
| `unordered_map` | ❌ | ✅ keys | O(1) avg | O(1) avg | Fast key-value pairs |

## When to Use Which Container

```cpp
// Use array when:
// - Fixed size known at compile time
// - Need stack allocation
array<int, 100> fixedArray;

// Use vector when:
// - Need dynamic resizing
// - Random access required
// - Default choice for sequences
vector<int> dynamicArray;

// Use deque when:
// - Need efficient front/back operations
// - Less memory locality ok
deque<int> doubleEndedQueue;

// Use list when:
// - Frequent insertion/deletion in middle
// - No random access needed
list<int> linkedList;

// Use set when:
// - Need sorted unique elements
// - Frequent lookup operations
set<int> sortedUnique;

// Use unordered_set when:
// - Need fast lookup
// - Don't care about order
unordered_set<int> fastLookup;

// Use map when:
// - Key-value associations
// - Need sorted keys
map<string, int> sortedKeyValue;

// Use unordered_map when:
// - Key-value associations
// - Need fast access
// - Don't care about key order
unordered_map<string, int> fastKeyValue;
```

This guide covers the most important data types and containers in C++. Each has its specific use cases and performance characteristics! 