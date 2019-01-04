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
	ls.add(0,root);
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