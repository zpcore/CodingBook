## Topological Sort
---

```java
class Node {
	int val;
	Set<Node> next;
	Node(int val) {
		this.val = val;
		next = new HashSet<>();
	}
}

public List<Node> topologicalSort(Set<Node> nodeSet) {
	Set<Node> visited = new HashSet<>();
	List<Node> ls = new LinkedList<>();
	for(Node n: nodeSet) helper(visited,n,ls);
	return ls;
}

private void helper(Set<Node> visited, Node root, List<Node> ls) {
	visited.add(root);
	for(Node n: root.next) {
		if(visited.contains(n)) continue;		
		helper(visited,n,ls);
	}
	ls.add(0,root);
}
```

#### Detect if a graph is a Directed Acyclic Graph (DAG):
```java
class Node {
	int val;
	Set<Node> next;
	Node(int val) {
		this.val = val;
		next = new HashSet<>();
	}
}

public boolean checkTopologicalSort(Set<Node> nodeSet) {
	Set<Node> visited = new HashSet<>();
	for(Node n: nodeSet) {
		if(!helper(visited,n,new HashSet<>())) return false;
	}
	return true;
}

private boolean helper(Set<Node> visited, Node root, Set<Node> checking) {
	visited.add(root);
	checking.add(root);
	for(Node n: root.next) {
		if(checking.contains(n)) return false;
		if(visited.contains(n)) continue;		
		if(!helper(visited,n,checking)) return false;
	}
	checking.remove(root);
	return true;
}
```