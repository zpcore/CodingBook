## Token/Bucket
***
#### How to write token  
```java
// get the token id for number i
// token width is w
private int getID(long i, long w) {
    return i < 0 ? (i + 1) / w - 1 : i / w;
}

Map<Integer, Integer> map = new HashMap<>();
int m = getID(nums[i], w);//get token id
map.put(m, nums[i]); //map toekn tag to content
```