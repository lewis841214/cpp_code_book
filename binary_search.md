# Binary Search

## Basic Binary Search
```cpp
int binarySearch(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;  // Avoid overflow
        if (nums[mid] == target) return mid;
        if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

## Lower Bound (First occurrence)
```cpp
int lowerBound(vector<int>& nums, int target) {
    int left = 0, right = nums.size();
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) left = mid + 1;
        else right = mid;
    }
    return left;
}
```

## Upper Bound (First greater than target)
```cpp
int upperBound(vector<int>& nums, int target) {
    int left = 0, right = nums.size();
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) left = mid + 1;
        else right = mid;
    }
    return left;
}
```

## Binary Search on Answer
```cpp
bool canAchieve(vector<int>& nums, int target) {
    // Check if we can achieve target
    return true; // placeholder
}

int binarySearchAnswer(vector<int>& nums) {
    int left = 0, right = 1e9;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (canAchieve(nums, mid)) right = mid;
        else left = mid + 1;
    }
    return left;
}
```

## Search in Rotated Array
```cpp
int searchRotated(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) return mid;
        
        if (nums[left] <= nums[mid]) { // Left half sorted
            if (nums[left] <= target && target < nums[mid])
                right = mid - 1;
            else left = mid + 1;
        } else { // Right half sorted
            if (nums[mid] < target && target <= nums[right])
                left = mid + 1;
            else right = mid - 1;
        }
    }
    return -1;
}
``` 