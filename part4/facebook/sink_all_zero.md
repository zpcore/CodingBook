## Sink All Zero
---

#### Question
Given a binary tree containing only 0 and 1, sink all 0 to the bottom of the binary tree. ('Sink' means for all the node with value 0, its child node should all be 0.)

#### Solution
```java
public TreeNode sinkAllZero(TreeNode root) {
	if(root==null) return root;	
	sinkAllZero(root.left);
	sinkAllZero(root.right);
	sink(root);
	return root;
}


private void sink(TreeNode root) {
	// in this time, root.val = 0.
	if(root.val==1) return;
	if((root.left==null || root.left.val==0) && (root.right==null || root.right.val==0)) return;
	if(root.left!=null && root.left.val==1) {
		root.left.val = 0;
		root.val = 1;
		sink(root.left);
	}else {
		root.right.val = 0;
		root.val = 1;
		sink(root.right);
	}
}
```
"Any fool can write code that a computer can understand. Good programmers write code that humans can understand." --- Martin Fowler 