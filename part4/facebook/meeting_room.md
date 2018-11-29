## Meeing Room (follow up Leetcode 253)
---
#### Code Link
[Link](https://github.com/zpcore/CodingProblem/blob/master/MinMeetingRoom.java)


#### Question
Follow up 253, assign the meeting to each room. (output one result is fine.)


#### Solution
When we check a new meeting interval, there are two cases:
1. Open a new room
2. Use an empty meeting room
Notes: 
We use deque to record which room is currently empty.  
Use hashmap (meeting interval -> room number), so we know when a meeting end, which room will be released (empty).  


```java
class Interval {
	int start;
	int end;
	Interval(int start, int end) {
		this.start = start;
		this.end = end;
	}
}

public List<List<Interval>> minMeetingRoom(Interval[] intervals) {
	Map<Interval,Integer> hm = new HashMap<>(); // map interval to the meeting room
	Arrays.sort(intervals,(a,b)->a.start-b.start);
	List<List<Interval>> res = new ArrayList<>();
	PriorityQueue<Interval> pq = new PriorityQueue<>((a,b)->a.end-b.end);
	Deque<Integer> emptyRoom = new ArrayDeque<>();
	for(Interval interval:intervals) {
		while(!pq.isEmpty() && pq.peek().end<=interval.start) {
			int room = hm.get(pq.poll());
			emptyRoom.add(room);
		}
		pq.add(interval);
		if(res.size()<pq.size()) { // case 1. add new room
			hm.put(interval,res.size());
			List<Interval> newOpenRoom = new ArrayList<>();
			newOpenRoom.add(interval); 
			res.add(newOpenRoom);
		}else { // case 2. use previous empty room (any number in newEmptyRoom is fine)
			int targetRoom = emptyRoom.pollFirst(); // just use the first empty room
			res.get(targetRoom).add(interval);
			hm.put(interval,targetRoom);
		}
	}
	return res;
}
```