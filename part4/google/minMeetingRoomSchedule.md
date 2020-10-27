## Minimal Meeting Room Schedule

#### Question
Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), find the minimum number of conference rooms required.
As a follow-up of the question minimal meeting room. We need to output with the minimal number of the meeting rooms, assign each meeting to the corresponding room. There can be multiple schedule assignment, we only need one solution.
ß
#### Solution
```c++
auto comp = [](const vector<int>& v1, const vector<int>& v2) {
    return v1[0]>v2[0];
};

vector<vector<vector<int>>> minMeetingRoomSchedule(vector<vector<int>>& intervals) {
    vector<vector<vector<int>>> ret;
    priority_queue<vector<int>, vector<vector<int>>, decltype(comp)> pq_room(comp); // <latest meeting end time, room id>1
    sort(intervals.begin(), intervals.end()); // sort the interval based on the start time, the end time doesn't matter
    for (auto& v: intervals) {
        int room_id = 0;
        if (!pq_room.empty() && pq_room.top()[0]<=v[0]) { // the room with the earliest end time has finished before meeting v
            room_id = pq_room.top()[1];
            pq_room.pop();
        } else { // need a new room
            room_id = pq_room.size();
            ret.push_back({}); // expand ret 
        }
        pq_room.push({v[1], room_id});
        ret[room_id].push_back(v);
    }
    return ret;
}
```
