## Inorder Traverse without Stack

#### Question
Suppose each node has the attribute 'parent'.



#### Solution
The key point is to determining the lastNode we visited. So that we can know the current walking path.

```java
public List<DoubleLinkedTreeNode> inorderWithoutStack(DoubleLinkedTreeNode root) {
	DoubleLinkedTreeNode curNode = root, lastNode = null;
	List<DoubleLinkedTreeNode> res = new ArrayList<>();
	while(curNode != null) {
		if(lastNode == curNode.parent) { // go down
			if(curNode.left!=null) {
				lastNode = curNode;
				curNode = curNode.left;
			}else if(curNode.right!=null) {
				res.add(curNode);
				lastNode = curNode;
				curNode = curNode.right;
			}else {
				res.add(curNode);
				lastNode = curNode;
				curNode = curNode.parent;
			}			
		}else if(lastNode == curNode.left) { // go up
			res.add(curNode);
			if(curNode.right!=null) {
				lastNode = curNode;
				curNode = curNode.right;
			}else {
				lastNode = curNode;
				curNode = curNode.parent;
			}
		}else if(lastNode == curNode.right) { // go up	
			lastNode = curNode;
			curNode = curNode.parent;
		}
	}
	return res;
}
```


Implement the iterator for inorder. (unchecked)
```java
class InorderIterator implements Iterator<TreeNode> {
	Deque<TreeNode> stack;
	TreeNode curNode;
	public InorderIterator(TreeNode root) {
		curNode = root;
		stack = new ArrayDeque<>();
	}

	@Override
	public TreeNode next() {
		while(curNode!=null) {
			stack.push(curNode);
			curNode = curNode.left;
		}
		TreeNode res = curNode;
		curNode = stack.pop();
		curNode = curNode.right;
		return res;
	}

	@Override 
	public boolean hasNext() {
		return !stack.isEmpty()||curNode!=null;
	}
}
```

