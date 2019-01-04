## Binary Index Tree (Fenwick Tree)
---

```java
public class BIT{
	
	int[] BITArray;

	BIT(int[] nums) {
		// construct the BIT from array, time complexity(O(nlogn))
		int len = nums.length;
		BITArray = new int[len+1];
		for(int i=0; i<len; i++) update(i,nums[i]);
	}

	//get range sum [0,n]: Find the node n+1, sum up from n+1 to its root.
	public int getSum(int n) {
		int parent = n+1;
		int sum = 0;
		while(parent!=0) {
			sum += BITArray[parent];
			parent -= (-parent & parent); // get parent
		}
	}

	public void update(int index, int incVal) {
		// incVal: new val - old val
		// start from index+1 pos in the BIT
		int next = index+1;
		while(next<BITArray.length) {
			BITArray[next] += incVal;
			next += (-next & next); // get next			
		}
	}

}
```

---
### Related Questions: Leetcode 315. Count of Smaller Numbers After Self