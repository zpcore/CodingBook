## Union Find
---

(Not necessary) First, construct a bijection (a mapping) between your objects and the integers in the range [0,n)  
Map(objext->Integer[0,n)]  
Array[object 1, object 2, object 3,..., object n] 


### Use array to save space
```java
int[] array = new int[len]; // assign a initial size

public void makeSet(int i) {
    array[i] = i;
}

public void union(int a, int b) {
    if(findSet(a)!=findSet(b)) {
        array[array[a]] = array[b]; 
    }
}

public int findSet(int a) {
    if(array[a]!=a) {
        array[a] = findSet(array[a]);
    }
    return array[a];
}
```

### Self contained into a class
```c++
struct DSJ {
    vector<int> par;
    DSJ(int len) {
        par.resize(len);
        iota(par.begin(), par.end(), 0);
    }
    void uni(int a, int b) {
        if (find(a)!=find(b))
            par[a] = par[b];
    }
    int find(int x) {
        if (par[x]!=x)
            x = find(par[x]);
        return par[x];
    }
};
```


### Use rank to balance the set
```java
class Node{
    int rank;
    int data;
    Node parent;
    Node(int data) {
        this.data = data;
        int rank = 0;
        parent = this;
    }
}

public Map<Integer,Node> hm = new HashMap<>();

public void makeSet(int data) {
    Node node = new Node(data);
    hm.put(data,node);
}

public void union(int a, int b) {
    union(hm.get(a),hm.get(b));
}

public void union(Node a, Node b) {
    // union group with small rank to big
    Node pA = findSet(a);
    Node pB = findSet(b);
    if(pA==pB) return;
    Node pBig = pA.rank>=pB.rank? pA:pB;
    Node pSmall = pA.rank>=pB.rank? pB:pA;
    if(pBig.rank==pSmall.rank) pBig.rank++;
    pSmall.parent = pBig;
}

public int findSet(int i) {
    return findSet(hm.get(i)).data;
}

public Node findSet(Node u) {       
    if(u.parent!=u) {
        u.parent = findSet(u.parent); // path compression           
    }
    return u.parent;
}
```