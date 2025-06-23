# String Algorithms

## String Basics
```cpp
string s = "hello";
s.length() / s.size();    // Length
s[0];                     // Access character
s.substr(1, 3);          // Substring from index 1, length 3
s.find("ll");            // Find substring (returns pos or string::npos)
s.find('e');             // Find character
s.rfind("l");            // Find last occurrence

// String manipulation
s.push_back('!');        // Add character at end
s.pop_back();            // Remove last character
s.insert(2, "XX");       // Insert at position
s.erase(1, 2);           // Remove 2 chars starting at index 1
s.replace(0, 2, "Hi");   // Replace substring

// Convert case
transform(s.begin(), s.end(), s.begin(), ::tolower);
transform(s.begin(), s.end(), s.begin(), ::toupper);
```

## String to Number / Number to String
```cpp
// String to number
int num = stoi("123");
long long ll = stoll("123456789");
double d = stod("3.14");

// Number to string
string str = to_string(123);

// Character to digit
char c = '5';
int digit = c - '0';  // 5

// Digit to character
int d = 7;
char c = d + '0';     // '7'
```

## Palindrome Check
```cpp
bool isPalindrome(string s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s[left] != s[right]) return false;
        left++;
        right--;
    }
    return true;
}

// Ignore non-alphanumeric
bool isPalindromeAlphaNum(string s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        while (left < right && !isalnum(s[left])) left++;
        while (left < right && !isalnum(s[right])) right--;
        if (tolower(s[left]) != tolower(s[right])) return false;
        left++;
        right--;
    }
    return true;
}
```

## Anagram Check
```cpp
bool isAnagram(string s, string t) {
    if (s.length() != t.length()) return false;
    
    unordered_map<char, int> count;
    for (char c : s) count[c]++;
    for (char c : t) {
        if (--count[c] < 0) return false;
    }
    return true;
}

// Alternative: sort both strings
bool isAnagramSort(string s, string t) {
    sort(s.begin(), s.end());
    sort(t.begin(), t.end());
    return s == t;
}
```

## Longest Common Prefix
```cpp
string longestCommonPrefix(vector<string>& strs) {
    if (strs.empty()) return "";
    
    string prefix = strs[0];
    for (int i = 1; i < strs.size(); i++) {
        while (strs[i].find(prefix) != 0) {
            prefix = prefix.substr(0, prefix.length() - 1);
            if (prefix.empty()) return "";
        }
    }
    return prefix;
}
```

## String Matching (KMP Algorithm)
```cpp
vector<int> computeLPS(string pattern) {
    int m = pattern.length();
    vector<int> lps(m, 0);
    int len = 0, i = 1;
    
    while (i < m) {
        if (pattern[i] == pattern[len]) {
            lps[i++] = ++len;
        } else if (len) {
            len = lps[len - 1];
        } else {
            lps[i++] = 0;
        }
    }
    return lps;
}

vector<int> KMPSearch(string text, string pattern) {
    vector<int> result;
    vector<int> lps = computeLPS(pattern);
    int n = text.length(), m = pattern.length();
    int i = 0, j = 0;
    
    while (i < n) {
        if (text[i] == pattern[j]) {
            i++; j++;
        }
        
        if (j == m) {
            result.push_back(i - j);
            j = lps[j - 1];
        } else if (i < n && text[i] != pattern[j]) {
            if (j) j = lps[j - 1];
            else i++;
        }
    }
    return result;
}
```

## String Permutations
```cpp
void permuteString(string s, vector<string>& result, int start) {
    if (start == s.length()) {
        result.push_back(s);
        return;
    }
    
    for (int i = start; i < s.length(); i++) {
        swap(s[start], s[i]);
        permuteString(s, result, start + 1);
        swap(s[start], s[i]);  // backtrack
    }
}

vector<string> permute(string s) {
    vector<string> result;
    permuteString(s, result, 0);
    return result;
}
```

## Reverse Words in String
```cpp
string reverseWords(string s) {
    istringstream iss(s);
    string word, result;
    vector<string> words;
    
    while (iss >> word) {
        words.push_back(word);
    }
    
    for (int i = words.size() - 1; i >= 0; i--) {
        result += words[i];
        if (i > 0) result += " ";
    }
    return result;
}

// In-place reverse
string reverseWordsInPlace(string s) {
    // Remove extra spaces first
    int i = 0, j = 0;
    while (j < s.length()) {
        while (j < s.length() && s[j] == ' ') j++;
        while (j < s.length() && s[j] != ' ') s[i++] = s[j++];
        while (j < s.length() && s[j] == ' ') j++;
        if (j < s.length()) s[i++] = ' ';
    }
    s.resize(i);
    
    // Reverse entire string
    reverse(s.begin(), s.end());
    
    // Reverse each word
    int start = 0;
    for (int end = 0; end <= s.length(); end++) {
        if (end == s.length() || s[end] == ' ') {
            reverse(s.begin() + start, s.begin() + end);
            start = end + 1;
        }
    }
    return s;
}
```

## Group Anagrams
```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> groups;
    
    for (string s : strs) {
        string key = s;
        sort(key.begin(), key.end());
        groups[key].push_back(s);
    }
    
    vector<vector<string>> result;
    for (auto& group : groups) {
        result.push_back(group.second);
    }
    return result;
}
```

## Encode and Decode Strings
```cpp
string encode(vector<string>& strs) {
    string encoded = "";
    for (string s : strs) {
        encoded += to_string(s.length()) + "#" + s;
    }
    return encoded;
}

vector<string> decode(string s) {
    vector<string> result;
    int i = 0;
    
    while (i < s.length()) {
        int j = i;
        while (s[j] != '#') j++;
        
        int length = stoi(s.substr(i, j - i));
        result.push_back(s.substr(j + 1, length));
        i = j + 1 + length;
    }
    return result;
}
```

## Longest Palindromic Substring
```cpp
string longestPalindrome(string s) {
    if (s.empty()) return "";
    
    int start = 0, maxLen = 1;
    
    auto expandAroundCenter = [&](int left, int right) {
        while (left >= 0 && right < s.length() && s[left] == s[right]) {
            int len = right - left + 1;
            if (len > maxLen) {
                start = left;
                maxLen = len;
            }
            left--;
            right++;
        }
    };
    
    for (int i = 0; i < s.length(); i++) {
        expandAroundCenter(i, i);     // Odd length palindromes
        expandAroundCenter(i, i + 1); // Even length palindromes
    }
    
    return s.substr(start, maxLen);
}
```

## String Compression
```cpp
string compress(string s) {
    string result = "";
    int i = 0;
    
    while (i < s.length()) {
        char current = s[i];
        int count = 1;
        
        while (i + 1 < s.length() && s[i + 1] == current) {
            count++;
            i++;
        }
        
        result += current;
        if (count > 1) {
            result += to_string(count);
        }
        i++;
    }
    
    return result.length() < s.length() ? result : s;
}
```

## Remove Duplicates from String
```cpp
string removeDuplicates(string s) {
    unordered_set<char> seen;
    string result = "";
    
    for (char c : s) {
        if (seen.find(c) == seen.end()) {
            seen.insert(c);
            result += c;
        }
    }
    return result;
}

// Remove adjacent duplicates
string removeAdjacentDuplicates(string s) {
    stack<char> st;
    
    for (char c : s) {
        if (!st.empty() && st.top() == c) {
            st.pop();
        } else {
            st.push(c);
        }
    }
    
    string result = "";
    while (!st.empty()) {
        result = st.top() + result;
        st.pop();
    }
    return result;
}
```

## String Rotation Check
```cpp
bool isRotation(string s1, string s2) {
    if (s1.length() != s2.length()) return false;
    string doubled = s1 + s1;
    return doubled.find(s2) != string::npos;
}
```

## Regular Expression Matching (Basic)
```cpp
bool isMatch(string s, string p) {
    int m = s.length(), n = p.length();
    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
    dp[0][0] = true;
    
    // Handle patterns like a* or a*b* etc.
    for (int j = 2; j <= n; j += 2) {
        if (p[j - 1] == '*') {
            dp[0][j] = dp[0][j - 2];
        }
    }
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (p[j - 1] == '*') {
                dp[i][j] = dp[i][j - 2]; // Zero occurrence
                if (s[i - 1] == p[j - 2] || p[j - 2] == '.') {
                    dp[i][j] = dp[i][j] || dp[i - 1][j]; // One or more
                }
            } else if (s[i - 1] == p[j - 1] || p[j - 1] == '.') {
                dp[i][j] = dp[i - 1][j - 1];
            }
        }
    }
    
    return dp[m][n];
}
``` 