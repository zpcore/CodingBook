## Find The Previous Smaller ELement (Mutation Leetcode 556) (unchecked)
---

#### Question
Given a positive 32-bit integer n, you need to find the biggest 32-bit integer which has exactly the same digits existing in the integer n and is smaller in value than n. If no such positive 32-bit integer exists, you need to return -1.
```java
Example 1:

Input: 12
Output: -1
 

Example 2:

Input: 21
Output: 12
```

#### Solution


```java
public int PreviousSmallerElement(int n) {
	char[] cArr = String.valueOf(n).toCharArray();
	
	//find nums[i]>nums[i+1] ,e.g. (3) 1 2 3 4 5
	int i = cArr.length-2;
	while(i>=0 && cArr[i]<=cArr[i+1]) i--;
	if(i==-1) return -1; // cannnot construct pre smaller 

	// method 1: sort cArr[i:] 
	// Arrays.sort(cArr,i,cArr.length);
	// reverse(cArr,i,cArr.length-1);


	// method 2: in place swap
	int j = cArr.length-1;
	while(j>i) {
		if(cArr[i]>cArr[j]) {
			swap(cArr,i,j);
			break;
		}
		j--;
	}
	reverse(cArr,i+1,cArr.length-1);


	return Integer.valueOf(new String(cArr));
}
```