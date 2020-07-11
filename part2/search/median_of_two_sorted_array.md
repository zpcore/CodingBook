## Median of Two Sorted Array

```c++
#if 0
Basic idea: split array A and array B in half. Make sure:
    1) leftHalf (or +1) == rightHalf;
    2) max(leftHalf)<=min(rightHalf).
    -----------------------------------
    A[0], A[1], ... A[i-1], | A[i], ... 
            total: i        |
    B[0], B[1], ... B[j-1], | B[j], ...
            total: j        | 
    -----------------------------------
Since i+j == A.size()+B.size() (or +1)
We have: j = (A.size()+B.size()+1)/2-i
In this way, we only need to control one variable i.

To do the binary search on i, first, we establish the limitation of i based on j \in [0:B.size].
We have i \in [(A.size()-B.size()+1)/2 : (A.size()+B.size()+1)/2].
#endif
    double findMedianSortedArrays(vector<int>& A, vector<int>& B) {
        if (A.size()<B.size()) A.swap(B); // make sure A is longer
        //  baded on j in range [0:B.size()], we have the following limit:
        int lo = (A.size()-B.size()+1)/2, hi = (A.size()+B.size()+1)/2+1;
        while (lo<hi) {
            int i = lo+(hi-lo)/2; // mid
            int j = (A.size()+B.size()+1)/2-i;
            if(i==0 || j==B.size() || A[i-1]<=B[j]) {
                lo = i+1;
            } else {
                hi = i;
            }
        }
        int bound1 = hi-1; // how many elements in A's left half
        int bound2 = (A.size()+B.size()+1)/2-bound1; // how many elements in B's left half
        if ((A.size()+B.size())&1) { // odd
            // bound1 cannot be 0 in this case since A.size()>B.size()
            return bound2==0? A[bound1-1]:std::max(A[bound1-1],B[bound2-1]);
        } else { // even
            int leftMaxA = bound1==0?INT_MIN:A[bound1-1];
            int leftMaxB = bound2==0?INT_MIN:B[bound2-1];
            int rightMinA = bound1==A.size()?INT_MAX:A[bound1];
            int rightMinB = bound2==B.size()?INT_MAX:B[bound2];
            return (std::max(leftMaxA,leftMaxB)+std::min(rightMinA,rightMinB))/2.0;      
        }
        
    }
```