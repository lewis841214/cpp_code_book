# C++ Code Book for LeetCode Contests

A focused collection of C++ code patterns and algorithms for competitive programming, designed for developers who already know Python and want to learn C++ quickly.

## 📚 Quick Reference Files

### Core Topics
- [**C++ Basics**](./cpp_basics.md) - Syntax, I/O, loops, functions
- [**STL Data Structures**](./data_structures_stl.md) - Vector, map, set, priority_queue, etc.
- [**String Algorithms**](./string_algorithms.md) - String manipulation, matching, and algorithms
- [**Array & Vector Patterns**](./array_vector_patterns.md) - Array operations, sorting, searching, matrix algorithms
- [**Binary Search**](./binary_search.md) - All binary search patterns
- [**Two Pointers**](./two_pointers.md) - Common two-pointer techniques
- [**Sliding Window**](./sliding_window.md) - Fixed and variable window patterns
- [**Dynamic Programming**](./dynamic_programming.md) - DP patterns with optimizations
- [**Trees & Graphs**](./trees_graphs.md) - Tree traversals, graph algorithms, Union Find
- [**Linked Lists**](./linked_lists.md) - All linked list operations
- [**Bit Manipulation & Backtracking**](./bit_manipulation_backtracking.md) - Bit ops and backtracking patterns

## 🚀 Quick Start Template

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    // Your code here
    
    return 0;
}
```

## 📖 How to Use This Book

1. **Start with `cpp_basics.md`** if you're new to C++
2. **Learn STL** from `data_structures_stl.md` - this is crucial for contests
3. **Pick a topic** based on the problem you're solving
4. **Copy-paste templates** and modify as needed
5. **Practice typing** the common patterns until they become muscle memory

## 🎯 Contest Strategy

### Before Contest
- Review the templates for topics you're weak on
- Practice typing common patterns (binary search, DFS, etc.)
- Know your STL operations by heart

### During Contest
- Start with the easiest problem to build confidence
- Use `#include <bits/stdc++.h>` and templates liberally
- Don't optimize prematurely - get a working solution first
- Watch out for integer overflow (use `long long` when in doubt)

## 💡 Key Differences from Python

```cpp
// C++ is 0-indexed like Python
vector<int> v = {1, 2, 3};  // Like Python list
v.size()                    // len(v) in Python
v.push_back(4)             // v.append(4) in Python

// Strings
string s = "hello";
s.length()                  // len(s) in Python
s.substr(1, 3)             // s[1:4] in Python

// Loops
for (int i = 0; i < n; i++)        // for i in range(n)
for (auto x : vector)              // for x in vector
```

## ⚠️ Common Gotchas

- **Integer overflow**: Use `long long` for large numbers
- **Array bounds**: C++ doesn't check bounds automatically
- **Uninitialized variables**: Always initialize your variables
- **Pass by reference**: Use `&` to modify function parameters
- **Iterator invalidation**: Be careful when modifying containers while iterating

## 📊 Time Complexities (Quick Reference)

| Operation | Time | Data Structure |
|-----------|------|----------------|
| Binary Search | O(log n) | Sorted Array |
| Hash Table Ops | O(1) avg | unordered_map/set |
| Tree Ops | O(log n) | map/set |
| Heap Ops | O(log n) | priority_queue |
| Sort | O(n log n) | Any array |
| DFS/BFS | O(V + E) | Graph |

Happy coding! 🎉 