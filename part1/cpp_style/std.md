##Std
---

iota
```c++
iota(vec.begin(), vec.end(), 100);
// vec = 100, 101, 102, ...
```

partial_sum
```c++
// vec = 1, 2, 3, 4
partial_sum(vec.begin(), vec.end(), vec.begin()); // 1, 3, 6, 10
partial_sum(vec.rbegin(), vec.rend(), vec.rbegin()); // 10, 9, 7, 4
```

acccumulate
```c++
//vector<vecctor<int>> vec = {{1,2}, {3,4}, {4,5}};
int s = accumulate(vec.begin(), vec.end(), 0, [](int init, const std::vector<int>& v){
    return init + v[1]; 
}); // 2+4+5
```


