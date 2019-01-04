## Spiral Matrix (leetcode 55)
***
#### Question
Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

Example 1:
```c
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```

Example 2:
```c
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

#### Note
For the matrix that is not the square, we should consider the case to not duplicate output number. Below is the four for loop in the first recursion:
```c
  [+1,+2,+3,+4]      [1, 2, 3,  4]     [ 1,  2,  3, 4]     [ 1, 2, 3, 4]
  [ 5, 6, 7, 8]  --> [5, 6, 7,+ 8] --> [ 5,  6,  7, 8] --> [+5, 6, 7, 8]
  [ 9,10,11,12]      [9,10,11,+12]     [+9,+10,+11,12]     [ 9,10,11,12]
```
(make sure the first two for loop covers the first row and last column, the remaining two for loop has extra termination condition.)


#### Solution
```java
public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList();
        if(matrix==null||matrix.length==0||matrix[0].length==0) return res;
        int m = matrix.length; // row
        int n = matrix[0].length; //column
        helper(matrix,res,0,n,m);
        return res;
    }
    
    public void helper(int[][] matrix, List<Integer> res, int level, int horLen, int verLen) {
        if(horLen<=0 || verLen<=0)
            return;

        for(int i=level;i<level+horLen;i++)
            res.add(matrix[level][i]);

        for(int i=level+1;i<level+verLen;i++)
            res.add(matrix[i][level+horLen-1]);

        for(int i=level+horLen-2; verLen>1 && i>=level; i--) // verLen>1
            res.add(matrix[level+verLen-1][i]);

        for(int i=level+verLen-2; horLen>1 && i>level; i--) // horLen>1
            res.add(matrix[i][level]);

        helper(matrix,res,level+1,horLen-2,verLen-2);
    }
```