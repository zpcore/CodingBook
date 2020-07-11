## Binary Search
***
#### 1. In a increasing array, find the next big/equal number. Return its index.
```java
// Find i, nums[i-1] < x <= nums[i]
int lo=0, hi=nums.length-1;
while(lo < hi){
  int mid = lo+(hi-lo)/2;
  if(nums[mid]<x) {
    lo = mid+1;
  }else {
    hi = mid;
  }
}
return hi;
```
#### 2. In a increasing array, find the next big number. Return its index.
```java
// Find i, nums[i-1] <= x < nums[i]
int lo=0, hi=nums.length-1;
while(lo < hi) {
  int mid = lo+(hi-lo)/2;
  if(nums[mid]<=x) {
    lo = mid+1;	
  }else{
    hi = mid;
  }
}
return hi;
```
### General Case (GOLDEN CODE):

#### 3. Find smallest/largest x that meet with the requirment

nums: A A A A A A A | B B B B B B B  
FA(x): Funtion returns true if input x is in set A and false in set B.  
FB(x): Funtion returns true if input x is in set B and false in set A.  

Steps to use binary search:  
1) Find the seperation function FA(x);  
2) Decide to find last A or first B.
```java
int lo=0, hi=nums.length;
while(lo < hi){
  int mid = lo+(hi-lo)/2;
  if(FA[mid]) // or !FB[mid] (Condition that left part can meet)
    lo = mid+1;
  else
    hi = mid;
}
return hi-1;//find largest x in set A, or -1 if none.
return hi;//find smallest x in set B, or nums.length if none.
```

### Ultra Golden code:
In this case, we conside the limitation.  
```c++
// assume i is limited by [lb:ub]
int lo = lb, hi = ub+1;
while(lo<hi){
  int mid = lo+(hi-lo)/2;
  if(FC[mid] || FA[mid]) // or !FB[mid] (Condition that left part can meet)
    //FC[] is the left most bound judge function that FA[] cannot judge (usually due to array index "mid" out of left boundry)
    lo = mid+1;
  else
    hi = mid;
}
return hi-1;//find largest x in set A, or lb-1 if none.
return hi;//find smallest x in set B, or ub if none.
```

### Binary Search Question
Leetcode 658. Find K Closest Elements
Leetcode 4. Median of Two Sorted Arrays (with limit)
Leetcode 1095. Find in Mountain Array (with limit)

