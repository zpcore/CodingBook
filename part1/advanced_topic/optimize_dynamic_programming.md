## Optimize Dynamic Programming
---

---
#### Optimize space and remove corner cases
Example: Leetcode 518. Coin Change 2  
1) Original Code:  
```java
// dp[i][j] number of combinations to make up of amount i with coins[0]:coins[j];
// dp[i][j] = with coins[j] to combine + without coins[j] to combine
public int change(int amount, int[] coins) {
    if(amount==0) return 1;
    if(coins.length==0) return 0;
    int dp[][] = new int[amount+1][coins.length];
    for(int i=1; i<=amount; i++) {
        for(int j=0;j<coins.length;j++) {
            int withCoinJ = 0;
            if(i<coins[j]) withCoinJ=0;
            else if(i==coins[j]) withCoinJ=1;
            else withCoinJ=dp[i-coins[j]][j];

            if(j==0) dp[i][0] += withCoinJ; // use coins[j]
            else dp[i][j] = withCoinJ+dp[i][j-1]; //without using coins[j]
            
        }
    }
    return dp[amount][coins.length-1];
}
```

---
2) Remove Corner case: Expand dp array size to cover corner cases
In this case, we increase j. We should do the following:
* Increase dp[] size from j to j+1
* Shift variable j in for loop by 1
* Shift all the table[j] to table[j-1] in the j for loop
* Remove duplicated corner case checking
* Return dp[j+1]
```java
// dp[i][j+1] number of combinations to make up of amount i with coins[0]:coins[j];
// dp[i][j+1] = with coins[j] to combine + without coins[j] to combine
public int change(int amount, int[] coins) {
    if(amount==0) return 1;
    if(coins.length==0) return 0;
    int dp[][] = new int[amount+1][coins.length+1];//change

    for(int i=1; i<=amount; i++) {
        for(int j=1;j<=coins.length;j++) { // change to j=[1,coins.length]
            int withCoinJ = 0;
            if(i<coins[j-1]) withCoinJ=0; // change
            else if(i==coins[j-1]) withCoinJ=1; // change table[j]
            else withCoinJ=dp[i-coins[j-1]][j];

            dp[i][j] = withCoinJ+dp[i][j-1]; 
            
        }
    }
    return dp[amount][coins.length]; // change
}
```
---
3) Decrease Dimension: Change the loop sequence
Can we decrease the rp from two dimension to one dimension? We can see that dp[][j] only depends on dp[][j-1], while dp[i][] depends on dp[0:i-1][]
* Switch the j and i loop
```java
// dp[i][j+1] number of combinations to make up of amount i with coins[0]:coins[j];
// dp[i][j+1] = with coins[j] to combine + without coins[j] to combine
public int change(int amount, int[] coins) {
    if(amount==0) return 1;
    if(coins.length==0) return 0;
    int dp[][] = new int[amount+1][coins.length+1];//change
    for(int j=1;j<=coins.length;j++) { 
        for(int i=1; i<=amount; i++) {
            int withCoinJ = 0;
            if(i<coins[j-1]) withCoinJ=0; // change
            else if(i==coins[j-1]) withCoinJ=1; // change table[j]
            else withCoinJ=dp[i-coins[j-1]][j];

            dp[i][j] = withCoinJ+dp[i][j-1]; 
            
        }
    }
    return dp[amount][coins.length]; // change
}
```
* Remove the dimension that is associated with j
```java
public int change(int amount, int[] coins) {
    if(amount==0) return 1;
    if(coins.length==0) return 0;
    int dp[] = new int[amount+1];//change
    for(int j=1;j<=coins.length;j++) { 
        for(int i=1; i<=amount; i++) {
            int withCoinJ = 0;
            if(i<coins[j-1]) withCoinJ=0;
            else if(i==coins[j-1]) withCoinJ=1; 
            else withCoinJ=dp[i-coins[j-1]]; // change 

            dp[i] = withCoinJ + dp[i]; // change 
        }
    }
    return dp[amount]; // change
}
```

* Check if we can remove corner cases' condition
```java
public int change(int amount, int[] coins) {
    int dp[] = new int[amount+1];
    dp[0] = 1; //change
    for(int j=1;j<=coins.length;j++) { 
        for(int i=1; i<=amount; i++) {
            int withCoinJ = 0;
            if(i<coins[j-1]) withCoinJ=0; // change
            else withCoinJ=dp[i-coins[j-1]];

            dp[i] = withCoinJ + dp[i];
        }
    }
    return dp[amount];
```
---
##### Question:
Leetcode 188. Best Time to Buy and Sell Stock IV

---

---
#### Reverse thinking: think from end to beginning
##### Situation
When the following dp table can affect the previous filling dp table.

##### Question:
Leetcode 312. Burst Balloons
