# Sqrt Decomposition

{% embed url="https://www.spoj.com/problems/GIVEAWAY/" %}

{% embed url="https://www.codechef.com/problems/GIVEAWAY" %}

Please be aware that the 2 questions above are the same, and all concerned numbers are pair-wisely distinct.

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
const ll N = 5e5+5;
ll dn[N];
vector<ll> v[710];
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  ll n;
  cin >> n;
  ll sqrtn = sqrt(n);
  vector<ll> x(n);
  for(ll i = 0; i < n; ++i){
    cin >> x[i];
    dn[i] = i/sqrtn;
    v[dn[i]].emplace_back(x[i]);
  }
  for(ll i = 0; i <= sqrtn; ++i) sort(begin(v[i]),end(v[i]));
  ll q;
  cin >> q;
  while(q--){
    ll cmd,a,b,c;
    cin >> cmd;
    if(cmd == 0){
      cin >> a >> b >> c;
      --a;
      --b;
      ll ans = 0;
      if(dn[a] == dn[b]){
        for(ll i = a; i <= b; ++i){
          if(x[i] >= c) ++ans;
        }
      }
      else{
        for(ll i = a; i < (dn[a]+1)*sqrtn; ++i){
          if(x[i] >= c) ++ans;
        }
        for(ll i = dn[a]+1; i < dn[b]; ++i){
          ll pos = lower_bound(begin(v[i]),end(v[i]),c) - begin(v[i]);
          ans += sqrtn-pos;
        }
        for(ll i = dn[b]*sqrtn; i <= b; ++i){
          if(x[i] >= c) ++ans;
        }
      }
      cout << ans << '\n';
    }
    else{
      cin >> a >> b;
      --a;
      ll na = dn[a];
      ll pos = lower_bound(begin(v[na]),end(v[na]),x[a]) - begin(v[na]);
      x[a] = b;
      v[na][pos] = b;
      sort(begin(v[na]),end(v[na]));
    }
  }
  return 0;
}
```

{% embed url="https://codeforces.com/contest/1207/problem/F" %}



