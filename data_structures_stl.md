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
unordered_map<int, string> um;
um[1] = "one";
// Same operations as map but O(1) average time
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

## Useful Iterators
```cpp
vector<int> v = {1, 2, 3, 4, 5};

// Range-based for loop
for (int x : v) {
    cout << x << " ";
}

// Iterator loop
for (auto it = v.begin(); it != v.end(); it++) {
    cout << *it << " ";
}

// Reverse iterator
for (auto it = v.rbegin(); it != v.rend(); it++) {
    cout << *it << " ";
}
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