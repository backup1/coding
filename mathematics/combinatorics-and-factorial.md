# Combinatorial and Factorial

## Factorial and its modular reverse

```cpp
inline ll getPow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%MOD;
    p >>= 1;
    cp = (cp*cp)%MOD;
  }
  return ret;
}
inline ll getInverse(ll a){
  return getPow(a,MOD-2)%MOD;
}
vector<ll> fact(200001,1),factinv(200001,1);
inline void init(ll l){
  for(ll i = 1; i < l; ++i){
    fact[i] = (fact[i-1]*i)%MOD;
    factinv[i] = (factinv[i-1]*getInverse(i))%MOD;
  }
}
```

## Combinatorial

$$
C_{a+b}^a = \frac{(a+b)!}{a! \times b!} = C_{a+b}^b
$$

```cpp
ll C(ll a,ll b){
  ll ans = fact[a+b];
  ans = (ans*factinv[a])%MOD;
  ans = (ans*factinv[b])%MOD;
  return ans;
}
```

