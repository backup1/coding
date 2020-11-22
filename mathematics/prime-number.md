# Prime Number

{% tabs %}
{% tab title="C++" %}
```cpp
#include <bits/stdc++.h>
#include <ext/hash_map>
#include <ext/hash_set>
using namespace std;
using namespace __gnu_cxx;
using ll = long long;
const ll SZ = 2e5+5;
hash_map<int,int> pidx;
hash_set<int> primeset;
vector<ll> primes;
inline void init(){
  for(ll i = 2; i <= SZ; ++i){
    bool ok = true;
    for(ll j : primes){
      if(j*j > i) break;
      if(i%j == 0){
        ok = false;
        break;
      }
    }
    if(ok){
      primeset.insert((int) i);
      primes.push_back(i);
      pidx[i] = primes.size();
    }
  }
}
```
{% endtab %}

{% tab title="Compilation" %}
```bash
$ g++ test.cpp -std=c++14 -Wno-deprecated
```
{% endtab %}
{% endtabs %}

## Sieve of Eratosthenes in O\(n ln ln n\) time

```cpp
#include <bits/stdc++.h>
#include <ext/hash_map>
#include <ext/hash_set>
using namespace std;
using namespace __gnu_cxx;
using ll = long long;
const ll SZ = 2e5+5;
hash_map<int,int> pidx;
hash_set<int> primeset;
vector<ll> primes;
inline void sieve(){
  ll idx = 1;
  vector<bool> vu(SZ,true);
  vu[0] = false;
  vu[1] = false;
  for(ll i = 2; i < SZ; ++i){
    if(vu[i]){
      for(int j = (i<<1); j < SZ; j += i){
        vu[j] = false;
      }
      pidx[i] = idx;
      primeset.insert((int) i);
      primes.push_back(i);
      ++idx;
    }
  }
}
```

## linear sieve

ref. [https://codeforces.com/blog/entry/54090](https://codeforces.com/blog/entry/54090)

```cpp
std::vector <int> prime;
bool is_composite[MAXN];

void sieve (int n) {
	std::fill (is_composite, is_composite + n, false);
	for (int i = 2; i < n; ++i) {
		if (!is_composite[i]) prime.push_back (i);
		for (int j = 0; j < prime.size () && i * prime[j] < n; ++j) {
			is_composite[i * prime[j]] = true;
			if (i % prime[j] == 0) break;
		}
	}
}
```

