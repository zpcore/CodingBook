## Truncate Message
---

Our goal is to truncate a list of messages down to size max_log_messages. For the sake of this problem, our "fair" truncation algorithm is as follows: Let X be the max log messages maintained per client. For each client: If the client has emitted > X messages, truncate the log messages for that client to X messages. If the client has emitted <= X messages, do not truncate the log messages for that client.

The goal of this problem is to figure out the maximum value of x which causes the total number of messages retained across all clients to be <= max_log_messages. Write Findx, which takes the input list and max_log_messages and returns X.

Example: Suppose there are 5 log clients, and their number of messages is: (A, 50), (B, 20), (C, 100‌‍‌‍‌‍), (D, 50). (E, 400). Suppose we want to have no more than 300 total messages after truncation.

#### Solution (!untested!)
Solution 1: Sort and binary search. Time complexity: O(nlgn), n is the number of message.
```c++
using psi = pair<string, int>;
int truncateMessage(vector<psi> messages, int max_message) {
    int n = messages.size();
    sort(messages.begin(), messages.end(), [](const psi& p1, const psi& p2) {
        return p1.second < p2.second;
    });
    int psum = 0;
    for (int i=0; i<n; i++) {
        // if we retain until message[i]
        int x = messages[i].second;
        //       i     
        // . . . x . . .
        // . . . x x x x
        int tot = psum + x*(n-i);
        if (tot>=max_message) {
            // this means message[i:] should be trancated
            // find max x satisfy condition: psum + x*(n-i) <= max_message
            return (max_message-psum)/(n-i);
        }
        psum += messages[i].second;
    }
    return 0;
}
```

