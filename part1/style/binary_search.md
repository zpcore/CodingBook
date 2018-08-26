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