## Is Subsequence
---

#### Question
Given a string s and a string t, check if s is subsequence of t.

You may assume that there is only lower case English letters in both s and t. t is potentially a very long (length = 500,000) string, and s is a short string (<=100).  
A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ace" is a subsequence of "abcde" while "aec" is not).  
Example 1:
s = "abc", t = "ahbgdc"
Return true.  
Example 2:
s = "axc", t = "ahbgdc"
Return false.


#### Soution
```java
public boolean isSubsequence(String s, String t) {
    if(s.length()==0) return true;
    int j = 0;
    for(int i=0;i<t.length();i++) {
        if(t.charAt(i)==s.charAt(j)) j++;
        if(j==s.length()) return true;
    }
    return false;
}
```

#### Follow-up:
If there are lots of incoming S, say S1, S2, ... , Sk where k >= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?
```java
//  tab
//t:      a h  b   g   a   d    c
//   a  0 4 4  4   4   -1  -1  -1 
//   b  1 1 1  -1  -1  -1  -1  -1
//   c  6 6 6  6   6   6   6  -1
//   g  3 3 3  3   -1  -1  -1  -1
//   h  1 1 -1 -1  -1  -1  -1  -1
public boolean isSubsequence(String s, String t) {
    // preprocessing
    // dp[i][j]: at current location j, next(include pos j)  char i+'a' first appear at location dp[i][j]
    // dp[i][j] = dp[i][j+1] except dp[t.charAt(j)-'a'][j] = j+1
    int[][] dp = new int[26][t.length()+1];
    for(int[] n:dp) Arrays.fill(n,-1);
    for(int j=t.length()-1;j>=0;j--) {
        for(int i=0;i<26;i++) {
            dp[i][j] = dp[i][j+1];
        }
        dp[t.charAt(j)-'a'][j] = j+1;
    }
    
    int pos = 0;
    for(char c:s.toCharArray()) {
        int i = c-'a';
        pos = dp[i][pos];
        if(pos==-1) return false;
    }
    return true;
}
```