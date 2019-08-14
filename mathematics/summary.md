# Summary

## Useful Code Snippets

```cpp
const ll MOD = 1e9+7;
const ll FACN = 200005;
vector<ll> fact(FACN,1),factinv(FACN,1);
ll getPow(ll a,ll p){ // a^p % MOD
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%MOD;
    p >>= 1;
    cp = (cp*cp)%MOD;
  }
  return ret;
}
ll getInverse(ll a){ // a^{-1} % MOD
  return getPow(a,MOD-2)%MOD;
}
// fact[n] = n! % MOD, factinv[n] = n!^{-1} % MOD
void init(ll l){
  for(ll i = 1; i < l; ++i){
    fact[i] = (fact[i-1]*i)%MOD;
    factinv[i] = (factinv[i-1]*getInverse(i))%MOD;
  }
}
ll C2(ll a,ll b){ // C_{a+b}^a, choose a among a+b
  ll ans = fact[a+b];
  ans = (ans*factinv[a])%MOD;
  ans = (ans*factinv[b])%MOD;
  return ans;
}
ll C(ll n,ll a){ // C_n^a, choose a among n
  return C2(a,n-a);
}
```

