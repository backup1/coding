---
description: GCD who treats edge cases for ex. a =0, b = 0
---

# GCD and extended Euclid algorithm

```cpp
int gcd(int a, int b){
  if(a > b) swap(a,b);
  if(a == b) return a;
  if(a == 0) return b;
  return gcd(a,b%a);
}
```

Extended Euclid algorithm (modulo inverse)

ref. [https://www.geeksforgeeks.org/multiplicative-inverse-under-modulo-m/](https://www.geeksforgeeks.org/multiplicative-inverse-under-modulo-m/)

question. [https://atcoder.jp/contests/arc143/tasks/arc143\_b](https://atcoder.jp/contests/arc143/tasks/arc143\_b)

```python
n = int(input())
mod = 998244353
carre = [i*i for i in range(n+5)]
n2 = carre[n]
fact = [1 for _ in range(n2+5)]
invfact = [1 for _ in range(n2+5)]

def modInverse(a, m=mod):
  m0 = m
  y = 0
  x = 1
  if (m == 1):
    return 0
  while (a > 1):
    q = a // m
    t = m
    m = a % m
    a = t
    t = y
    y = x - q * y
    x = t
  if (x < 0):
    x = x + m0
  return x

for i in range(2,n2+5):
  fact[i] = (i*fact[i-1])%mod
  invfact[i] = modInverse(fact[i])

ret = fact[n2]
mul = (n2*fact[carre[n-1]])%mod
tmp = 0
for k in range(n,n2-n+2):
  tmp += fact[k-1]*invfact[k-n]*fact[n2-k]*invfact[n2-k-n+1]
  tmp %= mod
print((ret - mul*tmp)%mod)
```
