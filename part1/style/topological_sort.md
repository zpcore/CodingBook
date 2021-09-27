## Topological Sort
---
#### Build the Topological Sequence
Use node to build the dependency. You can also use map or other data structure.
```java
class Node {
    int val;
    Set<Node> next; //current node has higher priority than next
    Node(int val) {
        this.val = val;
        next = new HashSet<>();
    }
}
```

```java
public List<Node> topologicalSort(Set<Node> nodeSet) {
    Set<Node> visited = new HashSet<>();
    List<Node> ls = new LinkedList<>();
    for(Node n: nodeSet) helper(visited,n,ls);
    return ls;
}

private void helper(Set<Node> visited, Node root, List<Node> ls) {
    if(visited.contains(n)) return;
    visited.add(root);
    for(Node n: root.next) {        
        helper(visited,n,ls);
    }
    ls.add(0,root); // or add to end and reverse the final result in topologicalSort
}
```

---
#### Detect if a graph is a Directed Acyclic Graph (DAG):
##### DFS Solution
```java
public boolean checkTopologicalSort(Set<Node> nodeSet) {
    Set<Node> visited = new HashSet<>();
    for(Node n: nodeSet) {
        if(!helper(visited,n,new HashSet<>())) return false;
    }
    return true;
}

private boolean helper(Set<Node> visited, Node root, Set<Node> checking) {
    if(checking.contains(n)) return false;
    if(visited.contains(n)) return true;
    visited.add(root);
    checking.add(root);
    for(Node n: root.next) {
        if(!helper(visited,n,checking)) return false;
    }
    checking.remove(root); // CAUTION: Remember to add this line!
    return true;
}
```
##### BFS Solution (unchecked)
Use in degree.
```java
public boolean checkTopologicalSort(Set<Node> nodeSet) {
    Set<Node> visited = new HashSet<>();
    Map<Node,Integer> indegree = new HashMap<>();
    Deque<Node> dq = new ArrayDeque<>();
    List<Node> sequence = new ArrayList<>();
    // compute indegree of each node
    for(Node n: nodeSet) indegree.put(m,0);     
    for(Node n: nodeSet) {
        if(n.next!=null) for(Node m: n.next) {
            indegree.put(m,indegree.get(m)+1);
        }
    }
    //add node with indegree 0
    for(Node n: indegree.keySet()) {
        if(indegree.get(n)==0) dq.add(n);
    }
    // BFS
    while(!dq.isEmpty()) {
        int size = dq.size();
        for(int i=0;i<size;i++) {
            Node curNode = dq.pollFirst();
            sequence.add(curNode);
            if(curNode.next!=null) for(node n:curNode.next) {
                int in = indegree.get(n);
                indegree.put(n,in-1);
                if(indegree.get(n)==0) dq.add(n);
            }
        }
    }
    return sequence.size()==nodeSet.size();
    // and we can print the priority from List sequence
}
```

Output all possible sequence (tested)
```c++
int dfs(unordered_set<int>& visited, unordered_map<int, vector<int>>& g, int cur, unordered_set<int>& curLine, unordered_map<int, int>& node2lvl) {
    // if (curLine.find(cur)!=curLine.end()) 
    //     return;
    // curLine.insert(cur);
    if (visited.find(cur)!=visited.end())
        return node2lvl[cur];
    visited.insert(cur);
    int lvl = 0;
    for (int nxt: g[cur]) {
        lvl = max(lvl, dfs(visited, g, nxt, curLine, node2lvl)+1);
    }
    // curLine.erase(cur);
    node2lvl[cur] = lvl;
    return lvl;
}

vector<vector<int>> generate_sequences(int cur_lvl, unordered_map<int, vector<int>>& lvl2nodes) {
    vector<vector<int>> ret;
    if (lvl2nodes.find(cur_lvl)==lvl2nodes.end())
        return ret;

    int nxt_lvl = cur_lvl+1;
    auto& comb = lvl2nodes[cur_lvl];
    sort(comb.begin(), comb.end());
    do {
        auto tmp = generate_sequences(nxt_lvl, lvl2nodes);
        if (tmp.size()==0) {
            ret.push_back({});
            auto& ref = ret.back();
            ref.insert(ref.end(), comb.begin(), comb.end());
        } else {
            for (auto v: tmp) {
                ret.push_back({});
                auto& ref = ret.back();
                ref.insert(ref.end(), comb.begin(), comb.end());
                ref.insert(ref.end(), v.begin(), v.end());
            }
        }
        
    } while(next_permutation(comb.begin(), comb.end()));

    return ret;
}


vector<vector<int>> topologicalSort(unordered_map<int, vector<int>>& g) {
    unordered_set<int> visited;
    unordered_set<int> curLine; // detect the circle

    unordered_set<int> allNode;
    // collect all the node number
    for (auto [n, nxt_v]: g) {
        allNode.insert(n);
        for (auto nxt: nxt_v) {
            allNode.insert(nxt);
        }
    }

    // compute depth lvl first using topological sort
    unordered_map<int, int> node2lvl;
    for (auto n: allNode) {
        // curLine.clear();
        dfs(visited, g, n, curLine, node2lvl);
    }

    // generate all possible sequence based on the lvl
    unordered_map<int, vector<int>> lvl2nodes;
    for (auto [n, l]: node2lvl)
        lvl2nodes[l].push_back(n);

    vector<vector<int>> ret;
    ret = generate_sequences(0, lvl2nodes);
    return ret;
}

```