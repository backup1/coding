# Prime Number

```cpp
using ll = long long;
map<ll,ll> pidx;
set<ll> primes;
void init(ll N){
  for(ll i = 2; i <= N; ++i){
    bool ok = true;
    for(ll j : primes){
      if(j*j > i) break;
      if(i%j == 0){
        ok = false;
	break;
      }
    }
    if(ok){
      primes.insert(i);
      pidx[i] = primes.size();
    }
  }
}
```

## Sieve of Eratosthenes in nloglogn time

```cpp
unordered_map<ll,ll> prime_pos;
set<ll> prime;
void sieve(ll n){
  ll idx = 1;
  vector<bool> vu(n+1,true);
  vu[0] = false;
  vu[1] = false;
  for(ll i = 2; i <= n; ++i){
    if(vu[i]){
      for(int j = (i<<1); j <= n; j += i) vu[j] = false;
      prime_pos[i] = idx;
      s.emplace(i);
      ++idx;
    }
  }
}
```

## Primitive Root \(Modular Prime Number\)

[https://cp-algorithms.com/algebra/primitive-root.html](https://cp-algorithms.com/algebra/primitive-root.html)

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
inline ll PrimitiveRoot(){
  vector<ll> fact;
  ll phi = mod-1, n = phi;
  for(ll i = 2; i*i <= n; ++i){
    if(n%i == 0){
      fact.push_back (i);
      while(n%i == 0) n /= i;
    }
  }
  if(n > 1) fact.push_back(n);
  for(ll res = 2; res <= mod; ++res){
    bool ok = true;
    for(ll i = 0; i < fact.size() and ok; ++i){
      ok &= (Pow(res,phi/fact[i]) != 1);
    }
    if(ok) return res;
  }
  return -1;
}
```

