###KMP
---
Does pattern P exists in S?  
```c++
int kmp(const string &S, const string &P) {
    if (P.empty()) return 0;

    vector<int> tab(P.size(), 0);
    for (int i = 1, j = 0; i < P.size(); ++i) {
        while (j && P[j] != P[i]) j = tab[j - 1];
        if (P[j] == P[i]) ++j;
        tab[i] = j;
    }

    for (int i = 0, j = 0; i < S.size(); ++i) {
        while (j && P[j] != S[i]) j = tab[j - 1];
        if (P[j] == S[i]) ++j;
        if (j == P.size()) {
            return i - j + 1;
            /* below is the code to get all the pos
            res.push_back(i-k+1);
            k = tab[k-1];
            */
        }
    }

    return -1;
}
```

```c++
int kmp(const string& S, const string& P) {
    vector<int> tab(P.size() + 1); // 1-based to avoid extra checks.
    for (auto i = 1, j = 0; i < P.size(); )
        if (P[j] == P[i]) 
            tab[++i] = ++j;
        else if (j == 0) 
            tab[++i] = j;
        else 
            j = tab[j];
    for (auto i = 0, j = 0; i < S.size(); i += max(1, j - tab[j]), j = tab[j]) {
        while (j < P.size() && S[i+j] == P[j]) ++j;
        if (j == P.size()) 
            return i;
    }
    return -1;
}
```