# String Problems


## 5. Longest Palindromic Substring
```
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.empty()) return string();
        
        // find center and length of best palindrome
        int bestL=0, bestC=0;
        for(int i = 0; i < s.size(); ++i) {
            int l = findPalindrome(s, i);
            if(l > bestL) {
                bestL = l;
                bestC = i;
            }
        }
        
        return s.substr(bestC - bestL / 2, bestL);
    }
    
    // find palindrome and return its length
    int findPalindrome(string& s, int center) {
        int oddL=1, evenL=0;
        
        // try odd
        for (int j = 1; center + j < s.size() && center - j >= 0; ++j) {
            if (s[center + j] != s[center - j]) break;
            oddL += 2;
        }
        
        // try even
        for (int j = 1; center - j >= 0 && center + j - 1 < s.size(); ++j) {
            if (s[center - j] != s[center + j - 1]) break;
            evenL += 2;
        }
        
        return (evenL > oddL) ? evenL :  oddL;
    }
};
```
