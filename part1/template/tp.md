## Two Pointer


#### Find two number pair with sum **greater** than a value k. Return total number of such pair.
```c++
int find_sum_greater_than_k(vector<int>& nums, int k) {
    // For each right bound, how many left bound satisfy
    int n = nums.size();
    sort(nums.begin(), nums.end());
    int l = 0, r = n-1;
    int ret = 0;
    while(l<r) {
        if (nums[l]+nums[r]>k) {
            ret += r-l;
            r--;
        } else
            l++;
    }
    return ret;
}
```

#### Find two number pair with sum **smaller** than a value k. Return total number of such pair.
```c++
int find_sum_smaller_than_k(vector<int>& nums, int k) {
    int n = nums.size();
    sort(nums.begin(), nums.end());
    int l = 0, r = n-1;
    int ret = 0;
    while(l<r) {
        if (nums[l]+nums[r]<k) {
            ret += r-l;
            l++;
        } else
            r--;
    }
    return ret;
}
```