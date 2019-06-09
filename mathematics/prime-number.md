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
void sieve(){
	unordered_map<int,int> prime_pos;
	set<int> prime;
	int n=2750131,idx=1;
	for (int i=2; i<=n; ++i){
		if (vu[i]){
			for (int j=2*i; j<=n; j += i) vu[j]=true;
			prime_pos[i]=idx;
			s.emplace(i);
			++idx;
		}
	}
}
```

