## C++ Debug Output Template for CP

#### Template 1
Reference: https://discuss.codechef.com/t/trace-__f-__va_args__-__va_args__/19200
```c++
#define TRACE // enable debug print

#ifdef TRACE
#define trace(...) __f(#__VA_ARGS__, __VA_ARGS__)
template <typename Arg1>
void __f(const char* name, Arg1&& arg1){
  cerr << name << " : " << arg1 << std::endl;
}
template <typename Arg1, typename... Args>
void __f(const char* names, Arg1&& arg1, Args&&... args){
  const char* comma = strchr(names + 1, ',');cerr.write(names, comma - names) << " : " << arg1<<" | ";__f(comma+1, args...);
}
#else
#define trace(...)
#endif
```

#### Template 2 (able to handle pair, vector)
```c++
// i : 9 |  pair : (9, 19) |  vec : {1, 2, 3}
string to_string(string s) { return '"' + s + '"'; }
string to_string(const char *s) { return to_string((string) s); }
string to_string(bool b) { return (b ? "true" : "false"); }

template<typename A, typename B>
string to_string(pair<A, B> p) { return "(" + to_string(p.first) + ", " + to_string(p.second) + ")"; }

template<typename A>
string to_string(A v) {
    bool first = true;
    string res = "{";
    for(const auto &x : v) {
        if(!first) { res += ", "; }
        first = false;
        res += to_string(x);
    }
    res += "}";
    return res;
}

#define TRACE // enable debug print

#ifdef TRACE
#define trace(...) __f(#__VA_ARGS__, __VA_ARGS__)
template <typename Arg1>
void __f(const char* name, Arg1&& arg1){
  cerr << name << " : " << to_string(arg1) << std::endl;
}
template <typename Arg1, typename... Args>
void __f(const char* names, Arg1&& arg1, Args&&... args){
  const char* comma = strchr(names + 1, ',');cerr.write(names, comma - names) << " : " << to_string(arg1)<<" | ";__f(comma+1, args...);
}
#else
#define trace(...)
#endif
```


#### Template 3 (able to handle pair, vector)
```c++
// [i, pair, vec]: 9 (9, 19) {1, 2, 3}
string to_string(string s) { return '"' + s + '"'; }
string to_string(const char *s) { return to_string((string) s); }
string to_string(bool b) { return (b ? "true" : "false"); }

template<typename A, typename B>
string to_string(pair<A, B> p) { return "(" + to_string(p.first) + ", " + to_string(p.second) + ")"; }

template<typename A>
string to_string(A v) {
    bool first = true;
    string res = "{";
    for(const auto &x : v) {
        if(!first) { res += ", "; }
        first = false;
        res += to_string(x);
    }
    res += "}";
    return res;
}

void debug_out() { cerr << endl; }

template<typename Head, typename... Tail>
void debug_out(Head H, Tail... T) {
    cerr << " " << to_string(H);
    debug_out(T...);
}

#define TRACE // enable debug print

#ifdef TRACE
#define trace(...) cerr << "[" << #__VA_ARGS__ << "]:", debug_out(__VA_ARGS__)
#else
#define trace(...) do {} while(0)
#endif
```