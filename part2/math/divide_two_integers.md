##Divide Two Integers (LC29)

---
#### Question
Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero.

Example 1:

Input: dividend = 10, divisor = 3
Output: 3
Example 2:

Input: dividend = 7, divisor = -3
Output: -2


#### Solution
Break down: 
Example 10/3: 10 = 3*2 + 3*1 + ?
Example 123/7: 123 = 7*16 + 7*1 + ?
Suppose both divident and divisor are positive number:
```java
public int divide(int dividend, int divisor) {
    int res = 0;
    while(dividend>=divisor) {
        int time = 1;
        int tmp = divisor;
        while(tmp<<1>>1==tmp && dividend >= (tmp<<1)) {                
            tmp<<=1;
            time <<= 1;
        }
        res+=time;
        dividend-=tmp;
    }
    return res;
}
```

##### Way to prevent overflow issue: 
Since negative number has wider range than positive number, we use negative for the computation:
```java
public int divide(int dividend, int divisor) {
    int sign = 1;
    if((dividend>0 && divisor<0) || (dividend<0 && divisor>0)) sign = -1;
    dividend = dividend>0?-dividend:dividend;
    divisor = divisor>0?-divisor:divisor;
    int res = 0;
    while(dividend<=divisor) {
        int time = 1;
        int tmp = divisor;
        while(tmp<<1>>1==tmp && dividend <= (tmp<<1)) {                
            tmp<<=1;
            time <<= 1;
        }
        res-=time; // keep res negative
        dividend-=tmp;
    }
    if(res==Integer.MIN_VALUE && sign == 1) return Integer.MAX_VALUE;
    if(sign==1) return -res;
    else return res;
}
```