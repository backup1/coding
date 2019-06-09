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

