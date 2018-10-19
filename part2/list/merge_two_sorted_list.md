## Merge Two Sorted Lists
***

Merge two sorted lists into one list.

#### Recursive
```java
public ListNode mergeTwoLists(ListNode h1, ListNode h2) {
    if(h1==null) return h2;
    if(h2==null) return h1; 
    if(h1.val<h2.val) {
        h1.next = mergeTwoLists(h1.next,h2);
        return h1;
    }else {
        h2.next = mergeTwoLists(h1,h2.next);
        return h2;
    }
}

```

#### Iterative
```java
public ListNode mergeTwoLists(ListNode h1, ListNode h2) {
    ListNode dummy = new ListNode(0);
    ListNode curNode = dummy;
    while(h1!=null && h2!=null) {
        if(h1.val<h2.val) {
            curNode.next = h1;
            h1 = h1.next;
        }else {
            curNode.next = h2;
            h2 = h2.next;
        }
        curNode = curNode.next;
    }
    if(h1!=null) {
        curNode.next = h1;
    }
    if(h2!=null) {
        curNode.next = h2;      
    }
    return dummy.next;
}
```