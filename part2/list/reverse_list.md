## Reverse List
---
Iterative: Use pre node and cur node to handle.
```java
//    1 -> 2 -> 3 -> NULL
// p  c
//    p    c
//         p    c
//              p     c
public ListNode reverse(ListNode head) {
	ListNode pre = null;
	ListNode cur = head;
	while(cur!=null) {
		ListNode tmp = cur.next;
		cur.next = pre;
		pre = cur;
		cur = tmp;
	}
	return pre;
}
```

Recursive:
```java
//    1 -> 2 -|> 3 -> NULL
//    1 -> 2 <- 3     
//    1 -|> 2 <- 3 
//    1 <- 2 <- 3 
public ListNode reverse(ListNode head) {
	if(head == null || head.next==null) return head;
	ListNode newHead = reverse(head.next);
	head.next.next = head;
	head.next = null;
	return newHead;
}
```