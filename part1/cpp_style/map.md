## Map (focus on BST)
---

```c++
map<int,int> m; // arranged by key
```

#### Find the next element greater/equal than/to (>=) k
```c++
map<int,int>::iterator it = m.lower_bound(k);
// if (it==m.end()): not found
// else result: it->first
```

#### Find the next element greater than (>) k
```c++
map<int,int>::iterator it = m.upper_bound(k);
// if (it==m.end()): not found
// else result: it->first
```

#### Find the next element smaller than (<) k
```c++
map<int,int>::iterator it = --m.lower_bound(k);
// if (it==m.begin()): not found
// else result: it->first
```


#### Template
```c++
template < typename Key, typename Value, typename Compare, typename Alloc >
auto lowerEntry(std::map<Key, Value, Compare, Alloc> map, Key key) -> decltype(map.begin()) {
    auto iter = map.lower_bound(key); // find the first element to go at or after key
    if(iter == map.begin()) // all elements go after key
        return map.end(); // signals that no lowerEntry could be found
    return --iter; // pre-decrement to return an iterator to the element before iter
}
```