# DP on Array

## Questions

{% embed url="https://codeforces.com/problemset/problem/467/C" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  ll n,m,k;
  cin >> n >> m >> k;
  vector<ll> p(n+1),val(n+1);
  for(ll i = 1; i <= n; ++i) cin >> p[i];
  while(k--){
    vector<ll> val2(n+1);
    ll tmp = 0;
    for(ll i = 1; i < m; ++i) tmp += p[i];
    for(ll i = m; i <= n; ++i){
      tmp += p[i];
      val2[i] = max(tmp+val[i-m],val2[i-1]);
      tmp -= p[i-m+1];
    }
    val = val2;
  }
  cout << val.back();
  return 0;
}
```

