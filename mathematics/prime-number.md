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

