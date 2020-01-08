# Graph Problems


## 221. Maximal Square
Better DP solution
```
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return 0;
        
        int m = matrix.size();
        int n = matrix[0].size();
        
        int maxLength = 0;
        
        for (int i = 0; i < m; ++i) maxLength = max(maxLength, matrix[i][0]-'0');
        for (int j = 0; j < n; ++j) maxLength = max(maxLength, matrix[0][j]-'0');
        
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[i][j] == '0') continue;
                
                matrix[i][j] = min(min(matrix[i-1][j-1], matrix[i-1][j]), matrix[i][j-1]) + 1;
                maxLength = max(maxLength, matrix[i][j]-'0');
            }
        }
        return maxLength * maxLength;
    }
};
```

## 1192. Critical Connections in a Network
[Bridge algorithm](https://www.youtube.com/watch?v=aZXi1unBdJA&feature=youtu.be) (also with algorithm to find articulation points, but not used for this problem)

Basically, the ideas are [[reference](https://leetcode.com/problems/critical-connections-in-a-network/discuss/468477/Clean-Readable-C%2B%2B-solution-with-comments-(Bridges-Algorithm))]:
1. Start from any node (e.g., node `0`) in the graph and do a standard DFS.
2. During DFS, we need to track two properties of each node:
    - Rank: the order in DFS to visit a node (starting node has rank `1`).
    - Low-link value; the lowest rank that a node can visit during DFS (self included).
3. After DFS is complete, any edge (`parent, node`) is identified as a bridge by the following condition:
`low-link value of node > rank of parent`,
where parent is the immediate previous node to `node` in DFS order.


```
class Solution {
private:
    vector<int> rank;
    vector<int> low;
    vector<int> visited;
    vector<vector<int>> bridges;
    vector<vector<int>> graph;
    
    void buildGraph(const vector<vector<int>>& connections) {
        for (auto& c : connections) {
            graph[c[0]].push_back(c[1]);
            graph[c[1]].push_back(c[0]);
        }
    }
    
    void dfs(int node, int parent, int& r) {
        if (visited[node]) return;
        visited[node] = 1;
        
        // update rank and initialize low-link value
        rank[node] = low[node] = r++;
        
        // update low-link value
        for (const auto &n: graph[node]) {
            if (n == parent) continue;
            
            if (!visited[n]) dfs(n, node, r);
            low[node] = min(low[n], low[node]);
        }
        
        // update bridge
        if (parent != -1 && low[node] > rank[parent])
            bridges.push_back(vector<int> {parent, node});
    }
    
public:
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        rank.resize(n, 0);
        low.resize(n, 0);
        visited.resize(n, 0);
        graph.resize(n);
        buildGraph(connections);
        
        cout << "graph\n";
        
        int r = 1;
        dfs(0, /*parent*/-1, r);
        
        return bridges;
    }
};
```