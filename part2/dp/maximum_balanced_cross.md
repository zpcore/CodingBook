## Maximum Balanced Cross
***
### Question
Given a matrix that contains only '0' and '1'. Find the maximum balanced cross in the table. Return its location of the central point. If no result found, return [-1,-1].(balanced cross: cross with same '1's in the four directions.)

#### Example
```
0 0 1 0 1
1 0 1 1 0
1 1 1 1 1
0 1 1 0 1
0 0 1 0 0
return [2,2]
```

### Solution
Dynamic programming. Space O(n^2) if not in place. Time O(n^2). n is the column of the array.
```java
public int[] findLargestCross(char[][] matrix) {
	if(matrix==null || matrix.length==0) return new int[]{-1,-1};
	int m = matrix.length;
	int n = matrix[0].length;
	if(n==0) return new int[-1,-1]{};
	int[][] dp = new int[m][n]; 
	//dp[i][j] min of 1) vertical consective '1's ending at row i at column j,
	// 2) horizontal consective '1's.
	int temp = 0; //left/(right) consective '1' end in current position
	for(int i=0; i<m; i++) {
		for(int j=0; j<n; j++) {
			if(matrix[i][j]=='1'){
				temp ++;
				int pre = i==0?0:dp[i-1][j]; //corner case		
				dp[i][j] = pre + 1; 
			}else{
				temp = 0;
				dp[i][j] = 0; //unnecesary, because it is initialized as 0
			}
			dp[i][j] = Math.min(dp[i][j],temp);
		}
		temp = 0;
	}
	int max = 0;
	int[] loc = new int[2];
	for(int i=m-1; i>=0; i--) {
		for(int j=n-1; j>=0; j--) {
			int leftTopOne = dp[i][j];
			if(matrix[i][j]=='1') {
				temp ++;
				int pre = i==m-1?0:dp[i+1][j]; //corner case
				dp[i][j] =pre + 1;
			}else {
				temp = 0;
				dp[i][j] = 0;
			}
			int rightBotOne = Math.max(dp[i][j],temp);
			int crossSize = Math.min(leftTopOne,rightBotOne);
			if(corssSize>max) {
				max = crossSize;
				loc[0] = i;
				loc[1] = j;
			}
		}
		temp = 0;
	}
	if(max==0) return new int[]{-1,-1};
	return loc;
 }
```