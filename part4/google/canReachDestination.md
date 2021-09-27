## Train Station
---

Given a list of schedule of train. Each schedule tells a train will departure from station "A" at time "startTime" and end at destination station "B" at "endTime". A person wants to start from "start" at time "T1" and reach at station "destination" by transferring through several trains. Return the earliest time he can reach the destination. Assume once he reach a station, he can immediately transfer to another train. 

(Original question asks given a deadline, whether he can reach the destination by the deadline.)

Definition of Schedule:
```c++
class Schedule {
    int startTime;
    string A;
    int endTime;
    string B;
};
```

#### Solution (!untested!)
Use Dijkstra to find the min time arrival for each station. 
```c++
using pis = pair<int, string>;
int earliestTimeArrival(vector<Schedule> schedules, int T1, string start, string destination) {
    unordered_map<string, vector<Schedule>> g; // start point -> list of schedule start at this point
    priority_queue<pis, vector<pis>, greater<pis>()> pq; // time, stop
    unordered_map<string, int> minArrival; // min distance map
    minArrival[start] = T1;
    pq.push({T1, start});
    while(!pq.empty()) {
        auto [dis, cur] = pq.top();
        pq.pop();
        if (cur==destination)
            break;
        for (auto s: g[cur]) {
            auto& [st, a, et, b] = s;
            if (cur > st) // the train has left before current time
                continue;
            // update the earliest time arrive at station b
            if (minArrival.find(b)!=minArrival.end() && minArrival[b]<=cur)
                continue;
            minArrival[b] = et;
            pq.push({b, et});
        }
    }
    return minArrival.find(destination)==minArrival.end()? -1: minArrival[destination];
}
```








