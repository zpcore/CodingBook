## Binary Search Tree Operation
---

#### Insert a node in BST
```java
public TreeNode insert(TreeNode root, int val) {
	if(root==null) {
		TreeNode newNode = TreeNode(val);
		return newNode;
	}
	if(root.val < val) { // insert to right subtree
		root.right = insert(root.right);
	} else if(root.val > val) { //insert to left subtree
		root.left = insert(root.left);
	}
	return root;
}
```

#### Remove a node in BST
Three Cases: 
case 1: the key node: key.left == null && key.right == null -> delete and return null  
case 2: the key node: key.left != null && key.right != null -> find the next big node to replace and remove that node  
case 3: key.left != null || key.right != null -> replace the node with the only child node  
```java
public TreeNode remove(TreeNode root, int val) {
	if(root == null) return null;
	if(root.val < val) { // in the right subtree
		root.right = remove(root.right,val);
		return root;
	} else if(root.val > val) { // in the left subtree
		root.left = remove(root.left,val);
		return root;
	} else { //(root.val == val) 
		if(root.left == null && root.right == null) return null;
		else if(root.left != null && root.right != null) {
			// find the next big; Caution, next big, not next biggest
			TreeNode curNode = root.right;
			while(curNode.left!=null) curNode = curNode.left;
			root.val = curNode.val;
			root.right = remove(root.right,curNode.val);
			return root;
		}else {
			return root.left==null?root.right:root.left;
		}
	}
}
```