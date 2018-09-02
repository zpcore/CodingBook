## Sort
***
### Quicksort
```java
public int[] quickSort(int[] arr) {
	helper(arr,0,arr.length-1);
	return arr;
}

public void helper(int[] arr, int start, int end) {
	if(start>=end) return;
	int mid = partition(arr,start,end);
	helper(arr,start,mid-1);
	helper(arr,mid+1,end);
}


public int partition(int[] arr, int lo, int hi) {
	int pivot = arr[hi]; //pivot (Element to be placed at right position)
	int i = lo; //left of i is element smaller than pivot
	for (int j=lo; j<=hi; j++) { //element to be processed (include j)
		if (arr[j] <= pivot)
			swap(arr, i++ ,j);
	}
	return i-1; //return the pivot
}

public void swap(int[] arr, int i, int j) {
	int tmp = arr[i];
	arr[i] = arr[j];
	arr[j] = tmp;
}

```
***
### Mergesort
```java


```
