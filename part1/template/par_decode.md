## Parenthesis Decoding

#### This template targets string decoding with parenthesis.
E.g., (\*b+c\*(d+f))\*(e-f).

```c++
template<typename T>
T dfs(const string& s, int& pos) {
    T ta;
    while(pos<s.size() && s[pos]!=')') {
        if (s[i]=='(') {
            T tb = dfs(s, ++pos); // dive into the next level
            // TODO: Handle tb into ta
        } else {
            // TODO: Same level operator operation
        }
    }
    pos++;
    return ta;
}

// Main entry
template<typename T> 
T exp_decode(const string& s) {
    int pos = 0
    return dfs(s, pos);
}


```

