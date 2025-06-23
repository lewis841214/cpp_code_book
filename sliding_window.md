# Sliding Window

## Maximum Sum Subarray of Size K (Fixed Window)
```cpp
int maxSumSubarray(vector<int>& nums, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += nums[i];
    }
    
    int maxSum = windowSum;
    for (int i = k; i < nums.size(); i++) {
        windowSum += nums[i] - nums[i - k];
        maxSum = max(maxSum, windowSum);
    }
    return maxSum;
}
```

## Longest Substring Without Repeating Characters
```cpp
int lengthOfLongestSubstring(string s) {
    unordered_set<char> window;
    int left = 0, maxLen = 0;
    
    for (int right = 0; right < s.length(); right++) {
        while (window.count(s[right])) {
            window.erase(s[left]);
            left++;
        }
        window.insert(s[right]);
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

## Minimum Window Substring
```cpp
string minWindow(string s, string t) {
    unordered_map<char, int> need, window;
    for (char c : t) need[c]++;
    
    int left = 0, right = 0;
    int valid = 0;
    int start = 0, len = INT_MAX;
    
    while (right < s.length()) {
        char c = s[right];
        right++;
        
        if (need.count(c)) {
            window[c]++;
            if (window[c] == need[c]) valid++;
        }
        
        while (valid == need.size()) {
            if (right - left < len) {
                start = left;
                len = right - left;
            }
            
            char d = s[left];
            left++;
            if (need.count(d)) {
                if (window[d] == need[d]) valid--;
                window[d]--;
            }
        }
    }
    return len == INT_MAX ? "" : s.substr(start, len);
}
```

## Longest Repeating Character Replacement
```cpp
int characterReplacement(string s, int k) {
    unordered_map<char, int> count;
    int left = 0, maxCount = 0, maxLen = 0;
    
    for (int right = 0; right < s.length(); right++) {
        count[s[right]]++;
        maxCount = max(maxCount, count[s[right]]);
        
        if (right - left + 1 - maxCount > k) {
            count[s[left]]--;
            left++;
        }
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

## Find All Anagrams in String
```cpp
vector<int> findAnagrams(string s, string p) {
    vector<int> result;
    if (s.length() < p.length()) return result;
    
    unordered_map<char, int> need, window;
    for (char c : p) need[c]++;
    
    int left = 0, right = 0, valid = 0;
    
    while (right < s.length()) {
        char c = s[right];
        right++;
        
        if (need.count(c)) {
            window[c]++;
            if (window[c] == need[c]) valid++;
        }
        
        while (right - left >= p.length()) {
            if (valid == need.size()) {
                result.push_back(left);
            }
            
            char d = s[left];
            left++;
            if (need.count(d)) {
                if (window[d] == need[d]) valid--;
                window[d]--;
            }
        }
    }
    return result;
}
```

## Subarray Product Less Than K
```cpp
int numSubarrayProductLessThanK(vector<int>& nums, int k) {
    if (k <= 1) return 0;
    
    int left = 0, product = 1, count = 0;
    
    for (int right = 0; right < nums.size(); right++) {
        product *= nums[right];
        
        while (product >= k) {
            product /= nums[left];
            left++;
        }
        
        count += right - left + 1;
    }
    return count;
}
```

## Permutation in String
```cpp
bool checkInclusion(string s1, string s2) {
    unordered_map<char, int> need, window;
    for (char c : s1) need[c]++;
    
    int left = 0, right = 0, valid = 0;
    
    while (right < s2.length()) {
        char c = s2[right];
        right++;
        
        if (need.count(c)) {
            window[c]++;
            if (window[c] == need[c]) valid++;
        }
        
        while (right - left >= s1.length()) {
            if (valid == need.size()) return true;
            
            char d = s2[left];
            left++;
            if (need.count(d)) {
                if (window[d] == need[d]) valid--;
                window[d]--;
            }
        }
    }
    return false;
}
``` 