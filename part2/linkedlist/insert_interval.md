## Intert Interval (LC 57)
---

```java
// case 1: add org
// org: ----
// new:       ==
// case 2: add new, add org
// org:      ----
// new:  ==
// case 3: add nothing, revise new 
// org:  -------...
// new:    =====...
// key point: use newInterval to cover original interval, modify the new interval
public List<Interval> insert(List<Interval> intervals, Interval newI) {
    List<Interval> res = new ArrayList<>();
    for(Interval i: intervals) {
        if(newI==null || i.end < newI.start) { //newInterval has been merged or newInteval is not come yet
            res.add(i);
        }else if(newI.end<i.start){//newInterval should be added, current interval should be add
            res.add(newI);
            newI = null;
            res.add(i);
        }else { // newInterval is not end yet, keep searching next
            newI.start = Math.min(newI.start, i.start);
            newI.end = Math.max(newI.end,i.end);
        }
    }
    if(newI!=null) res.add(newI);
    return res;
}
```

#### Similar Question: Merge Intervals (LC 56)
```java
public List<Interval> merge(List<Interval> intervals) {
    Collections.sort(intervals,(a,b)->a.start-b.start);
    List<Interval> res = new ArrayList<>();
    Interval lastI = null;
    for(Interval i:intervals) {
        if(lastI == null) {
            lastI = i;                
        }else if(lastI.end<i.start) {
            res.add(lastI);
            lastI = i;
        }else {
            lastI.end = Math.max(i.end,lastI.end);
        }
    }
    if(lastI != null) res.add(lastI);
    return res;
    
}
```