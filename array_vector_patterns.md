# Array & Vector Patterns

## Vector Basics
```cpp
vector<int> v = {1, 2, 3};
v.push_back(4);           // Add to end: O(1)
v.pop_back();             // Remove from end: O(1)
v.insert(v.begin() + 1, 5); // Insert at position: O(n)
v.erase(v.begin() + 1);   // Remove at position: O(n)
v.size();                 // Size
v.empty();                // Check if empty
v.clear();                // Clear all
v.front(), v.back();      // First and last element
v.resize(10, 0);          // Resize to 10 elements, fill with 0

// 2D vector
vector<vector<int>> grid(m, vector<int>(n, 0));
```

## Array Initialization
```cpp
// Static array
int arr[100];             // Uninitialized
int arr[100] = {0};       // All zeros
int arr[] = {1, 2, 3};    // Size inferred

// Vector initialization
vector<int> v(10);        // Size 10, default values
vector<int> v(10, 5);     // Size 10, all elements = 5
vector<int> v = {1, 2, 3}; // Initialize with values

// 2D initialization
vector<vector<int>> dp(m, vector<int>(n, -1));
```

## Sorting and Searching
```cpp
vector<int> v = {3, 1, 4, 1, 5};

// Sort
sort(v.begin(), v.end());                    // Ascending
sort(v.begin(), v.end(), greater<int>());   // Descending

// Custom sort
sort(v.begin(), v.end(), [](int a, int b) {
    return abs(a) < abs(b);  // Sort by absolute value
});

// Binary search (on sorted array)
bool found = binary_search(v.begin(), v.end(), 3);
auto it = lower_bound(v.begin(), v.end(), 3); // First >= 3
auto it2 = upper_bound(v.begin(), v.end(), 3); // First > 3

// Find elements
auto it = find(v.begin(), v.end(), 4);       // Find value
if (it != v.end()) { /* found */ }
```

## Prefix Sum
```cpp
vector<int> prefixSum(vector<int>& nums) {
    vector<int> prefix(nums.size() + 1, 0);
    for (int i = 0; i < nums.size(); i++) {
        prefix[i + 1] = prefix[i] + nums[i];
    }
    return prefix;
}

// Range sum query [left, right] inclusive
int rangeSum(vector<int>& prefix, int left, int right) {
    return prefix[right + 1] - prefix[left];
}

// 2D prefix sum
vector<vector<int>> build2DPrefix(vector<vector<int>>& matrix) {
    int m = matrix.size(), n = matrix[0].size();
    vector<vector<int>> prefix(m + 1, vector<int>(n + 1, 0));
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            prefix[i][j] = matrix[i-1][j-1] + prefix[i-1][j] + 
                          prefix[i][j-1] - prefix[i-1][j-1];
        }
    }
    return prefix;
}
```

## Array Rotation
```cpp
// Rotate array right by k positions
void rotate(vector<int>& nums, int k) {
    int n = nums.size();
    k %= n;
    reverse(nums.begin(), nums.end());
    reverse(nums.begin(), nums.begin() + k);
    reverse(nums.begin() + k, nums.end());
}

// Rotate left by k positions
void rotateLeft(vector<int>& nums, int k) {
    int n = nums.size();
    k %= n;
    reverse(nums.begin(), nums.begin() + k);
    reverse(nums.begin() + k, nums.end());
    reverse(nums.begin(), nums.end());
}
```

## Kadane's Algorithm (Maximum Subarray)
```cpp
int maxSubArray(vector<int>& nums) {
    int maxSoFar = nums[0];
    int maxEndingHere = nums[0];
    
    for (int i = 1; i < nums.size(); i++) {
        maxEndingHere = max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = max(maxSoFar, maxEndingHere);
    }
    return maxSoFar;
}

// Also return the indices
pair<int, pair<int, int>> maxSubArrayWithIndices(vector<int>& nums) {
    int maxSum = nums[0];
    int currentSum = nums[0];
    int start = 0, end = 0, tempStart = 0;
    
    for (int i = 1; i < nums.size(); i++) {
        if (currentSum < 0) {
            currentSum = nums[i];
            tempStart = i;
        } else {
            currentSum += nums[i];
        }
        
        if (currentSum > maxSum) {
            maxSum = currentSum;
            start = tempStart;
            end = i;
        }
    }
    return {maxSum, {start, end}};
}
```

## Dutch National Flag (3-way partitioning)
```cpp
void sortColors(vector<int>& nums) {
    int left = 0, right = nums.size() - 1, curr = 0;
    while (curr <= right) {
        if (nums[curr] == 0) {
            swap(nums[curr], nums[left]);
            left++;
            curr++;
        } else if (nums[curr] == 2) {
            swap(nums[curr], nums[right]);
            right--;
            // Don't increment curr
        } else {
            curr++;
        }
    }
}
```

## Find Duplicates
```cpp
// Find duplicate in array where elements are 1 to n
int findDuplicate(vector<int>& nums) {
    // Floyd's cycle detection
    int slow = nums[0];
    int fast = nums[0];
    
    // Find intersection point
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);
    
    // Find start of cycle
    fast = nums[0];
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
}

// Find all duplicates (elements appear at most twice)
vector<int> findDuplicates(vector<int>& nums) {
    vector<int> result;
    for (int i = 0; i < nums.size(); i++) {
        int index = abs(nums[i]) - 1;
        if (nums[index] < 0) {
            result.push_back(abs(nums[i]));
        } else {
            nums[index] = -nums[index];
        }
    }
    return result;
}
```

## Missing Number
```cpp
// Array contains n distinct numbers from 0 to n
int missingNumber(vector<int>& nums) {
    int n = nums.size();
    int expectedSum = n * (n + 1) / 2;
    int actualSum = accumulate(nums.begin(), nums.end(), 0);
    return expectedSum - actualSum;
}

// Using XOR
int missingNumberXOR(vector<int>& nums) {
    int missing = nums.size();
    for (int i = 0; i < nums.size(); i++) {
        missing ^= i ^ nums[i];
    }
    return missing;
}
```

## Array Intersection
```cpp
vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
    unordered_map<int, int> count;
    for (int num : nums1) count[num]++;
    
    vector<int> result;
    for (int num : nums2) {
        if (count[num] > 0) {
            result.push_back(num);
            count[num]--;
        }
    }
    return result;
}

// If arrays are sorted
vector<int> intersectSorted(vector<int>& nums1, vector<int>& nums2) {
    vector<int> result;
    int i = 0, j = 0;
    
    while (i < nums1.size() && j < nums2.size()) {
        if (nums1[i] < nums2[j]) {
            i++;
        } else if (nums1[i] > nums2[j]) {
            j++;
        } else {
            result.push_back(nums1[i]);
            i++; j++;
        }
    }
    return result;
}
```

## Next Greater Element
```cpp
vector<int> nextGreaterElement(vector<int>& nums) {
    vector<int> result(nums.size(), -1);
    stack<int> st; // Store indices
    
    for (int i = 0; i < nums.size(); i++) {
        while (!st.empty() && nums[i] > nums[st.top()]) {
            result[st.top()] = nums[i];
            st.pop();
        }
        st.push(i);
    }
    return result;
}

// Circular array version
vector<int> nextGreaterElementsCircular(vector<int>& nums) {
    int n = nums.size();
    vector<int> result(n, -1);
    stack<int> st;
    
    for (int i = 0; i < 2 * n; i++) {
        while (!st.empty() && nums[i % n] > nums[st.top()]) {
            result[st.top()] = nums[i % n];
            st.pop();
        }
        if (i < n) st.push(i);
    }
    return result;
}
```

## Product of Array Except Self
```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> result(n, 1);
    
    // Left products
    for (int i = 1; i < n; i++) {
        result[i] = result[i - 1] * nums[i - 1];
    }
    
    // Right products
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        result[i] *= right;
        right *= nums[i];
    }
    
    return result;
}
```

## Merge Intervals
```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    if (intervals.empty()) return {};
    
    sort(intervals.begin(), intervals.end());
    vector<vector<int>> result = {intervals[0]};
    
    for (int i = 1; i < intervals.size(); i++) {
        if (intervals[i][0] <= result.back()[1]) {
            // Merge
            result.back()[1] = max(result.back()[1], intervals[i][1]);
        } else {
            result.push_back(intervals[i]);
        }
    }
    return result;
}
```

## Matrix Operations
```cpp
// Transpose matrix
vector<vector<int>> transpose(vector<vector<int>>& matrix) {
    int m = matrix.size(), n = matrix[0].size();
    vector<vector<int>> result(n, vector<int>(m));
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            result[j][i] = matrix[i][j];
        }
    }
    return result;
}

// Rotate matrix 90 degrees clockwise
void rotate90(vector<vector<int>>& matrix) {
    int n = matrix.size();
    
    // Transpose
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            swap(matrix[i][j], matrix[j][i]);
        }
    }
    
    // Reverse each row
    for (int i = 0; i < n; i++) {
        reverse(matrix[i].begin(), matrix[i].end());
    }
}

// Spiral traversal
vector<int> spiralOrder(vector<vector<int>>& matrix) {
    vector<int> result;
    if (matrix.empty()) return result;
    
    int top = 0, bottom = matrix.size() - 1;
    int left = 0, right = matrix[0].size() - 1;
    
    while (top <= bottom && left <= right) {
        // Right
        for (int j = left; j <= right; j++) {
            result.push_back(matrix[top][j]);
        }
        top++;
        
        // Down
        for (int i = top; i <= bottom; i++) {
            result.push_back(matrix[i][right]);
        }
        right--;
        
        // Left
        if (top <= bottom) {
            for (int j = right; j >= left; j--) {
                result.push_back(matrix[bottom][j]);
            }
            bottom--;
        }
        
        // Up
        if (left <= right) {
            for (int i = bottom; i >= top; i--) {
                result.push_back(matrix[i][left]);
            }
            left++;
        }
    }
    return result;
}
```

## Longest Consecutive Sequence
```cpp
int longestConsecutive(vector<int>& nums) {
    unordered_set<int> numSet(nums.begin(), nums.end());
    int longest = 0;
    
    for (int num : nums) {
        if (numSet.find(num - 1) == numSet.end()) { // Start of sequence
            int currentNum = num;
            int currentLen = 1;
            
            while (numSet.find(currentNum + 1) != numSet.end()) {
                currentNum++;
                currentLen++;
            }
            longest = max(longest, currentLen);
        }
    }
    return longest;
}
```

## Quick Select (Kth Largest Element)
```cpp
int findKthLargest(vector<int>& nums, int k) {
    return quickSelect(nums, 0, nums.size() - 1, nums.size() - k);
}

int quickSelect(vector<int>& nums, int left, int right, int k) {
    if (left == right) return nums[left];
    
    int pivotIndex = partition(nums, left, right);
    
    if (k == pivotIndex) {
        return nums[k];
    } else if (k < pivotIndex) {
        return quickSelect(nums, left, pivotIndex - 1, k);
    } else {
        return quickSelect(nums, pivotIndex + 1, right, k);
    }
}

int partition(vector<int>& nums, int left, int right) {
    int pivot = nums[right];
    int i = left;
    
    for (int j = left; j < right; j++) {
        if (nums[j] <= pivot) {
            swap(nums[i], nums[j]);
            i++;
        }
    }
    swap(nums[i], nums[right]);
    return i;
}
``` 