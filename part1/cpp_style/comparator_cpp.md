####Comparator for priority_queue
1) int
```cpp
std::priority_queue<int,std::vector<int>,std::less<int>> pq; //max-heap (top is max)
std::priority_queue<int,std::vector<int>,std::greater<int>> pq; //min-heap
```

2) Object (class comparator)
```cpp
struct Comparator{ // or class
    bool operator()(T t1, T t2) {
        return t1.val<t2.val;
    }
}; // max-heap
priority_queue<T,vector<T>,Comparator> pq;
```
3) Object (lambda exp comparator)
```cpp
auto compare = [](T t1,T t2) {
    return t1-t2;
};
priority_queue<T,vector<T>,decltype(compare)> pq(compare);
```

#### Sort Object
```cpp
// lambda expression
sort(T.begin(),T.end(),[](const auto& t1, const auto& t2){
            return t1.val<t2.val;
        });

sort(T.begin(),T.end(),[](const T& t1, const T& t2){
            return t1.val<t2.val;
        });
```