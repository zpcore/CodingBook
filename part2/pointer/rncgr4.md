## Read N Characters Given Read4
Leetcode 157
```java
/* The read4 API is defined in the parent class Reader4.
      int read4(char[] buf); */
public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Maximum number of characters to read
     * @return    The number of characters read
     */
    public int read(char[] buf, int n) {    
        int k = 4;
        int tot = 0;
        char[] tmp = new char[4];
        while(k==4) {
            k = read4(tmp);            
            for(int i=0;i<Math.min(k,n);i++) {
                buf[tot++] = tmp[i];
            }
            n -= k;
            if(n<=0) break;
        }
        return tot;
    }
}

```

---
Call multiple times:
```java
public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Maximum number of characters to read
     * @return    The number of characters read
     */
    char[] buffer = new char[4];
    int bPtr = 0;  // pointer points to where we should read in the buffer
    int bCnt = 0; // data remained in the buffer
    
    public int read(char[] buf, int n) {
        int tot = 0;
        while(tot<n) {
            if(bCnt==0) {
                bCnt = read4(buffer);
            }
            if(bCnt == 0) break; // no more data to read (n > data in the system)
            while(bCnt>0 && tot<n) {
                buf[tot++] = buffer[bPtr++];
                bCnt--;
            }
            if(bCnt==0) bPtr=0;
        }
        return tot;
    }
}
```