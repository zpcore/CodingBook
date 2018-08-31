## Kth Smallest Element of Two Sorted Array
***
#### Question
Find the kth smallest element of two sorted array. (This question can be applied for finding median of two sorted array)  

Example
```c
a = [2,4,6,7,7,10]
b = [1,2,3,4,5,5,5,11]
k = 5
```
Result: 4

#### Solution
```java
public int solution(int[] a, int[] b, int k){
	return helper(a,b,0,0,a.length-1,b.length-1,k);
}

public int helper(int[] a, int[] b, int st1, int st2, int ed1, int ed2, int k){
	if(st1>ed1) return b[st2+k-1];
	if(st2>ed2) return a[st1+k-1];
	int mid1 = (ed1+st1)/2;
	int mid2 = (ed2+st2)/2;
	if(mid1+mid2-st1-st2>=k-1){//////////
		if(a[mid1]<b[mid2]){
			return helper(a,b,st1,st2,ed1,mid2-1,k);
		}else{
			return helper(a,b,st1,st2,mid1-1,ed2,k);
		}
	}
	else{
		if(a[mid1]<b[mid2]){
			return helper(a,b,mid1+1,st2,ed1,ed2,k-mid1-st1-1);
		}else{
			return helper(a,b,st1,mid2+1,ed1,ed2,k-mid2-st2-1);
		}
	}
}
```