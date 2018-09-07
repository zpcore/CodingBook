## Binary Reduction
***
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
public ListNode mergeKLists(ListNode[] lists) {
    int totList = lists.length;
    if(totList==0) return null;
    while(totList>1) {
        int i = 0;
        while(2*i<totList) {
            if(2*i+1==totList) { // only one list left
                lists[i] = lists[2*i];
            }else { // merge two lists
                ListNode head = reduceTwo(lists[2*i],lists[2*i+1]);
                lists[i] = head;
            }
            i++;
        }
        totList = (totList+1)/2;
    }
    return lists[0];
}
```


#### Similar Question
LeetCode 23. Merge k Sorted Lists