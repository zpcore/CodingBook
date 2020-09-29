### String
---

#### 1. Erase all the occurrences of char (e.g., 'a') in a string:  
```c++
#include <algorithm>
str.erase(std::remove(str.begin(), str.end(), 'a'), str.end());
```

#### 2. Find all the substring p in s:
```c++
for(size_t i = 0; (i = s.find(p,i)) != string::npos; ++i) {
    pos.emplace_back(i);
}
```