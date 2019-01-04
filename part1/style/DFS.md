## DFS
---
#### T0: Find all permutation, combination, etc.. How to avoid producing duplicate results:
Rule: 相同元素：前不取，后不取。取前再取后。  
Explain: For two A and different locations: i, j. If in the previous step we didn't select A_i, in current step, we shouldn't select A_j.  

#### T1: Modify an object to meet with the requirements. List all the results.  
Use the string object as an example:
```java
List<String> result = new ArrayList<>();

// f(i) element at index i force the program into next recursion
// Op(j) if operation on the element at index j safisfies the requirment
public void helper(int last_modified, int last_checked, String s) {
	for(int i = last_checked+1; i<s.length(); i++) {
		if(f(i)) {
			for(int j=last_checked+1; j<=i; j++) {
				if(op(j)) helper(j,i,s);
			}
			return;
		}
	}
	result.add(s); // now, add the result here
}
```
Example: Leetcode 301. Remove Invalid Parentheses

#### T2: Two DFS ways (Broader VS Deeper): 
Subset Problem:   
```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        helper(res,new ArrayList<>(),nums,0);
        return res;
    }
    
    public void helper(List<List<Integer>> res, List<Integer> ls, int[] nums, int pos) {
        res.add(new ArrayList<>(ls)); //doesn't add element afterwards
        for(int i=pos;i<nums.length;i++) { //add nums[i]
            ls.add(nums[i]);
            helper(res,ls,nums,i+1);
            ls.remove(ls.size()-1);
        }
    }
}
```

```java
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        helper(res,new ArrayList<>(),nums,0);
        return res;
    }
    
    public void helper(List<List<Integer>> res, List<Integer> ls, int[] nums, int pos) {
        if(pos==nums.length) {
            res.add(new ArrayList<>(ls));
            return;
        }
        helper(res,ls,nums,pos+1); // doesn't add nums[pos]
        ls.add(nums[pos]);
        helper(res,ls,nums,pos+1); // add nums[pos]
        ls.remove(ls.size()-1);
        
    }
```
#### T3: Iterative DFS
```java
// iterative DFS through a graph
public void dfs(Node root) {
	if(root==null) return;
	Deque<Node> stack = ArrayDeque<>();
	stack.push(root);
	while(!stack.isEmpty()) {
		Node curNode = stack.pop();
		for(Node n: curNode.next()) {
			if(n!=null) stack.push(n); // check if the node exist.
			// operation to Node n here:
		}
	}
}
```


#### T4: Subset and Subsequence
Use sort for subset
Cannot use sort for subsequence. Use boolean table or set to get rid of duplicate.

