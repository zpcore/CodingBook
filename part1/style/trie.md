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
	
}
```
