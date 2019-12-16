# Combinatorial and Factorial

## Factorial and its modular reverse

```cpp
using ll = int64_t;
const ll SZ = 2e5+5;
inline ll Pow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%mod;
    p >>= 1;
    cp = (cp*cp)%mod;
  }
  return ret;
}
inline ll Inverse(ll a){
  return Pow(a,mod-2);
}
vector<ll> fact(SZ,1),factinv(SZ,1);
inline void init(ll l){
  for(ll i = 1; i < l; ++i){
    fact[i] = (fact[i-1]*i)%mod;
    factinv[i] = (factinv[i-1]*Inverse(i))%mod;
  }
}
```

## Combinatorial

$$
C_n^a = \frac{n!}{a! \times (n-a)!} = C_n^{n-a}
$$

```cpp
using ll = int64_t;
const ll SZ = 2e5+5;
inline ll Pow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%mod;
    p >>= 1;
    cp = (cp*cp)%mod;
  }
  return ret;
}
inline ll Inverse(ll a){
  return Pow(a,mod-2);
}
vector<ll> fact(SZ,1),factinv(SZ,1);
inline void init(ll l){
  for(ll i = 1; i < l; ++i){
    fact[i] = (fact[i-1]*i)%mod;
    factinv[i] = (factinv[i-1]*Inverse(i))%mod;
  }
}
inline ll C(ll n,ll a){
  if(a < 0 or a > n) return 0;
  ll ans = fact[n];
  ans = (ans*factinv[a])%mod;
  ans = (ans*factinv[n-a])%mod;
  return ans;
}
```

