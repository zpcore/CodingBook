## Sliding Window
Feature: look back into data in a certain range in a shifting way.

```c++
// @k: size of sliding window
void sliding_window(const vector<int>& vec, const int k) {
	int tot = 0; // total sum in the sliding window
	for(int i=0; i<vec.size(); i++) {
		/*
		Do operation to window vec[max(i-k,0):i-1]
		*/
		tot += vec[i]; 
		if(i>=k) tot -= vec[i-k];
		/*
		Do operation to window vec[max(i-k+1,0):i] 
		*/
	}
}
```