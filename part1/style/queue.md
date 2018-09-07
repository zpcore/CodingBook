## Queue in Java
---
#### Stack (LIFO)
```java
Deque<> stack = new ArrayDeque<>();
```
Method Summary:
empty(), peek(), pop(), push(E item), search(Object o)  
<1,2,3,4,5>  
offer(0)=offerLast(0):<1,2,3,4,5,0>  
push(0)=<0,1,2,3,4,5>  


#### FIFO
```java
 LinkedList<Integer> fifo = new LinkedList();
 Queue<Integer> fifo = new LinkedList();
```
add,offer (add tail)  
peek (peek head)  
remove,poll (remove head)  

#### Heap
##### Priority Queue
```java
Arrays.sort(intervals, (u1,u2) -> u1.start-u2.start);
//may overflow
PriorityQueue<Interval> heap=new PriorityQueue<>(intervals.length,(a,b)->a.end-b.end);
//no overflow
PriorityQueue<Interval> heap=new PriorityQueue<>((a,b)->a.val<b.val?-1:1);
```
#### Time Complexity of Priorityqueue
remove() -> This is to remove the head/root, it takes O(logN) time.  
remove(Object o) -> This is to remove an arbitrary object. Finding this object takes O(N) time, and removing it takes O(logN) time.  


### List of List
```java
List<List<Integer>> res = new ArrayList<>();//correct
List<List<Integer>> res = new ArrayList<List<>>();//Wrong
```
***
### Reverse linked list within a range
#### Example
```java
1 -> 2 -|> 3 -> 4 -> 5 -|> 6, input: node 2, nodeNum = 3, return: node 6
1 -> 2 -|> 5 -> 4 -> 3 -|> 6
```
#### Solution
```java
public ListNode reverse(ListNode preHead, int nodeNum) {
    // @input: reverse the nodeNum of nodes after preHead
    // @return: the first node right of the reversed area or null
    ListNode preNode = preHead.next;
    if(preNode == null) return null;
    ListNode curNode = preNode.next;
    if(nodeNum==0) return preNode;
    if(nodeNum==1) return curNode;
    for(int i=0; i<nodeNum-1 && curNode!=null; i++) {
        ListNode tmp = curNode.next;
        curNode.next = preNode;
        preNode = curNode;
        curNode = tmp;
    }
    preHead.next.next = curNode;
    preHead.next = preNode;
    return curNode;
}
```