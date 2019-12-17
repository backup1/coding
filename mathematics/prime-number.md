# Prime Number

```cpp
#include <ext/hash_map>
#include <ext/hash_set>
using namespace __gnu_cxx;
using ll = long long;
hash_map<ll,ll> pidx;
hash_set<ll> primeset;
vector<ll> primes;
inline void init(ll N){
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
      primeset.insert(i);
      primes.push_back(i);
      pidx[i] = primes.size();
    }
  }
}
```

## Sieve of Eratosthenes in O\(n ln ln n\) time

```cpp
#include <ext/hash_map>
#include <ext/hash_set>
using namespace __gnu_cxx;
using ll = long long;
hash_map<ll,ll> pidx;
hash_set<ll> primeset;
vector<ll> primes;
inline void sieve(ll n){
  ll idx = 1;
  vector<bool> vu(n+1,true);
  vu[0] = false;
  vu[1] = false;
  for(ll i = 2; i <= n; ++i){
    if(vu[i]){
      for(int j = (i<<1); j <= n; j += i){
        vu[j] = false;
      }
      prime_pos[i] = idx;
      primeset.emplace(i);
      primes.push_back(i);
      ++idx;
    }
  }
}
```

