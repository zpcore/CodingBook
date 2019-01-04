## Minimum Water Level Cannot Pass
---
#### Question
Given a 2D char matrix containing number, char 'S' and char 'E'. The number represents the height of the land. The land can be covered by water if its height is less than the water level. Find the minimum water level that prevent a feasible walking path from start point 'S' to end point 'E'.
```
Example:  
[ [S,3,1]
  [2,1,3]
  [2,1,E] ]
Return 2
```
land height is in range [0,10^9];  
matrix row and column are in range [2,1000].  

#### Solution
Method 1. Try all possible water levels. Check whether it is reachable from 'S' to 'E' using DFS. Time complecity is O((MN)^ 2), assuming the matrix size is M*N.  
Method 2. Best first search. Time complexity(O(MN)).
```java
class Node {
	int x = 0;
	int y = 0;
	int val = 0;
	Node(int x, int y, int val) {
		this.x = x;
		this.y = y;
		this.val = val;
	}
}

private final int[][] dir = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
public int minWaterLevel(char[][] height) {
	int m = height.length;
	int n = height[0].length;
	int min = Integer.MAX_VALUE;
	Queue<Node> pq = new PriorityQueue<>((a,b)->b.val-a.val);
	Set<String> visited = new HashSet<>();
	pq.add(new Node(0,0,Integer.MAX_VALUE));
	visited.add(String.valueOf(0)+","+String.valueOf(0));
	while(true) {
		Node curNode = pq.poll();
		System.out.println(curNode.x+","+curNode.y);
		min = Math.min(min,curNode.val);
		int x = curNode.x;
		int y = curNode.y;
		for(int[] i:dir) {
			if(x+i[0]<0 || x+i[0]>=m || y+i[1]<0 || y+i[1]>=n) continue;
			String tmp = String.valueOf(x+i[0])+","+String.valueOf(y+i[1]);
			if(!visited.contains(tmp)) {
				if(height[x+i[0]][y+i[1]]=='E') return min+1;
				pq.add(new Node(x+i[0],y+i[1],height[x+i[0]][y+i[1]]-'0'));
				visited.add(tmp);
			}
		}
		
	}
}
```