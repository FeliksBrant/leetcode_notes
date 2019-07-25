# Priority queue



## 253. Meeting Rooms II
```
class Solution {
public:
    // [end_time, start_time], reverse the order since priority queue 
    // compares first in pair as default
    typedef pair<int, int> interval; 
    
    int minMeetingRooms(vector<vector<int>>& intervals) {
        // Some corner cases
        if (intervals.empty())
            return 0;
        if (intervals.size() == 1)
            return 1;
        
        // First sort intervals by start time.
        sort(intervals.begin(), intervals.end());

        // Simulate number of meeting rooms in use by iterating
        // through intervals ordered by start time.
        priority_queue<interval, vector<interval>, greater<interval>> current_meetings;
        auto interval = intervals.begin();
        int max_room_in_use = 0;
        
        while (interval != intervals.end()) {
            // Simply add current meeting if no other meetings are going on.
            if (current_meetings.empty()) {
                current_meetings.push(make_pair((*interval)[1], (*interval)[0]));
                interval++;
                continue;
            }
            
            // Remove finished meetings
            int current_start_time = (*interval)[0];
            while (!current_meetings.empty() 
                   && current_meetings.top().first <= current_start_time) 
                current_meetings.pop();

            // Add current meeting to current_meetings.
            current_meetings.push(make_pair((*interval)[1], (*interval)[0]));
            
            // Update max number of room in use
            if (current_meetings.size() > max_room_in_use)
                max_room_in_use = current_meetings.size();
            
            // Go to next meeting
            interval++;
        }

        return max_room_in_use;
    }
};
```