# Binary Search Tree


- generic binary search algorithm

consider a sorted list filtered by certain evaluator and returns the following list containing only True or False:
```
[F, F, F, T, T, T, T]
```
To find first T:
```
int lo = 0, hi = nums.size();

while(lo < hi){
    int mid = (lo + hi) / 2;
    if(evaluator(list, mid)){
        hi = mid;
    } else {
        lo = mid + 1;
    }
}
return lo;
```
To find last F:
```
int lo = 0, hi = nums.size();

while(lo < hi){
    int mid = (lo + hi + 1) / 2;
    if(evaluator(list, mid)){
        hi = mid - 1;
    } else {
        lo = mid;
    }
}
return lo;
```


## 29. Divide Two Integers
- corner cases: integer overflow
- exclusive or for determine same sign of two integers
  - ``sign = (a>0) ^ (b>0)``


## 33. Search in Rotated Sorted Array

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.empty())
            return -1;

        int lo = 0, hi = nums.size();
        
        while(lo < hi) {
            int mid = (hi + lo) / 2;
            if (nums[mid] == target)
                return mid;
            // The point here is to determine whether we should go left or right.
            // By knowing whether left or rigth side is sorted, we could easily check 
            // whether target is within the sorted side of the nums. If target is 
            // inside sorted side, we should go there, otherwise go the other side.
            else if (nums[0] < nums[mid]) {  // left side is sorted
                if (nums[0] <= target && target < nums[mid] )
                    hi = mid;
                else 
                    lo = mid + 1;
            } else {  // right side is sorted
                // Note we don't need to check if hi > 0 because 
                // we will exit while loop is hi == 0, in which case lo must == 0. 
                if (nums[mid] < target && target <= nums[hi-1])
                    lo = mid + 1;
                else 
                    hi = mid;
            }
        }
        
        return ((lo < nums.size() && nums[lo] == target) ? lo : -1);
    }
};
```


## 81. Search in Rotated Sorted Array II
```
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        if (nums.empty())
            return false;
        
        int lo = 0, hi = nums.size();
        
        while (lo < hi) {
            int mid = (lo + hi) / 2;

            if (nums[mid] == target)
                return true;
            
            // If left, rigth, mid equal, there is no way wo know where to go.
            // So simply move one step left.
            if (nums[lo] == nums[mid] && nums[mid] == nums[hi-1]) {
                hi --;
            } else if (nums[lo] <= nums[mid]) {  // Left side is sorted
                if (nums[lo] <= target && target < nums[mid])
                    hi = mid;
                else 
                    lo = mid + 1;
            } else {  // Right side is sorted
                if (nums[mid] < target && target <= nums[hi-1])
                    lo = mid + 1;
                else
                    hi = mid;
            } 
        }
        
        return (lo < nums.size() && nums[lo] == target);
    }
};
```



