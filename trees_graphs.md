# Trees & Graphs

## TreeNode Definition
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};
```

## Binary Tree Traversals
```cpp
// Inorder (Left -> Root -> Right)
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> result;
    if (!root) return result;
    
    stack<TreeNode*> st;
    TreeNode* curr = root;
    
    while (curr || !st.empty()) {
        while (curr) {
            st.push(curr);
            curr = curr->left;
        }
        curr = st.top();
        st.pop();
        result.push_back(curr->val);
        curr = curr->right;
    }
    return result;
}

// Preorder (Root -> Left -> Right)
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> result;
    if (!root) return result;
    
    stack<TreeNode*> st;
    st.push(root);
    
    while (!st.empty()) {
        TreeNode* node = st.top();
        st.pop();
        result.push_back(node->val);
        
        if (node->right) st.push(node->right);
        if (node->left) st.push(node->left);
    }
    return result;
}

// Level Order (BFS)
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        int size = q.size();
        vector<int> level;
        
        for (int i = 0; i < size; i++) {
            TreeNode* node = q.front();
            q.pop();
            level.push_back(node->val);
            
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        result.push_back(level);
    }
    return result;
}
```

## Tree Height & Diameter
```cpp
int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}

int diameterOfBinaryTree(TreeNode* root) {
    int maxDiam = 0;
    
    function<int(TreeNode*)> height = [&](TreeNode* node) -> int {
        if (!node) return 0;
        int left = height(node->left);
        int right = height(node->right);
        maxDiam = max(maxDiam, left + right);
        return 1 + max(left, right);
    };
    
    height(root);
    return maxDiam;
}
```

## Validate BST
```cpp
bool isValidBST(TreeNode* root) {
    function<bool(TreeNode*, long, long)> validate = [&](TreeNode* node, long minVal, long maxVal) -> bool {
        if (!node) return true;
        if (node->val <= minVal || node->val >= maxVal) return false;
        return validate(node->left, minVal, node->val) && 
               validate(node->right, node->val, maxVal);
    };
    
    return validate(root, LONG_MIN, LONG_MAX);
}
```

## Lowest Common Ancestor
```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;
    
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    
    if (left && right) return root;
    return left ? left : right;
}
```

## Graph Representations
```cpp
// Adjacency List
vector<vector<int>> adj;

// For weighted graphs
vector<vector<pair<int, int>>> weightedAdj;

// Initialize
void initGraph(int n) {
    adj.resize(n);
    // weightedAdj.resize(n);
}

// Add edge
void addEdge(int u, int v) {
    adj[u].push_back(v);
    adj[v].push_back(u);  // For undirected
}

void addWeightedEdge(int u, int v, int weight) {
    weightedAdj[u].push_back({v, weight});
    weightedAdj[v].push_back({u, weight});  // For undirected
}
```

## DFS & BFS
```cpp
// DFS
void dfs(int node, vector<bool>& visited) {
    visited[node] = true;
    // Process node
    
    for (int neighbor : adj[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor, visited);
        }
    }
}

// BFS
void bfs(int start) {
    vector<bool> visited(adj.size(), false);
    queue<int> q;
    
    visited[start] = true;
    q.push(start);
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        // Process node
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}
```

## Topological Sort (Kahn's Algorithm)
```cpp
vector<int> topologicalSort(int n, vector<vector<int>>& edges) {
    vector<vector<int>> adj(n);
    vector<int> indegree(n, 0);
    
    for (auto& edge : edges) {
        adj[edge[0]].push_back(edge[1]);
        indegree[edge[1]]++;
    }
    
    queue<int> q;
    for (int i = 0; i < n; i++) {
        if (indegree[i] == 0) q.push(i);
    }
    
    vector<int> result;
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        result.push_back(node);
        
        for (int neighbor : adj[node]) {
            indegree[neighbor]--;
            if (indegree[neighbor] == 0) {
                q.push(neighbor);
            }
        }
    }
    
    return result.size() == n ? result : vector<int>();
}
```

## Dijkstra's Algorithm
```cpp
vector<int> dijkstra(int start, int n) {
    vector<int> dist(n, INT_MAX);
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    
    dist[start] = 0;
    pq.push({0, start});
    
    while (!pq.empty()) {
        int d = pq.top().first;
        int u = pq.top().second;
        pq.pop();
        
        if (d > dist[u]) continue;
        
        for (auto& edge : weightedAdj[u]) {
            int v = edge.first;
            int weight = edge.second;
            
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
```

## Union Find (Disjoint Set)
```cpp
class UnionFind {
private:
    vector<int> parent, rank;
    int components;

public:
    UnionFind(int n) : parent(n), rank(n, 0), components(n) {
        iota(parent.begin(), parent.end(), 0);
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);  // Path compression
        }
        return parent[x];
    }
    
    bool unite(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return false;
        
        // Union by rank
        if (rank[px] < rank[py]) swap(px, py);
        parent[py] = px;
        if (rank[px] == rank[py]) rank[px]++;
        
        components--;
        return true;
    }
    
    bool connected(int x, int y) {
        return find(x) == find(y);
    }
    
    int getComponents() { return components; }
};
``` 