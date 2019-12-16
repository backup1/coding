# Power and Sum

## Modular Power

```cpp
using ll = int64_t;
const ll mod = 1e9+7;
inline ll Pow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%mod;
    p >>= 1;
    cp = (cp*cp)%mod;
  }
  return ret;
}
```

## Modular Power / Inverse

```cpp
using ll = int64_t;
const ll mod = 1e9+7; // prime number
inline ll Pow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%mod;
    p >>= 1;
    cp = (cp*cp)%MOD;
  }
  return ret;
}
inline ll Inverse(ll a){
  return Pow(a,mod-2);
}
```

## Modular Sum

$$
x^n + x^{n-1} + \cdots + x^3 + x^2 + x + 1 = \frac{x^{n+1} - 1}{x - 1}
$$

```cpp
// O(ln^2 n) solution
inline ll Sum(ll x,ll n){
  if(n == 0) return 1;
  if(n == 1) return (x+1)%MOD;
  ll m = (n-1)/2;
  ll ans = Sum(x,m);
  ans = (ans*(1+Pow(x,m+1)))%mod;
  if(n%2 == 0) ans = (1+x*ans)%mod;
  return ans;
}
// O(ln n) solution - faster using formula
inline ll Sum(ll x,ll n){
  if(x%mod == 1) return n+1;
  return (Pow(x,n+1)-1)*Inverse(x-1)%mod;
}
```



