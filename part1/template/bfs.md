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
```