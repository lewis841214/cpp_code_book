# C++ Basics for Python Developers

## Headers and Setup
```cpp
#include <bits/stdc++.h>  // Include everything (contest only)
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);  // Fast I/O
    
    // Your code here
    return 0;
}
```

## Data Types
```cpp
int x = 42;
long long ll = 1e18;
double d = 3.14;
char c = 'a';
string s = "hello";
bool flag = true;

// Arrays
int arr[100];
vector<int> v = {1, 2, 3};
```

## Input/Output
```cpp
// Read
int n;
cin >> n;
string s;
getline(cin, s);  // Read entire line

// Print
cout << "Hello" << endl;
cout << x << " " << y << "\n";
```

## Loops
```cpp
// For loop
for(int i = 0; i < n; i++) {
    // code
}

// Range-based (like Python for)
for(auto x : vector) {
    // code
}

// While
while(condition) {
    // code
}
```

## Functions
```cpp
int add(int a, int b) {
    return a + b;
}

// Pass by reference
void modify(vector<int>& v) {
    v.push_back(42);
}
``` 