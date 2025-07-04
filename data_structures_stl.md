# C++ STL Data Structures

## Vector (Dynamic Array)
```cpp
vector<int> v = {1, 2, 3};
v.push_back(4);           // Add to end
v.pop_back();             // Remove from end
v.insert(v.begin() + 1, 5); // Insert at position
v.erase(v.begin() + 1);   // Remove at position
v.size();                 // Size
v.empty();                // Check if empty
v.clear();                // Clear all
v[0];                     // Access by index
v.front(), v.back();      // First and last element

// 2D vector
vector<vector<int>> grid(m, vector<int>(n, 0));
```

## String
```cpp
string s = "hello";
s += " world";            // Concatenate
s.length() / s.size();    // Length
s.substr(1, 3);          // Substring from index 1, length 3
s.find("ll");            // Find substring (returns position or string::npos)
s.replace(0, 2, "Hi");   // Replace substring
s.push_back('!');        // Add character
s.pop_back();            // Remove last character

// String to number
int num = stoi("123");
long long ll = stoll("123456789");
double d = stod("3.14");

// Number to string
string str = to_string(123);
```

## Set & Multiset
```cpp
set<int> s;
s.insert(5);             // Insert element
s.erase(5);              // Remove element
s.count(5);              // Check if exists (0 or 1)
s.find(5);               // Returns iterator (s.end() if not found)
s.lower_bound(5);        // First element >= 5
s.upper_bound(5);        // First element > 5
s.size();                // Number of elements

// "Pop" operations (get and remove element)
// Method 1: Get first element and remove it
if (!s.empty()) {
    int first = *s.begin();  // * is dereference operator - gets the VALUE that the iterator points to
    s.erase(s.begin());      // Remove it
}

// Method 2: Get last element and remove it
if (!s.empty()) {
    auto it = s.end();
    --it;                    // Move to last element
    int last = *it;          // Get last (largest) element
    s.erase(it);             // Remove it
}

// Method 3: Using extract() (C++17) - removes and returns node
if (!s.empty()) {
    auto node = s.extract(s.begin());  // Extract first element
    int value = node.value();          // Get the value
    // node is automatically destroyed, element is removed from set
}

// Get any element when you know there's only one
if (s.size() == 1) {
    int onlyElement = *s.begin();
    s.clear();  // or s.erase(s.begin());
}

// Multiset allows duplicates
multiset<int> ms;
ms.insert(5);
ms.insert(5);            // Can insert same element multiple times
ms.count(5);             // Returns frequency
```

## Map & Unordered_map
```cpp
map<string, int> m;
m["key"] = 10;           // Insert/update
m.count("key");          // Check if exists
m.find("key");           // Returns iterator
m.erase("key");          // Remove key

// Iterate
for (auto& p : m) {
    cout << p.first << " " << p.second << endl;
}

// Unordered_map (hash table) - faster average case
/ Declaration: C++ supports multiple implementations, but we will be using
// std::unordered_map. Specify the data type of the keys and values.
unordered_map<int, int> hashMap;

// If you want to initialize it with some key value pairs, use the following syntax:
unordered_map<int, int> hashMap = {{1, 2}, {5, 3}, {7, 2}};

// Checking if a key exists: use the following syntax:
hashMap.contains(1); // true (1)
hashMap.contains(9); // false (0)

// Accessing a value given a key: use square brackets, similar to an array.
hashMap[5]; // 3

// Note: if you were to access a key that does not exist, it creates the key with a default value of 0.
hashMap[342]; // 0

// Adding or updating a key: use square brackets, similar to an array.
// If the key already exists, the value will be updated
hashMap[5] = 6;

// If the key doesn't exist yet, the key value pair will be inserted
hashMap[9] = 15;

// Deleting a key: use the .erase() method.
hashMap.erase(9);

// Get size
hashMap.size(); // 3

// Iterate over the key value pairs: use the following code.
// .first gets the key and .second gets the value.
for (auto const& pair: hashMap) {
    cout << pair.first << " " << pair.second << endl;
}
```

## Priority Queue (Heap)
```cpp
// Max heap by default
priority_queue<int> maxHeap;
maxHeap.push(5);
maxHeap.push(10);
maxHeap.push(1);
int top = maxHeap.top(); // 10
maxHeap.pop();

// Min heap
priority_queue<int, vector<int>, greater<int>> minHeap;

// Custom comparator for pairs (min by first element)
auto cmp = [](pair<int,int> a, pair<int,int> b) {
    return a.first > b.first;
};
priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq(cmp);
```

## Stack & Queue
```cpp
// Stack (LIFO)
stack<int> st;
st.push(5);
st.push(10);
int top = st.top();      // 10
st.pop();
st.empty();

// Queue (FIFO)
queue<int> q;
q.push(5);
q.push(10);
int front = q.front();   // 5
int back = q.back();     // 10
q.pop();
q.empty();

// Deque (double-ended queue)
deque<int> dq;
dq.push_front(1);
dq.push_back(2);
dq.pop_front();
dq.pop_back();
```

## Pair & Tuple
```cpp
pair<int, string> p = {1, "hello"};
p.first;                 // 1
p.second;               // "hello"
make_pair(1, "hello");  // Alternative creation

// Tuple
tuple<int, string, double> t = {1, "hello", 3.14};
get<0>(t);              // 1
get<1>(t);              // "hello"

// Structured binding (C++17)
auto [x, y, z] = t;
```

## Algorithms
```cpp
vector<int> v = {3, 1, 4, 1, 5};

// Sort
sort(v.begin(), v.end());                    // Ascending
sort(v.begin(), v.end(), greater<int>());   // Descending

// Binary search (on sorted array)
bool found = binary_search(v.begin(), v.end(), 3);
auto it = lower_bound(v.begin(), v.end(), 3);
auto it2 = upper_bound(v.begin(), v.end(), 3);

// Min/Max
int minVal = *min_element(v.begin(), v.end());
int maxVal = *max_element(v.begin(), v.end());

// Sum
int sum = accumulate(v.begin(), v.end(), 0);

// Reverse
reverse(v.begin(), v.end());

// Unique (removes consecutive duplicates, array must be sorted)
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());

// Next/Previous permutation
next_permutation(v.begin(), v.end());
prev_permutation(v.begin(), v.end());
```

## Vector Iteration Methods (Best Practices)
```cpp
vector<int> arr = {1, 2, 3, 4, 5};

// Method 1: Range-based for loop (RECOMMENDED for read-only)
// Pro: Clean, safe, no bounds checking needed
for (int x : arr) {
    cout << x << " ";  // x is a copy, can't modify original
}

// Method 2: Range-based with reference (RECOMMENDED for modifications)
// Pro: No copying, can modify elements, clean syntax
for (int& x : arr) {
    x *= 2;           // Modifies original element
    cout << x << " ";
}

// Method 3: Range-based with const reference (RECOMMENDED for large objects)
// Pro: No copying, prevents accidental modification
for (const int& x : arr) {
    cout << x << " ";  // Can't modify x
}

// Method 4: Index-based loop (RECOMMENDED when you need the index)
// Pro: You get both index and value
for (size_t i = 0; i < arr.size(); i++) {
    cout << "arr[" << i << "] = " << arr[i] << endl;
    arr[i] *= 2;      // Can modify
}

// Method 5: Iterator loop (when you need iterator operations)
// Pro: Works with all STL containers, more flexible
for (auto it = arr.begin(); it != arr.end(); ++it) {
    cout << *it << " ";  // * dereferences iterator to get value
    *it *= 2;            // Can modify through iterator
}

// Method 6: Reverse iteration
for (auto it = arr.rbegin(); it != arr.rend(); ++it) {
    cout << *it << " ";  // Prints in reverse order
}

// Method 7: Using algorithms (functional approach)
// Pro: Expressive, can be parallelized
#include <algorithm>
std::for_each(arr.begin(), arr.end(), [](int& x) {
    x *= 2;
    cout << x << " ";
});

// PERFORMANCE TIP: Use ++it instead of it++ for iterators
// ++it is slightly faster as it doesn't create a temporary copy
```

## When to Use Each Method
```cpp
// Use range-based for loop when:
// - You just need to read/process each element
// - You don't need the index
// - Code readability is important
for (const auto& element : arr) { /* process element */ }

// Use index-based loop when:
// - You need the index for logic
// - You're working with multiple arrays simultaneously
// - You need to skip elements or jump around
for (size_t i = 0; i < arr.size(); i++) { /* use arr[i] and i */ }

// Use iterators when:
// - You need to insert/erase during iteration
// - You're writing generic code that works with different containers
// - You need advanced iterator operations
for (auto it = arr.begin(); it != arr.end(); ++it) { /* use *it */ }
```

## Understanding the dereference operator (*)
```cpp
vector<int> nums = {10, 20, 30};
auto it = nums.begin();     // it is an iterator (like a pointer)
cout << it;                 // This would print the iterator itself (memory address-like)
cout << *it;                // This prints 10 (the VALUE the iterator points to)

// Without *, you get the iterator; with *, you get the value
int value = *it;            // value = 10
it++;                       // Move iterator to next element
int nextValue = *it;        // nextValue = 20
```

## Custom Comparators
```cpp
// Sort by absolute value
sort(v.begin(), v.end(), [](int a, int b) {
    return abs(a) < abs(b);
});

// Sort pairs by second element
vector<pair<int, int>> pairs = {{1, 3}, {2, 1}, {3, 2}};
sort(pairs.begin(), pairs.end(), [](auto a, auto b) {
    return a.second < b.second;
});

// Custom set comparator
auto cmp = [](int a, int b) { return abs(a) < abs(b); };
set<int, decltype(cmp)> customSet(cmp);
``` 