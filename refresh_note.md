# algorithms:
1. Array:
    1. two pointer
    2. sliding window
    3. prefix sum
2. Hashing
3. LinkedList
4. stack (FILO) / queue (FIFO)
5. Graph:
    1. dfs
    2. bfs
    3. implicit graph
6. Heap (priority queue)
7. Greedy
8. Binary search
9. Backtracking
10. DP

# 📚 C++ `priority_queue` Summary

Why yse priority_queue .pop(): O(log(n)), add O(log(n))
Because the insert of array like structure take O(n).


## 🔹 Default Behavior

```cpp
#include <queue>

std::priority_queue<int> maxHeap;

#include <queue>
#include <vector>
#include <functional>

std::priority_queue<int, std::vector<int>, std::greater<>> minHeap;

#include <queue>
#include <tuple>
#include <vector>
#include <functional>

std::priority_queue<std::tuple<int, int, int>, 
                    std::vector<std::tuple<int, int, int>>, 
                    std::greater<>> pq; // less

maxHeap.top();

