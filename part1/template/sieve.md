##Sieve of Eratosthenes
---

Find all the prime numbers <= n.

```c++
vector<int> isPrime(n+1, 1);
for (int i=2; i*i<=n; i++) {
    if (!isPrime[i])
        continue;
    for (int j=i*i; j<=n; j+=i) {
        isPrime[j] = 0;
    }
}
```