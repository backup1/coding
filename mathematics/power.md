# Power

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

