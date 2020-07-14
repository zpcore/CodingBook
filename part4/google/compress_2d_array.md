## Compress 2d Array
---
(Key word: topological sort, priority queue)  

Given a 2D array of distinct integers with n numbers, "compress" the array such that the resulting array's numbers are in the range [1, n] and their relative order is kept.  
Relative order means that if a number at position (row1, col1) is smaller than a number at position (row2, col2) in the original array, it should still have that relationship in the resulting array.  

```
Example 1:
Input:
7 6
4 9

Output:
3 2
1 4
```

Follow-up:  
Compress the array to make the numbers as small as possible as long as the relative order holds true for numbers in the same row and column.  

```
Example 1:
Input:
7 6
4 9

Output:
2 1
1 2

Example 2:
Input:
25 74 54
12 56 83

Output:
2 4 3
1 2 4

Example 3:
Input:
20 80 60 70
11 90 22 44
33 99 49 88

Output:
2 7 5 6
1 8 2 3
3 9 4 7
```

###Solution  

Flatten the whole array, sort and map from 1.
```c++
vector<vector<int>> compress2dArray(vector<vector<int>>& vec) {
    vector<vector<int>> ans;
    vector<int> flat;
    for (const auto& v:vec) {
        for (int i:v) {
            flat.push_back(i);
        }
    }
    sort(flat.begin(), flat.end());
    int j=0;
    int pre = 0;
    unordered_map<int, int> n2r;
    for (int i:flat) {
        if (i>pre)
            ++j;
        n2r[i] = j;
    }
    for (const auto& v:vec) {
        ans.push_back(vector<int>());
        for (int i:v) {
            ans[i].push_back(n2r[i]);
        }
    }
    return ans;
}
```

Follow-up:  
###Solution 1  
Build the ordering of the whole matrix elements, then deal the matrix from small to big. Meanwhile, keep record of the biggest number each row and each column. Time complexity O(r\*c\*log(r\*c)).
```c++
struct Comp {
    const vector<vector<int>>& _m;
    Comp(const vector<vector<int>>& m):_m(m){}
    Comp(const vector<vector<int>>&&) = delete; //prevents rvalue binding (unnecessary)
    bool operator()(const vector<int>& v1, const vector<int>& v2) {
        return _m[v1[0]][v1[1]] > _m[v2[0]][v2[1]]; // sort from big to small, small is the head to pop
    }
};

vector<vector<int>> compress2dArray(vector<vector<int>>& vec) {
    int m = vec.size(), n = vec[0].size();
    vector<vector<int>> ans(m, vector<int>(n));
    priority_queue<vector<int>, vector<vector<int>>, Comp> pq{Comp(vec)};
    for (int i=0; i<m; i++) {        
        for (int j=0; j<n; j++) {
            pq.push({i,j});
        }
    }
    vector<int> rowMax(m); // max of each row
    vector<int> colMax(n); //max of each column
    int cur = 0;
    while(!pq.empty()) {
        vector<int> top = pq.top();
        pq.pop();
        int r = top[0], c = top[1];
        int mr = rowMax[r], mc = colMax[c];
        int res = max(mr, mc)+1;
        ans[r][c] = res;
        rowMax[r] = res;
        colMax[c] = res;
    }
    return ans;
}
```
###Solution 2
The whole graph can be constructed into a DAG, where the priority of element element is decided by its row and column.
```c++
vector<vector<int>> compress2dArray(vector<vector<int>>& vec) {
    int m = vec.size(), n = vec[0].size();
    vector<vector<int>> ans(m, vector<int>(n));
    // build the DAG
    vector<vector<int>> al(m*n); // parent -> child, small -> big
    // build DAG from each row
    for (int i=0; i<m; i++) {
        vector<pair<int,int>> val_pos; // <val, pos>
        for (int z=0; z<n; z++) {
            val_pos.emplace_back(vec[i][z],i*n+z);
        }
        sort(val_pos.begin(),val_pos.end(),[](const pair<int,int>& p1, const pair<int,int>& p2){return p1.first<p2.first;});
        for (int z=0; z<n-1; z++) {
            auto p = val_pos[z];
            // int val = p.first;
            // int pos = p.second;
            al[val_pos[z].second].push_back(val_pos[z+1].second);
        }
    }
    // build DAG from column
    ...
    // topological sort
    // compute indegree
    vector<int> pos2indegree(m*n); // pos -> indegree;
    for (int i=0; i<m; i++) {
        for (int j=0; j<n; j++) {
            const vector<int>& child_v = val_pos(i*m+n);
            for (int child: child_v)
                pos2indegree[child]++;
        }
    }

    // add node with indegree 0
    deque<int> dq;
    for (int pos: pos2indegree) {
        if (pos2indegree[pos]==0)
            dq.push_back(pos);
    }

    while(!dq.empty()) {
        int size = dq.size();
        int res = 1;
        for (int i=0; i<size; i++) {
            int pos = dq.front();
            dq.pop_front();
            ans[pos/n][pos%n] = res;
            vector<int> children = al[pos];
            for (int child: children) {
                pos2indegree[child]--;
                if (pos2indegree[child]==0)
                    dq.push_back(child);
            }
        }
        res++;
    }
    return ans;
}
```












