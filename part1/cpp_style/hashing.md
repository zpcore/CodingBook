# Hashing
#### Use pair as the key of hashmap/hashset
1) Define the hash function for pair:
```cpp
struct pair_hash {
    inline std::size_t operator()(const std::pair<int,int> & v) const {
        return v.first*31+v.second;
    }
};

std::unordered_set< std::pair<int, int>,  pair_hash> mySet;
```
This works, because pair<T1,T2> defines equality. For custom classes that do not provide a way to test equality you may need to provide a separate function to test if two instances are equal to each other.

2) Use boost library:
```cpp
#include <boost/functional/hash.hpp>
std::unordered_set< 
        pair<int, int>, 
        boost::hash< std::pair<int, int> > 
    > mySet;
```