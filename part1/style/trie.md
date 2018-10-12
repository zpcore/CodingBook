## Trie

---
```java
class Trie{

	class Node{
		Map<Character,Node> hm;
		boolean isEnd = false;
		Node() {
			hm = new HashMap<>(); 
		}
	}

	Node root;

	public Trie() {
		root = new Node();
	}

	public void insert(String word) {
		Node curNode = root;
		for(char c: word.toCharArray()) {
			if(!curNode.hm.containsKey(c)) curNode.hm.put(c,new Node());
			curNode = curNode.hm.get(c);
		}
		curNode.isEnd = true;
	}

	public boolean search(String word) {
		Node curNode = root;
		for(char c: word.toCharArray()) {
			if(!curNode.hm.containsKey(c)) return false;
			curNode = curNode.hm.get(c);
		}
		return curNode.isEnd;
	}

	// find all string with prefix
	public List<String> find(String prefix) {
		Node curNode = root;
		StringBuilder sb = new StringBuilder();
		List<String> ls = new ArrayList<>();
		for(char c: prefix.toCharArray()) {
			if(!curNode.hm.containsKey(c)) return ls;
			curNode = curNode.hm.get(c);
			sb.append(c);
		}
		dfs(curNode,sb,ls);
		return ls;
	} 

	public void dfs(Node node, StringBuilder sb, List<String> ls) {
		if(node.isEnd) ls.add(sb.toString());
		for(char c:node.hm.keySet()) {
			sb.append(c);
			dfs(node.hm.get(c),sb,ls);
			sb.deleteCharAt(sb.length()-1);
		}
	}
	
}
```
