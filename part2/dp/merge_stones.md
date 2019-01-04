## Merge Stone (Stone Game)
***
### Question
There is a stone game.At the beginning of the game the player picks n piles of stones in a line.  
The goal is to merge the stones in one pile observing the following rules:  
1. At each step of the game, the player can merge two adjacent piles to a new pile.
2. The score is the number of stones in the new pile.  
You are to determine the minimum of the total score.

#### Example
For <span style="background-color: #dae9f2">[4, 1, 1, 4]</span>, in the best solution, the total score is 18:

<span style="background-color: #dae9f2">1. Merge second and third piles => [4, 2, 4], score +2</span>  
<span style="background-color: #dae9f2">2. Merge the first two piles => [6, 4]，score +6</span>  
<span style="background-color: #dae9f2">3. Merge the last two piles => [10], score +10</span>  

Other two examples:  
[1, 1, 1, 1] return 8  
[4, 4, 5, 9] return 18  

### Solution
1. Bottom up solution. Space complexity O(n^2), time complexity O(n^3).
```java
Basic idea:  //dp[k,i] represent min merge from stone[k] to stone[i]   
// X X X X | X X X... X X X A   //       k  k+1             i   
// dp[0,i] = sum(0,i)+min(dp[0,k]+dp[k+1,i]), for k=0,...,i-1   
// X X X X | X X - X ... X X X A   
//       k     j  j+1          i   
// dp[k+1,i] = sum(k+1,i)+ min(dp[k+1,j]+dp[j+1,i]) for j=i,...k+1
public int minCost(int[] stones) {  
	int len = stones.length; 
	if(len==0) return 0;    
	sumArr = new int[len+1];    
	for(int i=0; i<len; i++) {      
		sumArr[i+1] = sumArr[i]+stones[i];    
	}        
	int[][] dp = new int[len][len];    
	for(int i=1; i<len; i++) {      
		int outMin = Integer.MAX_VALUE;//stone[i]      
		for(int k=i-1; k>=0; k--) {        
			int min = Integer.MAX_VALUE;        
			if(k==i-1) min = 0;        
			for(int j=i-1; j>=k+1; j--) {          
				min = Math.min(min,sum(k+1,i)+dp[k+1][j]+dp[j+1][i]);        
			}        
			dp[k+1][i] = min;        
			outMin = Math.min(outMin,sum(0,i)+dp[0][k]+dp[k+1][i]);      
		}      
		dp[0][i] = outMin;    
	}    
	return dp[0][len-1];  
} 
public int sum(int i, int j) {     
	return sumArr[j+1]-sumArr[i];   
}
```

2. Top down solution is more easy to understand:  [Solution](https://aaronice.gitbooks.io/lintcode/content/dynamic_programming/stone_game.html)