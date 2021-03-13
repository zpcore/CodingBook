
#### Question
(Median)
给一些interval和对应tag，interval可能有重合，要求输出每个小段的interval和对应的所有tag。

#### Solution
Method 1: priority_queue or map.
```c++
vector<vector<int>> overlapIntervalTag(vector<vector<int>> intervals) {
// output each vector vector<int>: [st, ed, tag1, tag2, ...] 
    sort(intervals.begin(), intervals.end());
    int n = intervals.size();
    multimap<int,int> mp; // end time, interval id
    unordered_set<int> st; // current ongoing overlapping interval id
    int pre = 0;
    vector<vector<int>> ret;
    for (int i=0; i<n;) {
        while(!mp.empty() && mp.begin()->first<=intervals[i][0]) {
            auto it = mp.begin();
            unorder_set<int> st_cpy  = st;
            while(!mp.empty() && it->first==mp.begin()->first) { // pop out the interval with same end time
                st.erase(mp.begin()->second);
                mp.erase(mp.begin());
            }
            ret.push_back({pre, it->first});
            for (int id: st_cpy) {
                ret.back().push_back(id);
            }
            pre = ret.back()[1];
            
        }

        int newSt = intervals[i][0];
        if (!st.empty()) {
            ret.push_back({pre, newSt});
            for(int id: st)
                ret.back().push_back(id);
        }
        while(i<n && intervals[i][0]==newSt) {
            mp.insert({intervals[i][1], i});
            st.insert(i);
            i++;
        }
        pre = newSt;

    }
    return ret;
}

```