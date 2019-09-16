# Normalization \(vector space basis\)

Kind of greedy using vector space basis normalization.

{% embed url="https://atcoder.jp/contests/abc141/tasks/abc141\_f" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = uint64_t;
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  ll n,tot1 = 0;
  cin >> n;
  vector<ll> a(n);
  for(ll i = 0; i < n; ++i){
    cin >> a[i];
    tot1 ^= a[i];
  }
  for(ll i = 0; i < n; ++i) a[i] &= ~tot1;
  ll rank = 0;
  for(int i = 59; i >= 0; --i){
    ll idx = n;
    for(ll j = rank; j < n; ++j){
      if(a[j]&(1ll<<i)){
        idx = j;
        break;
      }
    }
    if(idx == n) continue;
    if(idx > rank) a[rank] ^= a[idx];
    for(ll j = rank+1; j < n; ++j){
      a[j] = min(a[j],a[j]^a[rank]);
    }
    ++rank;
  }
  ll tot2 = 0;
  for(ll i = 0; i < n; ++i) tot2 = max(tot2,tot2^a[i]);
  cout << tot1+2*tot2;
  return 0;
}
```

