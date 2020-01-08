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

