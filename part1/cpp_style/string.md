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

#### 3. Split the string based on pattern
```c++
vector<string> split(const string &str, const string& pattern) {
    vector<string> tokens;

    string::size_type start = 0;
    string::size_type end = 0;

    while ((end = str.find(pattern, start)) != string::npos) {
        tokens.push_back(str.substr(start, end - start));
        start = end + pattern.size();
    }

    tokens.push_back(str.substr(start));

    return tokens;
}

```