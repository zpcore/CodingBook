## Sort List (Bottom Up for list)
---
#### Question 
Sort a linked list in O(n log n) time using constant space complexity.

Example 1:

Input: 4->2->1->3
Output: 1->2->3->4
Example 2:

Input: -1->5->3->4->0
Output: -1->0->3->4->5

#### Solution
 For the list, we need to know the size of each merge part (len1, len2 in the code). The remaining code is the same as Merge Two Sorted List.
```java
class Solution {
    public ListNode sortList(ListNode head) {
        // First, get the total length, so that we know how to assign the width
        int len = 0;
        ListNode curNode = head;
        while(curNode!=null) {
            curNode = curNode.next;
            len++;
        }
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        for(int w=1;w<len;w*=2) {
        	// use curNode: 1) to keep track of the start point 2) connect the previous section with next section or NULL
            curNode = dummy; 

            for(int i=0;i<len-w;i+=2*w) { // 0,2
                // head 1: i; head 2: i+w
                // each list length: 
                int len1 = Math.min(w,len-i);
                int len2 = Math.min(w,len-i-w);
                ListNode h1 = curNode.next;
                ListNode h2 = h1;
                
                //get start point of second head
                for(int k=0; k<len1; k++) h2 = h2.next;     
                ////////////////////////////////////////////////                
                // merge two sorted list with len1 and len2
                int cnt1 = 0, cnt2 = 0;                
                while(cnt1<len1 && cnt2<len2) {
                    if(h1.val<h2.val) {
                        curNode.next = h1;
                        h1 = h1.next;
                        cnt1++;
                    }else {
                        curNode.next = h2;
                        h2 = h2.next;
                        cnt2++;
                    }
                    curNode = curNode.next;
                }
                while(cnt1<len1) {
                    curNode.next = h1;
                    h1 = h1.next;
                    cnt1++;
                    curNode = curNode.next;
                }
                while(cnt2<len2) {
                    curNode.next = h2;
                    h2 = h2.next;
                    cnt2++;
                    curNode = curNode.next;
                }
                ///////////////////////////////////////////////////
                curNode.next = h2; // connect the end node with next section or null
            }

        }
        return dummy.next;
        
    }
}
```
