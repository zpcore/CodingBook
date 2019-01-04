## Binary Reduction 
***
(Use ListNode as the examlple)  
Apply binary reduction to a group of lists.  

#### Recursive (top down)
Space Complexity: O(lg(n)), n is the length of lists
```java
public ListNode binaryReduction(ListNode[] lists, int s, int e) {
	// at the begining, s=0, e=lists.length-1
	if(s==e) return lists[s];
	int mid = s+(e-s)/2;
	ListNode n1 = binaryReduction(lists,s,mid);
	ListNode n2 = binaryReduction(lists,mid+1,e);
	lists[s] = reduceTwo(n1,n2);//method to reduce two lists.
}
```

#### Iterative (bottom up)
Space Complexity: O(1)
```java
// Solution 2
public ListNode mergeKLists(ListNode[] lists) {
    int length = lists.length;
    for(int width = 1; width<length; width*=2){

        for(int i=0; i+width<length; i+=2*width) {
            int left = i, mid = i+width, right = i+2*width;
            // left: start point of first half; mid: start point of second half;
            // right: end point of the two part (exclusive)
            ListNode head = mergeTwoLists(lists[left],lists[mid]);
            lists[left] = head;
        }
    }
    return lists[0];
}
```
<details><summary>deprecated code</summary>
<p>

```java
Solution 1 (deprecated)
public ListNode mergeKLists(ListNode[] lists) {
    int totList = lists.length;
    if(totList==0) return null;
    while(totList>1) { //decrease by half layer by layer until it reaches 1.

        for(int i=0; 2*i<totList; i++) {
        	int 
 			if(2*i+1==totList) { // only one list left
                lists[i] = lists[2*i];
            }else { // merge two lists
                ListNode head = reduceTwo(lists[2*i],lists[2*i+1]);
                lists[i] = head;
            }
        }

        totList = (totList+1)/2;
    }
    return lists[0];
}
```
</p>
</details>

#### Similar Question
LeetCode 23. Merge k Sorted Lists