## Bidirectional BFS
---

Return the distance between two node n1 and n2. If unreachable, return -1.
```c++
int bbfs(int n1, int n2, unordered_map<int,vector<int>>& g) {
    if (n1==n2)
        return 0;
    unordered_set<int> q1, q2, visited, tmp;
    q1.insert(n1);
    q2.insert(n2);
    int ret = 0;
    while(!q1.empty() && !q2.empty()) {
        if (q1.size()>q2.size()) // use the small branch to search
            swap(q1, q2);        
        ret ++;
        for (int cur: q1) {
            for (int nxt: g[cur]) {
                if (q2.find(nxt)!=q2.end())
                    return ret;
                tmp.insert(nxt);
            }
        }
        swap(q1, tmp);
        tmp.clear();
    }
    return -1;
}


vector<int> getOrigArray(vector<int> nums) {
    int n = nums.size();
    unordered_map<int, int> d2c;
    for (int i: nums)
        d2c++;
    vector<int> ret;
    for (int i: nums) {
        // build the chain
        int cur = i;
        while(cur%2==0 && d2c.find(cur)!=d2c.end()) {
            cur = cur/2;
        }
        if (cur==i)
            continue;
        // now cur is the last element in the chain. It's guaranteed cur is not a double of any number in nums (belongs to the original array)
        // eliminate elements in the chain into ret
        for (; cur<i; cur<<=1) {
            if (d2c.find(cur)==d2c.end())
                continue;
            int usedAsOrig = d2c[cur];
            d2c.erase(cur);
            ret.insert(ret.end(), usedAsOrig, cur);
            if (cur==i)
                break;
            d2c[2*cur]-=usedAsOrig;
        }
        
        if (d2c.size()==0)
            break;
    }
    return ret;
}
```