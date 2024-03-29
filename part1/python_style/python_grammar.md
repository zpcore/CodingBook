#### Sort
```python
from functools import cmp_to_key
a = [1,4,3,2,5,7,6]

def comp(x,y):
    return x-y # from small to large
    
a.sort(key=lambda x: x, reverse = True)# sort from large to small
a.sort(key=cmp_to_key(comp)) # from small to large

b = [[1,2],[1,3],[2,4],[2,3],[5,7],[5,1]]
b.sort(key = lambda x: (x[0],x[1])) # sort by first index, then second
```

#### PriorityQueue
```python
from queue import PriorityQueue
q = PriorityQueue()
q.put(1)
q.put(3)
while not q.empty():
    num = q.get()
    print(num) # from small to big


import heapq
q = [3, 5, 1, 2, 6, 8, 7]
heapq.heapify(q)
print(q)# [1, 2, 3, 5, 6, 8, 7]
heapq.heappop(q) #1
heapq.heappop(q) #2
heapq.heappush(q, 4) #add 4 in heap
print(q[0]) #smallest element

# with custom priority
heappush(q, (10, task1)) # 10 is the priority
```

#### Binary search tree
```python
# pip install sortedcontainers
from sortedcontainers import SortedList
sl = SortedList([10, 11, 12, 13, 14])
# sl = SortedList(key=lambda x: -x)
sl.bisect_left(12) # return 2, like C++ lower_bound
sl.bisect_right(12) # return 3, like C++ upper_bound
SortedList.remove(10) # value must be a member, or use discard() 
SortedList.add(5)
```


#### Bisect (maintain a list in sorted order)
```python
a = [1,2,4,6]
bisect.bisect_left(a, x, lo=0, hi=len(a), *, key=None)
bisect.bisect_right(a, x, lo=0, hi=len(a), *, key=None
```