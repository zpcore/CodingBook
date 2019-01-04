## Infiltrate a Binary Tree (unchecked)
---

#### Question
浸润整棵需要的时间是6 = 0 + 2 + 4
```
      0
    /   \
   2     3
  /
4
```
follow up1: 如果是k叉数  
follow up2: 将每个node按照浸润的时间先后打印出来，上面例子应该是 0 -> 2 -> 3 -> 4  


#### Solution
```java
public int infiltrateATree(TreeNode root) {
	if(root==null) return 0;
	return Math.max(infiltrateATree(root.left),infiltrateATree(root.right))+root.val;
}
```

##### Follow up 1:  
```java
public int infiltrateAManyTree(ManyTreeNode root) {
	if(root==null) return 0;
	int max = 0;
	for(ManyTreeNode mnode:root.childList) {
		max = Math.max(max,infiltrateAKTree(mnode));
	}
	return max+root.val;
}
```

##### Follow up 2:  
```java
class ExtendedTreeNode {
	TreeNode node;
	int sum;
	ExtendedTreeNode(TreeNode node, int val) {
		this.node = node;
		sum = val;
	}
}
public List<TreeNode> infiltrateATree(TreeNode root) {
	List<TreeNode> res = new ArrayList<>();
	PriorityQueue<ExtendedTreeNode> pq = new PriorityQueue<>((a,b)->a.sum-b.sum);
	pq.add(new ExtendedTreeNode(root,root.val));
	while(!pq.isEmpty()) {
		ExtendedTreeNode curNode = pq.poll();
		res.add(curNode.node);
		if(curNode.left!=null) pq.add(new ExtendedTreeNode(curNode.node.left,curNode.node.left.val+curNode.sum));
	}
	return res;
}
```