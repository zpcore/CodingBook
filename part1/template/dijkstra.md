## Dijkstra Algorithm

Finding the shortest path in the graph. (Must be positive edge weights)

```c++
using pii = pair<int, int>;
unordered_map<int, vector<pii>> graph; // node -> vector of <neighbour node, edge weight>
//@arg1: graph
//@arg2: source node, @arg3: destination node
//@return: minimum distance from source to destination node, or -1 if destination is unreachable
int dijkstra(unordered_map<int, vector<pii>>& graph, int src, int dest) {
    unordered_map<int, int> minDis; // table record the minimum distant of the node to src. Update the table whenever we find a shorter distance to the node.
    priority_queue<pii, vector<pii>, greater<pii>()> pq; // <distance from src to this node, node>
    // unordered_set<int> visited; // we don't need visited to record the visited node
    pq.push({0, src});
    while(!pq.empty()) {
        auto [dis, cur] = pq.top(); // only a node got poped, can get the min dis to the node
        if (cur == dest)
            return dis;
        pq.pop();
        for (const auto& [next, weight]: graph[cur]) {
            int newDis = minDis[cur] + weight;
            if (minDis.contains(next) && minDis[next]<newDis) // skip update the table
                continue;
            minDis[next] = newDis; // update the table with the shorter distance
            pq.push({newDis, next});
        }
    }
    return -1;
}
```


#### Sample Question:
1631. Path With Minimum Effort