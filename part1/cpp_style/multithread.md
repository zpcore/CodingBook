## Multithread

### Setup certain number of thread
(https://leetcode.com/problems/web-crawler-multithreaded/discuss/530382/C%2B%2B)
```c++
#define THREAD_NUM 5 // number of thread to run

std::queue<Arg> q;
mutex mtx; 
condition_variable cv;
int waiting; // record the number of thread that is waiting due to q empty

void init(Arg start_arg) {
    wait = 0;
    std::vector<std::thread> threads;
    q.push(start_arg);
    unique_lock<mutex> locker(mtx);
    for (int i=0; i<THREAD_NUM; i++)
        threads.emplace_back(one_thread, arg);
    locker.unlock();
    for (int i=0; i<THREAD_NUM; i++)
        threads[i].join();
}

void one_thread() {
    while (true){
        unique_lock<mutex> locker(mtx);
        waiting ++;
        // if q is not empty, then we start directly.
        // if q is empty, then we wait for unfinished thread.
        cv.wait(locker, []{return !q.empty() || (q.empty() && waiting == THREAD_NUM);});
        if (q.empty())
            return;
        waiting --;
        Arg arg = q.front();
        q.pop();
        locker.unlock();
        // operation based on arg.
        //... generate several arg in arg_set
        for (Arg argx: arg_set)
            q.push_back(argx);
        cv.notify_all();
    }
}

```