## Bitwise Operation


#### Count 1 (GCC compiler)
```c++
__builtin_popcount(x); 
__builtin_popcountl(x); //  for long type
__builtin_popcountll(x); // for long long type
```

#### Count leading or trailing zeros in **position** number (GCC compiler)
(also has -l and -ll for long and long long)
```c++
__builtin_clz(x); // leading zeros
__builtin_ctz(x); // trailing zeros
```

#### Check parity of a number (GCC compiler)
Parity of a number refers to whether it contains an odd or even number of 1-bits.
This function returns true(1) if the number has odd parity else it returns false(0) for even parity.
```c++
__builtin_parity(n);
```

#### Bitset
```c++
#define MAX_W 100
bitset<MAX_W> bs;

// construct bitset from int or string:
bitset<MAX_W> a(17);
bitset<MAX_W> a("1010");
```

#### Return lowest bit of 1
```c++
m & (-m)
```

#### Turn off the lowest bit that was set to 1
```c++
m & (m-1)
```
E.g., m=26 (11010) -> 24(11000)