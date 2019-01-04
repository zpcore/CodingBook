## Convert Sorted List to Binary Search Tree
---

#### Question
Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Example:

Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5

#### Solution
Key point: Use inorder traverse to add each TreeNode to the tree; 
Make use of global variable to track the current value of the new treeNode.
```java
class Solution {

    ListNode cNode;
    
    public TreeNode sortedListToBST(ListNode head) {
        ListNode curNode = head;
        int cnt = 0;
        while(curNode != null) {
            curNode = curNode.next;
            cnt ++;
        }
        cNode = head;
        return helper(0,cnt-1);
    }
    // 0 1 2
    
    private TreeNode helper(int i, int j) { // return the last node
        if(i>j) return null;
        int mid = i+(j-i)/2; // 0,1,2
        TreeNode left = helper(i,mid-1);
        TreeNode newNode = new TreeNode(cNode.val);
        cNode = cNode.next;
        newNode.left = left;
        
        TreeNode right = helper(mid+1,j);
        newNode.right = right;
        
        return newNode;

    }
}
```