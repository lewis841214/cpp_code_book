# Bit Manipulation & Backtracking

## Bit Manipulation Basics
```cpp
// Basic operations
int x = 5;  // 101 in binary
x & 1;      // Check if odd (last bit)
x | 1;      // Set last bit to 1
x ^ 1;      // Flip last bit
x << 1;     // Left shift (multiply by 2)
x >> 1;     // Right shift (divide by 2)
~x;         // Bitwise NOT

// Check if power of 2
bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}

// Count set bits
int countBits(int n) {
    int count = 0;
    while (n) {
        count++;
        n &= (n - 1);  // Remove rightmost set bit
    }
    return count;
    // Or use: __builtin_popcount(n)
}

// Get/Set/Clear specific bit
bool getBit(int num, int i) {
    return (num & (1 << i)) != 0;
}

int setBit(int num, int i) {
    return num | (1 << i);
}

int clearBit(int num, int i) {
    return num & ~(1 << i);
}

// XOR properties
// a ^ a = 0
// a ^ 0 = a
// Single number in array where all others appear twice:
int singleNumber(vector<int>& nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```

## Subset Generation (Bitmask)
```cpp
// Generate all subsets using bitmask
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> result;
    int n = nums.size();
    
    for (int mask = 0; mask < (1 << n); mask++) {
        vector<int> subset;
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) {
                subset.push_back(nums[i]);
            }
        }
        result.push_back(subset);
    }
    return result;
}

// Iterate through all submasks of a mask
void iterateSubmasks(int mask) {
    for (int submask = mask; submask > 0; submask = (submask - 1) & mask) {
        // Process submask
    }
}
```

## Backtracking - Permutations
```cpp
// Generate all permutations
void permute(vector<int>& nums, vector<vector<int>>& result, int start) {
    if (start == nums.size()) {
        result.push_back(nums);
        return;
    }
    
    for (int i = start; i < nums.size(); i++) {
        swap(nums[start], nums[i]);
        permute(nums, result, start + 1);
        swap(nums[start], nums[i]);  // backtrack
    }
}

vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> result;
    permute(nums, result, 0);
    return result;
}
```

## Backtracking - Combinations
```cpp
// Generate all combinations of size k
void combine(int n, int k, vector<vector<int>>& result, vector<int>& current, int start) {
    if (current.size() == k) {
        result.push_back(current);
        return;
    }
    
    for (int i = start; i <= n; i++) {
        current.push_back(i);
        combine(n, k, result, current, i + 1);
        current.pop_back();  // backtrack
    }
}

vector<vector<int>> combine(int n, int k) {
    vector<vector<int>> result;
    vector<int> current;
    combine(n, k, result, current, 1);
    return result;
}
```

## Backtracking - Subsets with Duplicates
```cpp
void subsetsWithDup(vector<int>& nums, vector<vector<int>>& result, 
                   vector<int>& current, int start) {
    result.push_back(current);
    
    for (int i = start; i < nums.size(); i++) {
        if (i > start && nums[i] == nums[i-1]) continue; // Skip duplicates
        current.push_back(nums[i]);
        subsetsWithDup(nums, result, current, i + 1);
        current.pop_back();
    }
}

vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> result;
    vector<int> current;
    subsetsWithDup(nums, result, current, 0);
    return result;
}
```

## N-Queens Problem
```cpp
bool isSafe(vector<string>& board, int row, int col, int n) {
    // Check column
    for (int i = 0; i < row; i++) {
        if (board[i][col] == 'Q') return false;
    }
    
    // Check diagonal
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q') return false;
    }
    
    // Check anti-diagonal
    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
        if (board[i][j] == 'Q') return false;
    }
    
    return true;
}

void solveNQueens(vector<vector<string>>& result, vector<string>& board, 
                 int row, int n) {
    if (row == n) {
        result.push_back(board);
        return;
    }
    
    for (int col = 0; col < n; col++) {
        if (isSafe(board, row, col, n)) {
            board[row][col] = 'Q';
            solveNQueens(result, board, row + 1, n);
            board[row][col] = '.';  // backtrack
        }
    }
}

vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> result;
    vector<string> board(n, string(n, '.'));
    solveNQueens(result, board, 0, n);
    return result;
}
```

## Sudoku Solver
```cpp
bool isValid(vector<vector<char>>& board, int row, int col, char num) {
    // Check row
    for (int j = 0; j < 9; j++) {
        if (board[row][j] == num) return false;
    }
    
    // Check column
    for (int i = 0; i < 9; i++) {
        if (board[i][col] == num) return false;
    }
    
    // Check 3x3 box
    int startRow = (row / 3) * 3;
    int startCol = (col / 3) * 3;
    for (int i = startRow; i < startRow + 3; i++) {
        for (int j = startCol; j < startCol + 3; j++) {
            if (board[i][j] == num) return false;
        }
    }
    
    return true;
}

bool solveSudoku(vector<vector<char>>& board) {
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            if (board[i][j] == '.') {
                for (char num = '1'; num <= '9'; num++) {
                    if (isValid(board, i, j, num)) {
                        board[i][j] = num;
                        if (solveSudoku(board)) return true;
                        board[i][j] = '.';  // backtrack
                    }
                }
                return false;
            }
        }
    }
    return true;
}
```

## Word Search
```cpp
bool exist(vector<vector<char>>& board, string word) {
    int m = board.size(), n = board[0].size();
    
    function<bool(int, int, int)> dfs = [&](int i, int j, int k) -> bool {
        if (k == word.length()) return true;
        if (i < 0 || i >= m || j < 0 || j >= n || board[i][j] != word[k]) {
            return false;
        }
        
        char temp = board[i][j];
        board[i][j] = '#';  // Mark as visited
        
        bool found = dfs(i+1, j, k+1) || dfs(i-1, j, k+1) || 
                     dfs(i, j+1, k+1) || dfs(i, j-1, k+1);
        
        board[i][j] = temp;  // backtrack
        return found;
    };
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (dfs(i, j, 0)) return true;
        }
    }
    return false;
}
```

## Palindrome Partitioning
```cpp
bool isPalindrome(string s, int start, int end) {
    while (start < end) {
        if (s[start] != s[end]) return false;
        start++;
        end--;
    }
    return true;
}

void partition(string s, vector<vector<string>>& result, 
              vector<string>& current, int start) {
    if (start == s.length()) {
        result.push_back(current);
        return;
    }
    
    for (int end = start; end < s.length(); end++) {
        if (isPalindrome(s, start, end)) {
            current.push_back(s.substr(start, end - start + 1));
            partition(s, result, current, end + 1);
            current.pop_back();  // backtrack
        }
    }
}

vector<vector<string>> partition(string s) {
    vector<vector<string>> result;
    vector<string> current;
    partition(s, result, current, 0);
    return result;
}
``` 