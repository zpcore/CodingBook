## Deserialize Binary Tree (Correctness Unchecked)
---

#### Question
Given a string, deserialize the string into tree. Each root node will be followed by () with its child nodes inside.
String "4(5,6)" represents the tree:
```
 4
/  \
5  6
```
String "4(5(7,8),9)" represents the tree:
```
        4
      /   \
     5     9
   /   \
   7   8
```

#### Solution
```java
index = 0

public TreeNode deserialize(String s) {
	return helper(s)[0];
}


private TreeNode[] helper(String s) { 
	// Always skip the last ')' before return;
	if(index==s.length()) return null;
	TreeNode n1 = null;
	TreeNode n2 = null;
	int i=st;
	
	//////////////////////////n1
	int val = 0;
	if(i<s.length() && Character.isDigit(s.charAt(i))) {
		while(i<s.length() && Character.isDigit(s.charAt(i))) {
			val = val*10+s.charAt(i)-'0';
			i++;
		}
		n1 = new TreeNode(val);
	}

	if(i<s.length() && s.charAt(i)=='(') { // find subchild of n1
		index = i+1;
		TreeNode[] child = helper(s);
		n1.left = child[0];
		n1.right = child[1];
	}
	i = index; // reset i from last recursion

	if(i<s.length() && s.charAr(i)==',') i++; //skip the ','

	//////////////////////////n2
	val = 0;
	if(i<s.length() && Character.isDigit(s.charAt(i))) {
		while(i<s.length() && Character.isDigit(s.charAt(i))) {
			val = val*10+s.charAt(i)-'0';
			i++;
		}
		n2 = new TreeNode(val);
	}
	

	if(i<s.length() && s.charAt(i)=='(') { // find subchild of n1
		index = i+1;
		TreeNode[] child = helper(s);
		n2.left = child[0];
		n2.right = child[1];
	}
	i = index; // reset i from last recursion

	//////////////////////////Return two child nodes
	if(i<s.length() && s.charAr(i)==')') index = i+1;
	return new TreeNode[]{n1,n2};
}
```

