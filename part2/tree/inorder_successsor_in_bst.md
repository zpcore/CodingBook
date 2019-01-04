## Inorder Successor in Binary Search Tree
---

#### Question
Give a node, find the inorder successor on a BST.  
Input Node: target, root.

#### Solution
1. If target.right!=null, return the smallest node in the right subtree.
2. If target.right==null, search the path from root to target, the result is located within the path. We need to find the last node from which we traverse into the left subtree during the search.

```java
public TreeNode inorderSuccessor(TreeNode target, TreeNode root) {
	if(target==null) return null;
	if(target.right!=null) {
		TreeNode curNode = target.right;
		while(curNode.left!=null) curNode = curNode.left;
		return curNode;
	}else {
		TreeNode curNode = root;
		TreeNode resNode = null;
		while(curNode!=target) {
			if(curNode.val>target.val) {
				resNode = curNode;
				curNode = curNode.left;
			}else {
				curNode = curNode.right;
			}
		}
		return resNode;
	}
}
```

#### Follow up
What if each node do tell its parent node? In this case, the root node is not given.
```java
public TreeNode inorderSuccessor(DoubleLinkedTreeNode target) {
	if(target==null) return null;
	if(target.right!=null) {
		DoubleLinkedTreeNode curNode = target.right;
		while(curNode.left!=null) curNode = curNode.left;
		return curNode;
	}else {
		TreeNode curNode = target;
		while(curNode.parent!=null && curNode.parent.right==curNode) {
			curNode = curNode.parent;
		}
		return curNode.parent;
	}
}

```