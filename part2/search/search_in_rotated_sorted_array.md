## Search In Rotated Sorted Array
---
### Without Duplucate (LeetCode)
```java
// find biggest nums[i], such that 
// 1. target<nums[0] && (nums[i]>=nums[0] || nums[i]<=target)      ||
// 2. target>=nums[0] && (nums[i]>=nums[0] && nums[i]<=target)
public int search(int[] nums, int target) {
    if(nums.length==0) return -1;
    int lo = 0, hi = nums.length;
    while(lo<hi) {
        int mid = lo+(hi-lo)/2;
        if((target<nums[0] && (nums[mid]>=nums[0]||nums[mid]<=target))||
           (target>=nums[0] && (nums[mid]>=nums[0] && nums[mid]<=target))) lo = mid+1;
        else hi = mid;
    }
    if(hi-1>=0 && nums[hi-1]!=target) return -1;
    return hi-1;
}
```


### With Duplicate (LeetCode 81)
Main point: remove the tail element that equals nums[0]

```java
public boolean search(int[] nums, int target) {
    if(nums.length==0) return false;
    int lo = 0, hi = nums.length;
    while(hi>=2 && nums[hi-1]==nums[0]) hi--; // remove the duplicate tail
    while(lo<hi) {
        int mid = lo+(hi-lo)/2;
        if((target<nums[0] && (nums[mid]>=nums[0]||nums[mid]<=target))||
           (target>=nums[0] && (nums[mid]>=nums[0] && nums[mid]<=target))) lo = mid+1;
        else hi = mid;
    }
    if(hi-1<0 || nums[hi-1]!=target) return false;
    return true;
}
```