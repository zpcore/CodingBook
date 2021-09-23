### Segment Tree
---

```
                            1 [0,7]
                         /          \
                        /            \
                       /              \
                      /                \
                     /                  \
                2 [0,4]                3[4,7]
                    / \                  / \
                   /   \                /   \
                  /     \              /     \
             4[0,1]  5[2,3]         6[4,5]  7[6,7]
             /\         /\            /\       /\
            /  \       /  \          /  \     /  \
        8[0] 9[1]  10[2] 11[3]  12[4] 13[5] 14[6] 15[7]
```
[3,5] l:11 r: 14 res+=id[11] l:12| l:6 r:7 res+id[5]+id[6] l:6 r:6| l:2 r:3 res+=id[2]
#### Range query

##### Iterative version
```c++
const int N = 1e5;  // limit for array size
int n;  // array size
int t[2 * N];

void build(vector<int> arr) {  // build the tree from bottom up
    n = arr.size();
    for (int i=0; i<n; ++i) 
      t[i+n] = arr[i];
    for (int i=n-1; i>0; --i) 
        t[i] = t[2*i] + t[2*i+1];
}

void update(int p, int value) {  // set value at position p to value
    t[n+p] = value;
    for (int i=(n+p)/2; i>=1; i/=2)
        t[i] = t[2*i] + t[2*i+1];
}

int query(int l, int r) {  // sum on interval [l, r)
    int res = 0;
    l += n;
    r += n;
    for (; l < r; l >>= 1, r >>= 1) {
        if (l&1) res += t[l++]; // is right branch
        if (r&1) res += t[--r]; // is right branch
    }
    return res;
}
```

##### Recursive version
```c++

```






#### Range minimum
```c++
void build() {  // build the tree
    for (int i=n-1; i>0; --i) 
        //t[i] = t[i<<1] + t[i<<1|1];
        t[i] = min(t[2*i] + t[2*i+1]);
}

void modify(int p, int value) {  // set value at position p
    for (t[p += n]=value; p>1; p>>=1) 
        t[p>>1] = min(t[p] + t[p^1]);
}

int query(int l, int r) {  // sum on interval [l, r)
    int res = INT_MAX;
    for (l += n, r += n; l < r; l >>= 1, r >>= 1) {
        if (l&1) res = min(res, t[l++]); // is right branch
        if (r&1) res = min(res, t[--r]); // is right branch
    }
    return res;
}
```